# Screen Inventory

1. **Create Travel Group** — Flutter Mobile creation form for UC-17.

# Screen: Create Travel Group

## Related UC

UC-17 — Create Travel Group

## Actor

Traveler

## Platform

Flutter Mobile only

## Purpose

Create a travel group linked to the selected eligible itinerary and make the creating Traveler its exclusive Group Host.

## Entry Conditions

Authenticated Traveler has an existing accessible itinerary.

## Entry Points

Eligible Itinerary → **Create Travel Group**.

## Exit / Navigation

- Cancel/Back → Itinerary with no group created.
- Success → newly created Travel Group Details & Members.
- Session expiry → Sign In with destination preserved.

## Required Data

Selected Itinerary ID/title/date; Group Name; created Group ID/status/timestamp; Host identity.

## Main Layout Regions

1. Material 3 top app bar.
2. Read-only itinerary association card.
3. Group Name form field.
4. Host-assignment explanation/status.
5. Primary create action and message region.

## Components

### User Inputs

- Group Name (required).

### Read-only Information

- Selected itinerary title and date.
- Creator will become Group Host.

### Primary Actions

- **Create Travel Group**.

### Secondary Actions

- Cancel/Back.

## Validation

Group Name is required and valid; itinerary must exist and be accessible; no group may be created without it. Apply inline CR-03/CR-04 validation and CR-13 double-submit protection.

## Business Rules

BR-41 and BR-42. Apply CR-03, CR-04, CR-06, CR-07, CR-10 to CR-13, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG01 | This field is required. |
| MSG54 | Travel group created! You are the Group Host. Share the invite code to add members. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG126 | You do not have permission to access this function. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Itinerary card loaded; Group Name empty.

### Loading State

Create action shows progress and is disabled.

### Loaded State

Form and itinerary data are ready.

### Empty State

Not applicable; an eligible itinerary is required to enter.

### Validation Error State

Missing Group Name shows MSG01 inline beneath the field.

### Business Error State

Invalid/inaccessible itinerary blocks creation and uses MSG126 where access is denied.

### System Error State

MSG127; no group or partial Host assignment is shown.

### Success State

MSG54 toast; navigate to new group reflecting linked itinerary and Host.

### Confirmation State

Not required.

### Disabled Action State

Create disabled while submitting and when required data is missing.

### Offline State

Creation requires connectivity; if unavailable, MSG127 and unchanged form.

## Navigation Map

Eligible Itinerary → Create Travel Group → Travel Group Details & Members.

## Open Questions

None.

## UX Suggestions

Use a compact itinerary preview rather than allowing itinerary editing on this screen.

