# TM-70 — UC-24 Search Tours V2 Contract Addendum

## Document Control

| Field | Value |
|---|---|
| Status | **APPROVED** |
| Approval date | 2026-10-07 |
| Task | TM-70 — [UC-24] Search Tours |
| Primary source | `Report3_Software-Requirement-Specification-V2.docx` |
| Applies to | Backend, responsive Next.js Web, Flutter Mobile |

## 1. Scope and Authority

This document supplements Report 3 V2 only where UC-24 contains an internal
conflict or leaves implementation semantics unspecified. Report 3 V2 remains
authoritative for all other requirements.

The approved decisions D1–D9 in this addendum apply only to UC-24. They do not
rewrite unrelated use cases, business rules, messages, entities, or platform
requirements.

UC-24 is a read-only public Tour discovery function. It does not create a
booking and does not modify Tour data.

## 2. Actors, Authorization, and Platforms

- **Actors:** Guest and Traveler.
- **Authentication:** Not required to search or view search results.
- **Booking boundary:** Authentication is required before a booking can be
  created.
- **Platforms:** Responsive Next.js Web and Flutter Mobile.
- **Visibility:** Only Administrator-approved and publicly published Tours are
  discoverable.

Both platforms implement the same search contract and acceptance behavior.
Differences in visual layout must not change supported criteria, sorting,
pagination, availability behavior, or result fields.

## 3. Complete Search Criteria

UC-24 supports:

- Search Keyword;
- Destination;
- Departure Date Range;
- Price Range;
- Duration;
- Tour Category;
- Minimum Rating;
- Sorting Option;
- Page Number.

Search is executed only when the user submits the keyword or applies the
filters. It is not executed on each keystroke.

## 4. Canonical Request Contract

Endpoint:

```http
GET /api/v1/tours
```

### 4.1 Query parameters

| Parameter | Type | Required | Normalization/default | Validation and interaction |
|---|---|---:|---|---|
| `keyword` | string | No | Trim, Unicode NFC; empty becomes omitted | Case-insensitive and accent-insensitive token matching under Section 5. |
| `destination` | string | No | Trim, Unicode NFC; empty becomes omitted | Case-insensitive and accent-insensitive substring match against the public Destination name(s) associated with the Tour. This predicate is independent from `keyword`; when both are supplied, both must match. |
| `startDate` | `yyyy-MM-dd` | No | No default | Inclusive lower bound in `Asia/Ho_Chi_Minh`; may be supplied without `endDate`. |
| `endDate` | `yyyy-MM-dd` | No | No default | Inclusive upper bound in `Asia/Ho_Chi_Minh`; may be supplied without `startDate`. |
| `minPrice` | whole VND integer | No | No default | Non-negative and must be `<= maxPrice` when both exist. No UC-24-specific upper bound is introduced. |
| `maxPrice` | whole VND integer | No | No default | Non-negative and must be `>= minPrice` when both exist. No UC-24-specific upper bound is introduced. |
| `durationDays` | integer | No | No default | Exact equality filter; must be `>= 1`; no TM-70-specific maximum. |
| `category` | string | No | Trim; empty becomes omitted | Must be a stable active category code returned by the Backend taxonomy contract. |
| `minimumRating` | decimal | No | No default | Inclusive `1.0..5.0`; Tour must have `aggregateRating >= minimumRating`. |
| `sort` | enum | No | Keyword present: `relevance`; otherwise: `departureDate` | Values: `relevance`, `priceAsc`, `priceDesc`, `rating`, `departureDate`. Empty-keyword `relevance` falls back to `departureDate`. |
| `page` | integer | No | `1` | One-based and must be `>= 1`. |
| `pageSize` | integer | No | `20` | Engineering safety range `1..100`; Web and Mobile UC-24 use 20 and expose no page-size selector. |

Unknown keys, duplicate keys, wrong-cased keys, invalid formats, and values
outside their allowed range are rejected as validation errors. Invalid values
must not be silently replaced with defaults.

Every accepted price must be exactly representable by the Backend request,
monetary transport, and persistence types. If those technical types impose an
upper representation limit, the API contract, OpenAPI schema, and tests must
publish the same limit as a technical constraint rather than a UC-24 business
rule.

If `page` is valid but exceeds `totalPages`, the response is a successful empty
page with the requested page and the correct pagination metadata. It is not
silently clamped.

## 5. Keyword Semantics

