# Screen Inventory

1. **Commercial Services Search & List** — search/browse active commercial POIs.
2. **Commercial Service Details** — inspect one selected commercial POI and optionally hand off to UC-31.

# Screen: Commercial Services Search & List

## Related UC

UC-30 — View Commercial Service

## Actor

Traveler

## Platform

Flutter Mobile only

## Purpose

Browse and submit searches for active POIs classified exactly as Hotel, Vehicle Rental, or Restaurant.

## Entry Conditions

Authenticated Traveler opens Commercial Services or a supported commercial-service entry.

## Entry Points

Traveler navigation; itinerary/POI catalogue entry.

## Exit / Navigation

Select POI → Commercial Service Details. Back from details restores page/filter/sort.

## Required Data

POI ID/name/category/description/location/address/photos; category; requested date/time; location; min/max price; supported filters; price/range; availability where reliable; total count/page/sort/filter state.

## Main Layout Regions

Top app bar; submit search field; exact-category chips; filters; result count; POI card list; pagination; messages.

## Components

### User Inputs

Category (Hotel/Vehicle Rental/Restaurant only), search location, date/time where applicable, min/max price, supported filters, sort, page.

### Read-only Information

POI cards with name/category/image/location, price/range where known, and truthful availability status.

### Primary Actions

- **Search** (submit).
- Select POI.

### Secondary Actions

Open/clear supported filters; sort; pagination; Back.

## Validation

Search on submit; exact categories only; active POIs; valid price range; CR-01 20/page/total/preserved state; no unverified confirmed availability.

## Business Rules

BR-87. BR-88/BR-89 apply only after handoff to UC-31. Apply CR-01 to CR-04, CR-07 to CR-13, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG01 | This field is required. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |
| MSG128 | No records found matching your criteria. |

## States

### Initial State

First 20 active supported-category POIs and total count.

### Loading State

Card skeletons; Search disabled.

### Loaded State

Matching POI cards and preserved criteria.

### Empty State

MSG128.

### Validation Error State

Inline missing/format/price-range errors; MSG01 where applicable.

### Business Error State

Inactive POI omitted; unsupported category unavailable.

### System Error State

MSG127; prior state unchanged.

### Success State

Loaded results; no Booking created.

### Confirmation State

Not applicable.

### Disabled Action State

Search disabled while submitting; unavailable/inactive result cannot proceed as bookable.

### Offline State

MSG127.

## Navigation Map

Commercial Services → Search & List → Details → Back restores list.

## Open Questions

Other supported filters/category attributes are not enumerated.

## UX Suggestions

Use a three-chip category selector and image-led POI cards; this does not create new categories.

# Screen: Commercial Service Details

## Related UC

UC-30 — View Commercial Service

## Actor

Traveler

## Platform

Flutter Mobile only

## Purpose

Display stored detail for one active supported-category POI and optionally navigate to separate UC-31.

## Entry Conditions

Active Hotel, Vehicle Rental, or Restaurant POI selected from list or supported itinerary/catalogue entry.

## Entry Points

Commercial Services Search & List; itinerary/POI catalogue.

## Exit / Navigation

Back → preserved source. Eligible Book Service CTA → UC-31 only.

## Required Data

POI ID/name/category/description; location/coordinates/address; opening hours where applicable; photos; category-specific commercial attributes; price/range where available; requested date/time; availability only where reliable.

## Main Layout Regions

Hero image/name/category; description; location/address; hours; commercial attributes; price/availability panel; optional UC-31 CTA.

## Components

### User Inputs

None for UC-30, except date/time context carried from search when already provided.

### Read-only Information

All selected POI and trustworthy commercial information.

### Primary Actions

- **Book Service** only as navigation to UC-31 when eligible.

### Secondary Actions

Back.

## Validation

Selected POI active and in exactly one supported category. Displayed location belongs to POI. Never claim unverified availability.

## Business Rules

BR-87; BR-88/BR-89 only govern UC-31 handoff. Apply CR-07, CR-08, CR-10 to CR-12, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

No new message code or user-facing availability message is invented. When availability cannot be reliably determined, the UI withholds confirmed-availability treatment and disables the booking handoff where required.

## States

### Initial State

Selected POI identity shell.

### Loading State

Detail skeleton; Book Service disabled.

### Loaded State

Complete stored detail and reliable availability where known.

### Empty State

Not applicable; inactive/missing POI is Business Error.

### Validation Error State

Not applicable.

### Business Error State

Inactive POI not presented as bookable; unverified availability is not presented as confirmed.

### System Error State

MSG127; no active booking claim.

### Success State

Details displayed; no Booking created.

### Confirmation State

Not part of UC-30.

### Disabled Action State

Book Service disabled when POI inactive/ineligible or availability cannot support booking.

### Offline State

MSG127.

## Navigation Map

Search/List or Itinerary POI → Commercial Service Details → optional UC-31.

## Open Questions

Category-specific attributes are not exhaustively defined.

## UX Suggestions

Use category-specific iconography without changing the centralized POI model or adding provider identity.
