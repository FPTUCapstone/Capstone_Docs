# MVP Goal

Allow a Guest or Traveler to browse and submit searches for approved, publicly available tours.

# Target User

Guest or authenticated Traveler on Flutter Mobile or responsive Next.js Web.

# Core User Problem

The user needs to find relevant public tours by destination, single departure date, and whole-VND price range with real-time schedule availability before viewing details.

# Core User Journey

Open Tours → see public tours → enter optional criteria → submit → validate → show matching paginated results or exact empty/error state → select Tour Details.

# Must Have (TM-70 Owned Scope)

- Canonical Flutter Mobile and responsive Next.js Web support.
- Display only approved/publicly available tours (`GET /api/v1/tours`).
- Search on explicit submit, never on every keystroke (CR-02).
- Owned filter and query parameters:
  - `destination`: optional string, trimmed and normalized to Unicode NFC before validation/request, maximum 300 characters.
  - `departureDate`: optional single calendar date (`YYYY-MM-DD`), evaluated against future scheduled departures in `Asia/Ho_Chi_Minh`.
  - `minPrice` / `maxPrice`: optional whole-VND integers in `0..9,999,999,999` with `minPrice <= maxPrice`.
  - `page` / `pageSize`: `page >= 1`, `pageSize` in `1..100` (default `20`), deterministic ordering by `title ASC, tourId ASC`.
- Real-time availability summary on each tour card (`available`, `soldOut`, `noUpcomingSchedule`, `unknown`, `remainingSlots`, `representativeScheduleId`, `departureAtUtc`).
- Platform pagination decision:
  - **Web**: Numbered pagination (`TourPagination`, 20 items per page, URL query parameter synchronization).
  - **Mobile**: Infinite scroll (`loadNextPage` triggered at 85% scroll threshold with `tourId` deduplication) + pull-to-refresh (`RefreshIndicator`).
- Validate date format/calendar validity and min/max price bounds inline without dispatching invalid requests.
- Preserve existing results with a non-destructive warning/retry indicator when a subsequent refresh or search fails.
- Use MSG64 for no matches, MSG65 for unavailable tour, MSG127 for failure.
- Selecting a result enters UC-26; no Booking is created.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope (Deferred SRS Filters & Adjacent Use Cases)

- Future SRS filters not in TM-70 owned scope:
  - `keyword` (free-text title/operator search)
  - `date range` (multi-day departure range; TM-70 owns single `departureDate`)
  - `duration` filter
  - `category` / theme filter
  - `rating` / review score filter
  - `sort` selector (TM-70 uses deterministic backend order `title ASC, tourId ASC`)
  - `interest tags` filter
- Booking creation (UC-27).
- Personalized tour recommendations (UC-25).
- Editing tours.

# MVP User Journey

Browse or submit filters; see a paginated result list (numbered pagination on Web, infinite scroll + pull-to-refresh on Mobile); open Tour Details.

# Dependencies

- Approved/public tour catalogue and current availability summary (`GET /api/v1/tours`).
- Report 3 BR-56, BR-60, CR-01 to CR-04, CR-07 to CR-12, CR-15, MSG01, MSG64, MSG65, MSG127.

# Risks

- Stale or non-public tours appearing.
- Search-on-type behavior would violate CR-02.
- Losing list state after returning from details would violate CR-01.

# Open Questions

Future SRS filters (`keyword`, `date range`, `duration`, `category`, `rating`, `sort`, `interest tags`) are deferred to subsequent catalogue filter expansion tasks; no extra filters are invented in TM-70.