Keyword searches these public fields:

1. Tour Title;
2. Destination name;
3. public Tour Operator name.

Tour Description and Tour Category are not keyword fields. Category has a
dedicated filter.

Matching behavior:

- trim surrounding whitespace;
- normalize to Unicode NFC;
- compare case-insensitively;
- compare accent-insensitively;
- tokenize on whitespace;
- every token must match at least one searchable field;
- tokens may match different searchable fields;
- use substring matching within a field;
- an empty normalized keyword adds no keyword predicate.

For each token, choose its best match using this precedence, from highest to
lowest:

1. exact normalized Tour Title;
2. Tour Title prefix;
3. Tour Title token or substring;
4. Destination;
5. Tour Operator name.

Every token must match. The Tour-level relevance class is the
lowest-precedence (worst) class among its matched tokens. For example, when one
token is an exact Title match and another token matches only Operator Name, the
Tour is classified in the Operator Name class.

Tours are ordered so that exact Title precedes Title prefix, which precedes
Title token/substring, Destination, and Operator Name. Within the same class,
order by Aggregate Rating descending with null last, then Title ascending,
then Tour ID ascending.

The implementation must prove case-insensitive and accent-insensitive behavior
against the production SQL provider; an in-memory approximation is not
sufficient evidence.

### 5.1 Dedicated Destination filter

The `destination` filter is separate from keyword search. Its input is trimmed
and normalized to Unicode NFC; an empty normalized value is omitted. A Tour
matches when the normalized value is a case-insensitive and accent-insensitive
substring of at least one public Destination name associated with that Tour.
When `keyword` and `destination` are both present, the Tour must satisfy both
predicates.

## 6. Departure Date Semantics

### 6.1 Search-discoverable schedule and request-level asOfUtc snapshot

A search-discoverable schedule is a `TourSchedule` that belongs to an
Administrator-approved and publicly published Tour and whose schedule record
is not Cancelled or otherwise administratively disabled.

#### Request-level asOfUtc snapshot
Every Tour Search query execution captures a single `asOfUtc` timestamp snapshot
in UTC exactly once at the start of request execution:
- Upcoming schedule determination defines an upcoming schedule as
  `departureAtUtc >= asOfUtc`.
- A schedule with `departureAtUtc < asOfUtc` is a past schedule.
- The identical snapshot instant is returned in the response page envelope
  `asOfUtc`.
- Representative schedule selection, upcoming filtering, and real-time
  availability evaluation within that request must evaluate against the exact
  same `asOfUtc` snapshot.
- Independent clock calls across query phases or clock-drift discrepancies
  within a single request are prohibited.

#### Schedule discoverability and booking eligibility
- An upcoming search-discoverable schedule (`departureAtUtc >= asOfUtc`) with
  active capacity participates in filtering and is eligible for booking.
- A sold-out search-discoverable schedule remains discoverable; its availability
  is `soldOut` and its `remainingSlots` is `0`.
- A past schedule (`departureAtUtc < asOfUtc`) or a schedule with lifecycle
  status `Completed` may satisfy an explicit past date criterion and can be
  selected as the representative schedule. However:
  - It is **not bookable** (`bookable = false`).
  - The Tour remains visible in search results.
  - Detail navigation remains allowed.
  - Booking is disabled for that representative schedule.
  - Its availability status must **not** be converted to `soldOut` merely
    because it is past or completed; underlying capacity/availability is
    preserved where known (e.g. `available` if unbooked capacity remained, or
    `soldOut` if capacity was exhausted).
  - Clients determine booking eligibility from the canonical schedule state
    and time contract (`departureAtUtc < asOfUtc`, status `Completed`, etc.) or
    downstream Tour Detail contract, without inventing non-standard DTO
    properties.
- A Cancelled or administratively disabled schedule never participates in
  date filtering, representative-schedule selection, or departure sorting.

`startDate` and `endDate` are independently optional local calendar dates:

- both omitted: no departure-date predicate;
- only `startDate`: match search-discoverable schedules departing on or after it;
- only `endDate`: match search-discoverable schedules departing on or before it;
- both supplied: match the inclusive `startDate..endDate` range.

Dates are interpreted in `Asia/Ho_Chi_Minh`. When both dates exist,
`startDate <= endDate` is required. Past dates are valid criteria and may return
an empty result.

A Tour matches the date criterion when at least one search-discoverable
schedule has a departure local date satisfying the applicable bound or range.

