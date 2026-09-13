# POI Exploration API Contract

Status: **APPROVED FOR IMPLEMENTATION PLANNING - 2026-09-12**

Jira: TM-58

Use case: UC-12 Explore Points of Interest

This document defines the approved backend contract for public POI list and detail reads. It is
approved together with `requirements/mvp/explore-points-of-interest-mvp.md`. Typed success DTOs and
RFC 7807 errors follow the canonical decision in `api/contracts/api-response-standard.md`.

# Endpoints

| Operation | Method and route | Authorization | Success |
|---|---|---|---|
| Explore POIs | `GET /api/v1/pois` | Anonymous/public | `200 OK` paginated DTO |
| View POI detail | `GET /api/v1/pois/{id}` | Anonymous/public | `200 OK` detail DTO |

# List Request

| Query parameter | Type | Default | Validation and behavior |
|---|---|---|---|
| `search` | string nullable | `null` | Trim before use; at most 200 characters after trimming; case-insensitive contains match on POI name. |
| `categoryId` | integer nullable | `null` | Greater than zero when supplied; unknown ID returns an empty page. |
| `originLatitude` | decimal nullable | `null` | Supply with longitude; range `-90..90`; normalize to six decimal places. |
| `originLongitude` | decimal nullable | `null` | Supply with latitude; range `-180..180`; normalize to six decimal places. |
| `maxDistanceKm` | decimal nullable | `null` | Greater than zero and requires both origin coordinates. |
| `openNow` | boolean | `false` | When true, keep only POIs open under the approved Vietnam-time rule. |
| `sort` | string | `name` | One of `name`, `distance`, `rating`; distance requires origin coordinates. |
| `page` | integer | `1` | Greater than zero. |
| `pageSize` | integer | `20` | Range `1..100`. |

Supplying an origin without `maxDistanceKm` is valid: the response includes distance but does not
apply a radius filter. Supplying `maxDistanceKm` without both coordinates is invalid.

# Sorting

- `name`: POI name ascending, then POI ID ascending.
- `distance`: unrounded distance ascending, then POI name and ID ascending.
- `rating`: rated POIs first, average rating descending, then review count descending, POI name
  ascending, and POI ID ascending.

The final POI ID tie-breaker makes pagination deterministic. The database query applies filtering,
sorting, and pagination before materialization.

# List Success DTO

```json
{
  "page": 1,
  "pageSize": 20,
  "totalCount": 1,
  "totalPages": 1,
  "items": [
    {
      "id": 42,
      "name": "My Khe Beach",
      "categoryId": 3,
      "categoryName": "Beach",
      "latitude": 16.061000,
      "longitude": 108.246000,
      "address": "Vo Nguyen Giap, Da Nang",
      "indoorOutdoor": "Outdoor",
      "averageVisitDurationMinutes": 90,
      "hasShelter": false,
      "averageRating": 4.6,
      "reviewCount": 1284,
      "thumbnailUrl": "https://example.invalid/poi/42/cover.jpg",
      "distanceKm": 2.40,
      "isOpenNow": true
    }
  ]
}
```

Nullable fields are `address`, `averageRating`, `thumbnailUrl`, and `distanceKm`. `reviewCount` and
`isOpenNow` are always present. `totalPages` is `0` when `totalCount` is `0`.

# Detail Success DTO

```json
{
  "id": 42,
  "name": "My Khe Beach",
  "description": "A public beach on the east coast of Da Nang.",
  "status": "Active",
  "categoryId": 3,
  "categoryName": "Beach",
  "latitude": 16.061000,
  "longitude": 108.246000,
  "address": "Vo Nguyen Giap, Da Nang",
  "indoorOutdoor": "Outdoor",
  "averageVisitDurationMinutes": 90,
  "hasShelter": false,
  "scenicScore": null,
  "photoRating": null,
  "averageRating": 4.6,
  "reviewCount": 1284,
  "isOpenNow": true,
  "openingHours": [
    {
      "dayOfWeek": 0,
      "openTime": "05:00:00",
      "closeTime": "21:00:00",
      "isClosed": false
    }
  ],
  "photos": [
    {
      "id": 8,
      "url": "https://example.invalid/poi/42/cover.jpg",
      "caption": "Beach at sunrise",
      "sortOrder": 0
    }
  ],
  "tags": [
    {
      "id": 2,
      "name": "Sunrise"
    }
  ],
  "createdAtUtc": "2026-09-09T11:20:00Z",
  "updatedAtUtc": "2026-09-09T11:20:00Z"
}
```

Creator identity is intentionally absent. Opening hours are ordered by `dayOfWeek`; photos by
`sortOrder` then ID; tags by name then ID. Nullable properties remain present and may contain
`null`.

# Calculated Fields

- Distance uses a database-translatable great-circle calculation in kilometres. Radius filtering
  and sorting use the unrounded value; only the returned value is rounded to two decimals.
- Rating uses all `social.Reviews.rating` rows with `target_type = 'POI'` and matching `target_id`.
  The returned average is rounded to one decimal; no reviews yields `averageRating = null` and
  `reviewCount = 0`.
- Thumbnail is the first POI photo ordered by `sort_order`, then `photo_id`.
- Opening-now uses `UTC+07:00`, Sunday `0`, and `openTime <= localTime < closeTime`. A missing row or
  `isClosed = true` is closed. Overnight intervals are outside the current SQL/domain model.

# Failure Contract

| Condition | HTTP status | Body |
|---|---|---|
| Invalid route ID or query value/combination | `400` | RFC 7807 `ValidationProblemDetails` with camelCase field keys. |
| Unknown category filter or no matching Active POI | `200` | Empty paginated DTO. |
| Missing or Inactive detail POI | `404` | RFC 7807 `ProblemDetails` with `errorCode = Poi.NotFound`. |
| Unexpected database/system failure | `500` | Generic RFC 7807 `ProblemDetails`; no exception, SQL, credential, or stack detail. |

`404` example:

```json
{
  "status": 404,
  "title": "The requested point of interest was not found.",
  "errorCode": "Poi.NotFound"
}
```

# Persistence and Performance Contract

- No schema change or EF migration.
- Read-only EF queries use `AsNoTracking`.
- No controller business logic or direct DbContext access.
- Filters, aggregate projections, deterministic sorting, and pagination execute in SQL.
- Do not load the catalogue and then calculate/filter in memory.
- Avoid N+1 loading; a list request may use one count query and one bounded result query.
- Pass the request cancellation token to every asynchronous database operation.
- Add SQL Server integration coverage for translation, collation-sensitive search, aggregate
  values, ordering, and pagination.

# Approval Gate

D1-D6, the MVP requirement, and this API contract were approved on 2026-09-12. TM-98 is merged into
Backend `develop` at `e6d64ec`. Preparation of the atomic TM-58 implementation plan is authorized;
production-code execution begins only after that plan is separately reviewed and approved and the
complete baseline, including SQL Server tests, is green.
