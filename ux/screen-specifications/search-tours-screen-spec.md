# Screen Inventory

1. **Search Tours** — public Tour search/list screen for UC-24.

# Screen: Search Tours

## Related UC

UC-24 — Search Tours (`TM-70`)

## Actor

Guest / Traveler

## Platform

Flutter Mobile and responsive Next.js Web.

## Purpose

Browse and submit searches for approved, publicly available Tours and open UC-26 details.

## Entry Conditions

Public Tours section is available. Authentication is not required for browsing.

## Entry Points

Public Home, Traveler Home, or primary Tours navigation.

## Exit / Navigation

Select result → Tour Details. Back from details restores page and active filters.

## Required Data (TM-70 Owned Scope)

- **Owned Search Inputs**:
  - `destination`: optional string, trimmed and normalized to Unicode NFC, max 300 characters.
  - `departureDate`: optional single departure date (`YYYY-MM-DD`; any valid calendar date including past dates is accepted as search criteria and may return an empty result, while schedule eligibility and availability are evaluated in `Asia/Ho_Chi_Minh`).
  - `minPrice` / `maxPrice`: optional whole-VND integers (`0..9,999,999,999`, `minPrice <= maxPrice`).
  - `page` / `pageSize`: `page >= 1`, default `pageSize = 20` (`1..100`).
- **Out-of-Scope / Deferred SRS Filters**:
  - `keyword`, `date range`, `duration`, `category`, `rating`, `sort`, and `interest tags` are explicitly out of scope for TM-70.
- **Result Data**:
  - `tourId`, `title`, `destinations`, `operatorName`, `durationDays`, `basePrice`, `currency`, `representativeScheduleId`, `departureAtUtc`, `availabilityStatus` (`available`, `soldOut`, `noUpcomingSchedule`, `unknown`), `remainingSlots`, `thumbnailUrl`, `page`, `pageSize`, `totalCount`, `totalPages`.

## Main Layout Regions

Top app bar/search header; submit-based filter bar (Web) or filter bottom sheet trigger (Mobile); active filter chips (bound to `displayedCriteria` / committed `query`); result count; Tour card list/grid; platform pagination region; empty/error/stale-warning region.

## Components

### User Inputs

- `destination` (trimmed + Unicode NFC normalized, $\le 300$ chars)
- single `departureDate` (`YYYY-MM-DD`)
- `minPrice` and `maxPrice` (whole VND `0..9,999,999,999`)
- page navigation:
  - **Web**: Numbered pagination (`TourPagination`, 20 items per page, URL search-param synced, $\ge 48\text{px} \times 48\text{px}$ interactive controls)
  - **Mobile**: Infinite scroll (triggers `loadNextPage` at 85% scroll extent with `tourId` deduplication) + pull-to-refresh (`RefreshIndicator`)

### Read-only Information

Tour cards with identity, thumbnail (or neutral accessible placeholder), operator summary, duration, formatted VND price, representative departure timestamp, and current availability summary; total count; active filter chips reflecting committed/displayed search criteria.

### Primary Actions

- **Search / Apply filters** (explicit submit).
- Select Tour.

### Secondary Actions

Open/reset filters (active filter chips are read-only indicators paired with a single `Clear filters` / `Reset` action that clears all active filters at once; individual chip removal is not supported); change page (Web) or pull-to-refresh / scroll to load more (Mobile).

## Validation

Search only on explicit submit (CR-02). `destination` is normalized to Unicode NFC and trimmed before checking length $\le 300$. `departureDate` must be a valid YYYY-MM-DD calendar date. Past valid dates are accepted criteria and may return an empty result. `minPrice` and `maxPrice` must be whole-VND integers in `0..9,999,999,999` with `minPrice <= maxPrice`. Web raw URL validation and the BFF proxy reject duplicate, casing-mismatched, or unknown query keys before canonicalization and return HTTP 400 ProblemDetails at the proxy boundary. Response parsers strictly reject malformed DTO payloads (unknown `availabilityStatus`, non-string `destinations` elements, non-integer numbers, invalid timestamps, or non-string `thumbnailUrl`). Only approved/public tours are returned. CR-01: 20 per page, total count, deterministic ordering (`title ASC, tourId ASC`).

## Business Rules

BR-56, BR-60. Apply CR-01 to CR-04, CR-07 to CR-12, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG01 | This field is required. |
| MSG64 | No tour packages found matching your destination and dates. |
| MSG65 | This tour package is currently unavailable for booking. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

First 20 public Tours and total count load; optional criteria empty.

### Loading State

Tour-card skeletons (`8` cards in `TourListSkeleton` on Web, `4` cards in `_TourSkeletonList` on Mobile); Search/Filter submit disabled during request.

### Loaded State

Matching Tour cards, total count, active filter chips, and pagination state.

### Empty State

MSG64; preserve criteria and offer filter reset without invented fallback results.

### Validation Error State

Inline destination/date/price errors associated with invalid fields; no search request dispatched and normal empty state is not rendered.

### Business Error State

Selected Tour becomes unavailable: MSG65 and no booking implication.

### System & Stale-Data Error State

- Initial load failure: MSG127 full error state with retry action.
- Subsequent refresh/search failure with existing results: preserve previously loaded results, displayed criteria associated with those results, and pagination, while retaining requested criteria in editable state and displaying a non-destructive inline warning banner with retry action.

### Success State

Loaded results; selecting a Tour navigates to UC-26 without creating Booking.

### Confirmation State

Not required.

### Disabled Action State

Search disabled while submitting; selection/booking indication disabled for unavailable result.

### Offline State

Public results require network; MSG127.

## Navigation Map

Home/Tours → Search Tours → Tour Details → preserved Search Tours.

## Scope & Pagination Decision Notes

- **Owned TM-70 Filters**: `destination`, single `departureDate`, `minPrice`, `maxPrice`, `page`, `pageSize`, and real-time availability status.
- **Out-of-Scope SRS Filters**: `keyword`, `date range`, `duration`, `category`, `rating`, `sort`, and `interest tags`.
- **Platform Pagination Pattern**: Web uses numbered pagination (`TourPagination`); Mobile uses infinite scroll (85% threshold) + pull-to-refresh (`RefreshIndicator`).