### 6.2 Representative schedule

The representative schedule for each Tour is selected deterministically:

1. **Ordering rule:** Eligible candidate schedules are ordered strictly by:
   - `departureAtUtc` ASC (equivalent to `Asia/Ho_Chi_Minh` departure instant ASC);
   - `scheduleId` ASC (natural/lexicographical ascending comparison matching the
     Backend identifier type, e.g. stable string ordering).
   If two schedules have the identical departure instant, the schedule with the
   lexicographically lower stable `scheduleId` is selected. Database/provider
   non-determinism is prohibited.

2. **Selection with departure-date criterion:** When an explicit date criterion
   exists (`startDate`, `endDate`, or both):
   - Filter search-discoverable schedules matching the date predicate (including
     past/completed schedules that fall within the requested date bounds).
   - Select the first schedule ordered by `departureAtUtc ASC, scheduleId ASC`
     (the earliest matching departure).

3. **Selection without departure-date criterion:** When no departure-date
   criterion is supplied:
   - Filter search-discoverable schedules that are upcoming
     (`departureAtUtc >= asOfUtc` using the request-level `asOfUtc` snapshot).
   - Select the first schedule ordered by `departureAtUtc ASC, scheduleId ASC`
     (the nearest upcoming departure).

4. **No representative schedule:** When no upcoming search-discoverable
   schedule exists and no explicit date criterion selected a schedule, return:
   - `representativeScheduleId = null`,
   - `departureAtUtc = null`,
   - `availabilityStatus = noUpcomingSchedule`,
   - `remainingSlots = null`.

`availabilityStatus` and `remainingSlots` describe the representative
schedule. If availability retrieval fails for a known representative schedule,
return `availabilityStatus = unknown` and `remainingSlots = null`, while
retaining its `representativeScheduleId` and `departureAtUtc`.

## 7. Duration

- Request field: `durationDays`.
- Unit: calendar days.
- Type: integer.
- Minimum: 1.
- Predicate: exact equality with the Tour's canonical duration in days.
- No additional TM-70-specific maximum is introduced.

## 8. Tour Category

The Backend owns the Tour Category taxonomy.

Minimum category fields:

```text
categoryId
code
name
isActive
```

Rules:

- many Tours may reference one `TourCategory`;
- each Tour has exactly one Tour Category in the current TripMate scope;
- Tour uses `category_id`, or the project-equivalent foreign-key
  representation;
- clients filter using the stable category `code` returned by Backend;
- only Active categories are selectable;
- Web and Mobile must not maintain independent hardcoded category lists;
- no many-to-many category membership is introduced.

Category taxonomy endpoint:

```http
GET /api/v1/tour-categories
```

It is anonymous and returns only categories with `isActive = true`, exposing
`categoryId`, `code`, and English `name`. Results are ordered by English
`name` ascending, then `code`
ascending.

The production category names and seed values are:

> **PENDING PRODUCT/BA SEED APPROVAL**

This seed approval does not block schema, domain, taxonomy endpoint, or query
implementation. It does block production backfill and final enforcement that
every persisted Tour has a non-null category.

## 9. Rating

- Scale: `1.0..5.0`.
- Aggregate: arithmetic mean of eligible published/visible Tour reviews.
- Filtering uses the unrounded mean.
- `minimumRating` is inclusive.
- API/display value is rounded to one decimal place.
- With no eligible review, `aggregateRating` is `null`.
- An unrated Tour remains eligible when no rating filter is supplied.
- An unrated Tour is excluded when `minimumRating` is supplied.

Review eligibility must follow the canonical published/visible and deletion
lifecycle rules of the Review domain. TM-70 must not make hidden or deleted
reviews contribute to the aggregate.

## 10. Sorting

Every sorting option ends with deterministic tie-break ordering.

| Sort | Canonical order |
|---|---|
| `relevance` | Exact Title matches first, then Title prefix, Title token/substring, Destination, and Operator Name; within the same class: Aggregate Rating DESC with null last, Title ASC, TourId ASC |
| `priceAsc` | Base Price ASC, Title ASC, TourId ASC |
| `priceDesc` | Base Price DESC, Title ASC, TourId ASC |
| `rating` | Aggregate Rating DESC, unrated last, Title ASC, TourId ASC |
| `departureDate` | Representative departure ASC (selected under Section 6 with departure instant ASC, scheduleId ASC tie-break using the request asOfUtc snapshot), missing representative schedule last, Title ASC, TourId ASC |

