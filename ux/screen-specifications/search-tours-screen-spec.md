# Screen Inventory

1. **Search Tours** — public Tour search/list screen for UC-24.

# Screen: Search Tours

## Related UC

UC-24 — Search Tours

## Actor

Guest / Traveler

## Platform

Flutter Mobile and responsive Next.js Web. Current Stitch output covers Flutter Mobile UI only.

## Purpose

Browse and submit searches for approved, publicly available Tours and open UC-26 details.

## Entry Conditions

Public Tours section is available. Authentication is not required for browsing.

## Entry Points

Public Home, Traveler Home, or primary Tours navigation.

## Exit / Navigation

Select result → Tour Details. Back from details restores page, filters, and sort.

## Required Data

User context; destination; date/date range; min/max price; interest tags; supported filters; sort; Tour identity/image/operator summary; price; departure/availability summary; total count; current page.

## Main Layout Regions

Top app bar/search title; submit-based search field; filter controls/sheet; sort; result count; Tour card list; pagination; empty/error region.

## Components

### User Inputs

Destination; date/date range; min/max price; interest tags and only other supported filters; sort; page.

### Read-only Information

Tour cards with identity, image, operator summary, formatted price, relevant departure and current availability summary; total count.

### Primary Actions

- **Search** (submit).
- Select Tour.

### Secondary Actions

Open/clear supported filters; sort; change page.

## Validation

Search only on submit (CR-02). Valid formats; end date not before start; minimum price not above maximum. Only approved/public tours. CR-01: 20 per page, total count, preserve page/filter/sort. CR-07/CR-08 formatting.

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

Tour-card skeletons; Search button disabled during request.

### Loaded State

Matching Tour cards, total count, page/filter/sort state.

### Empty State

MSG64; preserve criteria and offer no invented fallback.

### Validation Error State

Inline date/price/format errors; no search request.

### Business Error State

Selected Tour becomes unavailable: MSG65 and no booking implication.

### System Error State

MSG127; previous client state unchanged.

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

## Open Questions

“Other Supported Filters” are not enumerated.

## UX Suggestions

Use a mobile filter bottom sheet and compact POI/tour cards; this adapts presentation only and preserves submit search and CR-01.

