# Screen Inventory

1. **Group Location Sharing Settings** — Flutter Mobile explicit opt-in settings screen for UC-22.

# Screen: Group Location Sharing Settings

## Related UC

UC-22 — Configure Group Location Sharing

## Actor

Traveler (active group member)

## Platform

Flutter Mobile only

## Purpose

Let the Traveler explicitly enable or disable sharing their GPS location with permitted group members.

## Entry Conditions

Authenticated Traveler is an active member of the selected group.

## Entry Points

Travel Group Details & Members → Location Sharing Settings.

## Exit / Navigation

Back → group. Device permission Settings may be opened when permission is denied.

## Required Data

Group ID/name; Traveler membership; saved sharing status; device location-permission status.

## Main Layout Regions

Top app bar; group identity; privacy explanation; current-status card; Material switch; permission/message area; save state.

## Components

### User Inputs

- Location sharing enabled/disabled switch.

### Read-only Information

Current saved status, device permission status, permitted audience (“group members”).

### Primary Actions

- Toggle/Save sharing status.

### Secondary Actions

- Back.
- Open device Settings after permission denial.

## Validation

Active membership; explicit opt-in; enabling requires device location permission. Never claim enabled without both conditions.

## Business Rules

BR-50, BR-51. Apply CR-06, CR-10 to CR-13, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG46 | Location permission is required for real-time navigation and trip tracking. Please enable it in Settings. |
| MSG62 | Live location sharing is now active with group members. |
| MSG63 | Live location sharing disabled. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG126 | You do not have permission to access this function. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Current setting loading.

### Loading State

Switch/action disabled while loading or saving.

### Loaded State

Saved status reflected accurately.

### Empty State

Not applicable; a boolean status is always represented once loaded.

### Validation Error State

Permission denial uses MSG46 and keeps sharing inactive.

### Business Error State

Inactive/non-member access uses MSG126.

### System Error State

MSG127; previous saved value remains.

### Success State

MSG62 for enabled or MSG63 for disabled; UI reflects saved state.

### Confirmation State

Not required.

### Disabled Action State

Switch disabled during request and when membership is invalid.

### Offline State

Cannot persist change; MSG127 and prior value remains.

## Navigation Map

Group Details → Location Sharing Settings → device Settings where necessary → return to settings/group.

## Open Questions

None.

## UX Suggestions

Use a concise privacy shield/location icon and a single prominent switch; do not add a live member map.

