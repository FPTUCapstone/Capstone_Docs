# Screen Inventory

1. **Tour Details** — public Tour detail and booking-eligibility screen for UC-26.

# Screen: Tour Details

## Related UC

UC-26 — View Tour Details

## Actor

Guest / Traveler

## Platform

Flutter Mobile and responsive Next.js Web. Current Stitch output covers Flutter Mobile UI only.

## Purpose

Present complete current public Tour information and expose booking only when the Tour remains eligible; Guests may view but must authenticate before UC-27.

## Entry Conditions

A public Tour is selected from Search, Recommendations, or another supported public entry.

## Entry Points

Search Tours; Tour Recommendations; supported public Tour link/card.

## Exit / Navigation

Back restores source list state. Eligible authenticated Traveler → UC-27. Guest booking action → Sign In with intended destination → UC-27.

## Required Data

Tour ID/title/description/status/duration; Tour Operator information; included POIs; itinerary; planned timeline; meeting point; base price; departure information; capacity/remaining slots; inclusions/other supported information; current eligibility.

## Main Layout Regions

1. Image-led Tour hero and identity.
2. Tour Operator summary.
3. Description and key travel metadata.
4. Included POIs.
5. Itinerary/timeline.
6. Meeting point and departure section.
7. Price/capacity/availability panel.
8. Sticky Booking CTA region and messages.

## Components

### User Inputs

Departure selection only if needed to inspect a specific availability; no booking participant input in UC-26.

### Read-only Information

All required Tour identity, operator, content, POIs, itinerary, timeline, meeting point, price, departure, remaining capacity, availability, and inclusions.

### Primary Actions

- **Book Tour** when eligible.

### Secondary Actions

Back; expand/collapse long content where purely presentational.

## Validation

Tour exists, approved/public. Current slot availability must load before CTA eligibility. Sold-out/unavailable/unknown-capacity states never show an enabled booking CTA.

## Business Rules

BR-59, BR-60. Apply CR-07 to CR-12 and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG65 | This tour package is currently unavailable for booking. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Tour identity shell from selected result.

### Loading State

Hero/content skeleton and separate availability loading; Book Tour disabled.

### Loaded State

Complete Tour content and confirmed current availability.

### Empty State

Not applicable: missing/non-public Tour is Business Error.

### Validation Error State

Not applicable to the read-only detail.

### Business Error State

Unavailable/sold out uses MSG65; content may remain visible but Book Tour disabled.

### System Error State

MSG127. If availability cannot be confirmed, Book Tour remains disabled.

### Success State

Loaded current details. Eligible Traveler may proceed to UC-27; Guest booking intent routes through Sign In.

### Confirmation State

Not part of UC-26.

### Disabled Action State

Book Tour disabled for sold-out/unavailable/unknown availability or while loading.

### Offline State

Current availability requires network; MSG127 and no enabled booking CTA.

## Navigation Map

Search/Recommendations → Tour Details → Sign In if Guest → UC-27; Back restores source list.

## Open Questions

“Other supported Tour information” is not enumerated.

## UX Suggestions

Use a sticky bottom booking bar that shows read-only price and remaining-capacity status; this is a layout suggestion, not a new requirement.