Defaults:

- keyword present: `relevance`;
- keyword absent: `departureDate`;
- `relevance` supplied with an empty normalized keyword: use
  `departureDate`.

`Title ASC, TourId ASC` is a tie-break, not a substitute for the selected
primary sort.

For `departureDate` sort, the representative schedule of each Tour is evaluated
against the request-level `asOfUtc` snapshot and deterministic selection rules
in Section 6 (`departure instant ASC, scheduleId ASC`). Primary ordering sorts
by representative departure instant ascending; Tours without a representative
schedule sort last. In all cases, `Title ASC, TourId ASC` resolves ties between
Tours.

## 11. Pagination

- Page numbering is one-based.
- Default page size is 20.
- Response includes `page`, `pageSize`, `totalCount`, and `totalPages`.
- Pagination controls appear at the bottom of the result list.
- Web and Mobile provide explicit page navigation and display the current page.
- Mobile infinite-scroll-only behavior does not satisfy this contract.
- A platform may prefetch internally, but the displayed result set and URL/state
  remain associated with one current page.

## 12. Availability

Availability is obtained at display time and is not cached beyond the current
result rendering. All availability evaluations for a search request use the
identical request-level `asOfUtc` snapshot.

Availability describes the representative schedule selected under Section 6.
Cancelled or administratively disabled schedules are never used.

### 12.1 Availability status versus booking eligibility

`availabilityStatus` represents capacity and inventory status for the
representative schedule:
- `available`
- `soldOut`
- `noUpcomingSchedule`
- `unknown`

It does **not** represent booking eligibility. Booking eligibility is evaluated
by clients from canonical schedule timing and lifecycle state or downstream
Tour Detail contracts without inventing non-standard DTO properties.

| Scenario | Representative schedule state | `availabilityStatus` | `remainingSlots` | Bookable (`bookable`) | Detail navigation |
|---|---|---|---|---|---|
| **Upcoming & Available** | `departureAtUtc >= asOfUtc`, capacity > 0 | `available` | Remaining count (> 0) | Yes (`true`) | Allowed |
| **Upcoming & Sold Out** | `departureAtUtc >= asOfUtc`, capacity == 0 | `soldOut` | `0` | No (`false`) | Allowed |
| **Provider failure** | Known schedule, availability retrieval failed | `unknown` | `null` | No (`false`) | Allowed |
| **Past / Completed** | `departureAtUtc < asOfUtc` or status `Completed` (selected via explicit past date filter) | `available` (or `soldOut` if capacity was 0) | Preserved count (or `0`) | No (`false`) | Allowed |
| **No upcoming schedule** | No date filter, no future schedule | `noUpcomingSchedule` | `null` | No (`false`) | Allowed |

Rules:
- A past or completed representative schedule must **not** be converted to
  `soldOut` merely because it is past or completed. Its capacity status and
  remaining slots are preserved where known, but booking is disabled
  (`bookable = false`).
- If availability retrieval fails for a known representative schedule:
  - the Tour remains visible;
  - `availabilityStatus` is `unknown`;
  - `remainingSlots` is `null`;
  - the known `representativeScheduleId` and `departureAtUtc` remain present;
  - Detail navigation remains available;
  - booking is disabled until availability is available again;
  - the Tour is not presented as sold out merely because availability is
    unknown.

## 13. Clear Filters

Clear Filters preserves the normalized Search Keyword and clears:

- Destination;
- `startDate`;
- `endDate`;
- `minPrice`;
- `maxPrice`;
- `durationDays`;
- Category;
- `minimumRating`.

It resets `page` to 1 and resets sort to the applicable default:

- keyword present: `relevance`;
- keyword empty: `departureDate`.

The result list is retrieved again using the preserved keyword.

## 14. Canonical Response Contract

### 14.1 Page envelope

| Field | Type | Required | Semantics |
|---|---|---:|---|
| `page` | integer | Yes | Current one-based page. |
| `pageSize` | integer | Yes | Requested/effective page size. |
| `totalCount` | integer | Yes | Total matches before pagination. |
| `totalPages` | integer | Yes | Page count derived from total and size. |
| `asOfUtc` | UTC timestamp | Yes | Time at which result availability was evaluated. Captured once at request start as the canonical request-level snapshot used for upcoming schedule filtering, representative selection, and availability evaluation. |
| `items` | array | Yes | Tour result items for the current page. |

