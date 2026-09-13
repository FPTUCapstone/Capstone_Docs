# Explore Points of Interest MVP

Status: **APPROVED FOR IMPLEMENTATION PLANNING - D1-D6 approved on 2026-09-12**

Jira: TM-58

Use case: UC-12 Explore Points of Interest

# MVP Goal

Allow a Guest or authenticated Traveler to browse Active points of interest and inspect the
stored catalogue detail needed by list, map-marker, and detail views. This actor choice is the
approved reconciliation of the Jira/coverage-matrix wording with the Traveler-only wording in
Report 3 section 3.3.3.

# Core User Journey

Open Explore Points of Interest -> load the first page of Active POIs -> optionally submit search,
category, distance, opening-now, or sort criteria -> view matching list/map-marker data -> select
one Active POI -> view its stored details.

# Must Have

- Flutter Mobile and responsive Next.js Web can consume the same read-only backend contract.
- Only `Active` POIs appear in public results or detail responses.
- Search is applied to the POI name after the client submits criteria.
- Category, distance from an origin, and opening-now filters are supported.
- CR-01 behavior starts at 20 records per page and returns the total count.
- List items contain POI identity, category, coordinates, address, visit duration, shelter,
  rating summary, thumbnail, distance when an origin is supplied, and current opening state.
- Detail contains description, category, coordinates, address, opening hours, photos, tags,
  stored travel attributes, rating summary, and current opening state.
- An empty search is a successful empty page, not an application failure.
- Missing and Inactive POIs are indistinguishable to public detail callers.
- Browse operations do not modify catalogue data or create audit records.
- JSON uses `camelCase`; database mappings preserve the SQL v7 schema and `snake_case` columns.

# Out of Scope

- Adding a POI to an itinerary; that write belongs to UC-11.
- Commercial-service detail or booking; those belong to UC-30 and UC-31.
- Favorite/bookmark writes.
- POI create, update, or deactivate behavior.
- New city, district, area, polygon, spatial, or geocoding models.
- Frontend/Mobile implementation and end-to-end UAT in the backend increment.
- EF Core migrations or SQL schema changes.

# Data Sources

- `catalog.POIs`
- `catalog.POICategories`
- `catalog.POIOpeningHours`
- `catalog.POIPhotos`
- `catalog.Tags`
- `catalog.POITagMap`
- `social.Reviews`, restricted to `target_type = 'POI'`

# Approved Decisions

| ID | Topic | Approved decision | Reconciliation context |
|---|---|---|---|
| D1 | Actor and authorization | Anonymous Guest and authenticated Traveler receive the same public catalogue response. | Report 3 section 3.3.3 names Traveler, while Jira, the UC table, and the coverage matrix name Guest and Traveler. |
| D2 | Selected area | Do not add an area table. Define the search area with origin latitude/longitude and an optional maximum distance; omit those values to search all Active POIs. | SQL v7 has coordinates and address only, with no stable area identifier or polygon. |
| D3 | Opening-now timezone | Evaluate against Vietnam time (`UTC+07:00`, no daylight-saving adjustment), with Sunday as day `0`; missing/closed day means closed and closing time is exclusive. | SQL v7 stores local weekly times but no POI timezone. |
| D4 | HTTP response standard | Use typed success DTOs and RFC 7807 `ProblemDetails`/`ValidationProblemDetails`, matching the backend repository contract. | This overrides the synthetic envelope example for TripMate and is recorded in `api/contracts/api-response-standard.md`. |
| D5 | Sorting and numeric presentation | Name ascending by default; distance ascending; rating descending with unrated POIs last; use POI ID as the final tie-breaker. Filter on unrounded distance, return distance to two decimals, and rating to one decimal. | Report 3 names sort criteria but does not define direction, tie-breakers, or rounding. |
| D6 | Page-size guard | Default to 20 and reject values above 100. | CR-01 establishes 20 per page in existing Docs, but no shared maximum is currently recorded. |
| D7 | TM-98 dependency | Resolved: PR #8 was merged into Backend `develop` at `e6d64ec`; TM-58 reuses that aggregate and its mappings. | TM-58 must not duplicate the merged POI types or infrastructure. |

D1-D6 were explicitly approved on 2026-09-12. D7 is a resolved engineering dependency rather than
an additional product decision.

# Error and Message Reconciliation

- Invalid submitted criteria return field-level validation; FE/Mobile preserves the previous result
  set as required by Report 3.
- Denied location permission is a client state. The client omits origin/distance parameters and may
  display MSG46; the backend does not receive device-permission state.
- Report 3 assigns MSG33 and MSG34 to UC-12 outcomes, but the message catalogue assigns those codes
  to unrelated itinerary outcomes. Until Docs resolves the catalogue, the backend returns an empty
  `200` page or `Poi.NotFound`; it does not emit the conflicting message text.
- Unexpected retrieval failures use the backend's generic non-sensitive error contract. FE/Mobile
  may map that outcome to MSG127.

# Dependencies

- TM-98 core POI aggregate and SQL v7 EF mappings merged at Backend commit `e6d64ec`.
- Approved canonical API contract for the two TM-58 endpoints.
- Approved response standard in `api/contracts/api-response-standard.md`.
- Report 3 BR-24, BR-34, BR-51, CR-01, MSG46, and MSG127, with the MSG33/MSG34 conflict documented
  above.

# Risks

- Duplicating the merged TM-98 entities or configurations would create conflicting domain and EF
  models.
- Calculating distance after loading all POIs would violate the performance checklist.
- Using the server's local timezone would make opening-now behavior environment-dependent.
- Returning creator/account data would leak information not required by the public use case.
- Treating screen-only ticket-price examples as persisted data would invent fields absent from SQL
  v7.

# Implementation Gate

The requirement and D1-D6 contract decisions are approved. TM-98 is merged and the TM-58 branch is
synchronized with that dependency. Preparation of an atomic implementation plan is authorized;
production code remains blocked until that plan receives separate approval and the complete
baseline, including SQL Server tests, is green.
