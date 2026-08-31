# Screen Inventory

1. **Join Shared Group Trip** — Flutter Mobile code/QR invitation redemption screen for UC-23.

# Screen: Join Shared Group Trip

## Related UC

UC-23 — Join Shared Group Trip

## Actor

Traveler

## Platform

Flutter Mobile only

## Purpose

Let an authenticated Traveler join an available group using a valid invitation code or QR invitation.

## Entry Conditions

Traveler is authenticated and has invitation code/QR data.

## Entry Points

Travel Groups → Join Group; supported QR invitation entry.

## Exit / Navigation

Back → Travel Groups. Success → joined Travel Group Details & Members.

## Required Data

Invitation code/QR data/status/expiry where applicable; group identity; membership status; Joined Timestamp; itinerary/shared-group summary.

## Main Layout Regions

Top app bar; invitation-code form; “or” separator; Scan QR action; validation/message region; joined-group preview on success.

## Components

### User Inputs

- Invitation Code.
- QR invitation captured through the mobile scanner entry.

### Read-only Information

Resolved group name/summary after validation.

### Primary Actions

- **Join Group**.
- **Scan QR Invitation**.

### Secondary Actions

Back.

## Validation

Code required on code path; invitation exists, usable, unexpired where applicable; group exists/available; Traveler is not already active. Apply CR-03/CR-04 and CR-13.

## Business Rules

BR-46, BR-47, BR-48. No invitation quota. Apply CR-03, CR-04, CR-06, CR-10 to CR-13, CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG01 | This field is required. |
| MSG56 | This invitation is invalid, expired, or no longer available. Please check the invitation and try again. |
| MSG57 | You are already a member of this travel group. |
| MSG58 | You have joined "{Group_Name}"! |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Empty code field and Scan QR action.

### Loading State

Validation/join progress; both submit paths disabled.

### Loaded State

Input ready.

### Empty State

Not applicable.

### Validation Error State

Missing code uses MSG01 inline; invalid invitation uses MSG56.

### Business Error State

Already active uses MSG57; unavailable group uses MSG56.

### System Error State

MSG127; no membership created.

### Success State

MSG58 with resolved group name; open group details.

### Confirmation State

Not required.

### Disabled Action State

Join disabled with empty code or while a join request is active.

### Offline State

Joining requires connectivity; MSG127.

## Navigation Map

Travel Groups → Join Shared Group Trip → Travel Group Details & Members.

## Open Questions

None. No usage quota behavior exists.

## UX Suggestions

Keep QR scanning and manual code entry equally visible; do not add public group browse.