### 14.2 Tour result item

All properties are required to be present, including nullable properties.

| Field | Type | Nullable | Semantics |
|---|---|---:|---|
| `tourId` | string | No | Stable Tour identifier. |
| `title` | string | No | Public Tour name. |
| `thumbnailUrl` | string | Yes | Real Tour thumbnail; null renders the neutral Tour placeholder. |
| `destinations` | string array | No | Public destination labels in display order. |
| `durationDays` | integer | No | Tour duration in days; at least 1. |
| `basePrice` | whole-VND integer | No | Public base price. |
| `currency` | string | No | `VND`. |
| `aggregateRating` | number | Yes | One-decimal aggregate or null when unrated. |
| `remainingSlots` | integer | Yes | Remaining slots for the representative schedule; `0` when sold out, or null when unavailable/not applicable. Preserves underlying capacity for past/completed representative schedules. |
| `availabilityStatus` | enum | No | Representative schedule capacity status: `available`, `soldOut`, `noUpcomingSchedule`, or `unknown`. Distinct from booking eligibility. |
| `representativeScheduleId` | string | Yes | Schedule selected by the deterministic rules in Section 6 (`departure instant ASC, scheduleId ASC`), if one exists. |
| `departureAtUtc` | UTC timestamp | Yes | Selected representative departure, converted for display by the client. |
| `operatorName` | string | No | Public Tour Operator name retained for compatibility and keyword context. |

No POI image is substituted for `thumbnailUrl`. Runtime mock or hardcoded Tour
images are prohibited.

## 15. Context Persistence

When the user opens UC-26 Tour Detail and returns, UC-24 restores:

- keyword;
- all filters;
- selected sort;
- current page.

Restoring scroll position is optional and is not an acceptance blocker. TM-70
does not prescribe TM-210's internal Tour Detail implementation.

## 16. Messages

The incorrect UC-24 reference to MSG29 is replaced by:

| ID | English resource text | Use |
|---|---|---|
| MSG157 | `Minimum price must not be greater than maximum price.` | Invalid submitted price range |
| MSG158 | `Start date must not be later than end date.` | Invalid submitted two-ended date range |

Existing UC-24 messages remain:

- MSG64 for no matching Tour;
- MSG127 for system/network search failure.

Messages and all other user-facing copy are English resource entries under
CR-09. They are not hardcoded separately by platform.

## 17. Business Rules

Existing Appendix BR52–BR55 remain unchanged for their existing domains.

| ID | UC-24 rule |
|---|---|
| BR137 | Search runs when the user submits the keyword or applies filters. Results are paginated according to CR-01. |
| BR138 | Price/date ranges are valid only when `minPrice <= maxPrice` and, when both dates exist, `startDate <= endDate`. |
| BR139 | Guest may browse/search Tour packages. Authentication is required only before creating a booking. |
| BR140 | Availability is retrieved at display time and is not cached beyond the current result rendering. |

BR56 remains the Approved/Public Tour visibility rule.

## 18. Dependencies and Boundaries

- **TM-208:** Preserve the nullable Backend Tour thumbnail contract.
- **TM-209:** Preserve Web/Mobile real-thumbnail and neutral-fallback behavior.
- **TM-210:** Tour Detail remains separate. TM-70 owns preservation of search
  context for handoff and return.
- **TM-211:** Rating infrastructure may be delivered separately, but Aggregate
  Rating, Minimum Rating, and Rating sort are mandatory UC-24 acceptance
  dependencies.
- **TM-212:** Favorites, recommendations, persona matching, and booking
  implementation remain outside TM-70.

Production category seed approval is an external Product/BA gate. It does not
authorize clients to hardcode temporary categories.

## 19. Acceptance Criteria

- **AC-01:** An anonymous Guest can open and submit Tour Search on Web and
  Mobile without an access token.
- **AC-02:** A Traveler receives the same search/filter/sort contract as a
  Guest.
- **AC-03:** Results contain only Administrator-approved and publicly published
  Tours.
- **AC-04:** Search runs on submit/apply, not on each keystroke.
- **AC-05:** Keyword matches Title, Destination, and Operator Name according to
  the all-token substring rule.
- **AC-06:** Keyword matching is case-insensitive and accent-insensitive; an
  unaccented query can match the accented public value.
- **AC-07:** Empty normalized keyword adds no keyword predicate.
- **AC-08:** Destination input is trimmed and NFC-normalized, then matched as a
  case-insensitive and accent-insensitive substring against public Destination
  names associated with the Tour. When Keyword is also supplied, both
  predicates must match.
- **AC-09:** `startDate` alone includes search-discoverable schedules on or
  after that local date, including a completed/past schedule for an explicit
  past criterion; Cancelled/disabled schedules never match.
- **AC-10:** `endDate` alone includes search-discoverable schedules on or before
  that local date, including a completed/past schedule for an explicit past
  criterion; Cancelled/disabled schedules never match.
- **AC-11:** Both date endpoints form an inclusive local date range; the
  representative schedule is deterministically selected by earliest departure
  instant ASC and stable scheduleId ASC tie-break; sold-out matching schedules
  remain discoverable; past/completed schedules matched by date criteria remain
  non-bookable while preserving capacity status.
- **AC-12:** Both dates with `startDate > endDate` show MSG158 and dispatch no
  search.
- **AC-13:** Past date criteria are accepted and may return an empty result.
- **AC-14:** Whole-VND price bounds are non-negative and inclusive. There is no
  UC-24-specific upper bound; any published technical limit matches the
  Backend monetary transport/persistence representation.
- **AC-15:** `minPrice > maxPrice` shows MSG157 and dispatches no search.
- **AC-16:** `durationDays` performs exact positive-integer equality filtering.
- **AC-17:** Category options are fetched from Backend; only Active categories
  are selectable and clients contain no independent taxonomy.
- **AC-18:** Filtering by category code returns only Tours assigned to that
  category.
- **AC-19:** Minimum Rating uses the unrounded aggregate and an inclusive
  predicate.
- **AC-20:** An unrated Tour appears without a rating filter and is excluded
  when a Minimum Rating is supplied.
- **AC-21:** Aggregate Rating is displayed to one decimal place; unrated is
  represented by a neutral no-rating state, not zero.
- **AC-22:** Every keyword token uses its best approved match class and every
  token must match. The Tour uses its worst matched token class; exact Title
  precedes Title prefix, Title token/substring, Destination, and Operator Name,
  with rating-null-last, Title, and TourId tie-breaks inside a class.
- **AC-23:** Price ascending and descending sorts use Base Price followed by
  Title and TourId tie-breaks.
- **AC-24:** Rating sort places higher ratings first and unrated Tours last.
- **AC-25:** Departure sort uses the representative-schedule rules in Section
  6 evaluated against the request-level asOfUtc snapshot (upcoming defined as
  departureAtUtc >= asOfUtc when no date filter exists, deterministic tie-break
  departure instant ASC, scheduleId ASC), excludes Cancelled/disabled schedules,
  orders representative departures ascending, and places missing representative
  schedules last, followed by Title ASC and TourId ASC tie-breaks.
- **AC-26:** Keyword presence selects Relevance by default; absence selects
  Departure Date by default.
- **AC-27:** A manually supplied Relevance sort with an empty keyword behaves
  as Departure Date sort.
- **AC-28:** Web and Mobile show explicit one-based page navigation with 20
  items per page by default and the total count.
- **AC-29:** Clear Filters preserves keyword, clears every approved filter,
  resets page to 1, and restores the applicable default sort.
- **AC-30:** Availability status represents capacity, not booking eligibility.
  If one known representative schedule's availability fails, the Tour remains
  visible with unknown availability and null slots, retains the representative
  schedule ID/time, enables Detail navigation, and disables booking without being
  marked sold out. A past/completed representative schedule preserves its
  capacity status while booking is disabled.
- **AC-31:** Real Tour thumbnail renders when supplied; null/failure uses the
  neutral Tour placeholder without POI or mock fallback.
- **AC-32:** A search/system failure shows MSG127 and leaves the previous result
  list and its displayed criteria unchanged.
- **AC-33:** Search → Detail → Back restores keyword, filters, sort, and current
  page.
- **AC-34:** Nullable result properties are present in JSON even when their
  value is null.
- **AC-35:** Web and Mobile consume the same request/response semantics and are
  covered by contract-parity tests.
- **AC-36:** Every user-facing UC-24 string is English and sourced from a
  platform resource file.
- **AC-37:** Public search responses remain `no-store`, retain pagination
  metadata, and preserve retry/stale-result behavior.
