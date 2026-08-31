# Screen Inventory

1. **Travel Group Details & Members** — Flutter Mobile member-list view for UC-19.

# Screen: Travel Group Details & Members

## Related UC

UC-19 — View Group Members

## Actor

Traveler (active group member)

## Platform

Flutter Mobile only

## Purpose

Show the current Group Host and active members with only privacy-permitted membership information.

## Entry Conditions

Authenticated Traveler is an active member of the selected existing group.

## Entry Points

Travel Groups → selected group → Members.

## Exit / Navigation

Back → Travel Groups. Permitted Host/member actions link to their separate UCs.

## Required Data

Group ID/name; current Traveler ID; Host ID; active member IDs; permitted member information; membership status; Joined Timestamp; location-sharing status; total count; page/filter/sort state.

## Main Layout Regions

1. Top app bar and group summary.
2. Host-highlighted member card.
3. Active-member list.
4. Total-count/pagination controls.
5. Role-permitted group actions.
6. Message/empty region.

## Components

### User Inputs

Page/filter/sort controls where exposed.

### Read-only Information

Permitted member identity, Host status chip, Joined Timestamp, membership and location-sharing status.

### Primary Actions

None for UC-19; viewing is read-only.

### Secondary Actions

Back; permitted navigation to Invite/Remove/Leave/Location Sharing UCs.

## Validation

Group exists; current Traveler is active; only permitted member data is returned. CR-01 requires 20 records per page, total count, and preserved page/filter/sort. CR-07/CR-15 apply.

## Business Rules

BR-48, BR-51. Apply CR-01, CR-07, CR-10 to CR-12, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG126 | You do not have permission to access this function. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |
| MSG128 | No records found matching your criteria. |

## States

### Initial State

Group summary shell and member-list placeholder.

### Loading State

Member-card skeletons; actions unavailable until authorization resolves.

### Loaded State

Host and active members shown with total count.

### Empty State

MSG128 where applicable; no blank list.

### Validation Error State

Not applicable to the read-only list.

### Business Error State

Inactive/non-member access uses MSG126.

### System Error State

MSG127 with Retry; no stale data is represented as current.

### Success State

Loaded State is the read-only success state.

### Confirmation State

Not part of UC-19.

### Disabled Action State

Role-restricted actions hidden/disabled; server denial still uses MSG126.

### Offline State

Current membership requires network retrieval; MSG127 when unavailable.

## Navigation Map

Travel Groups → Travel Group Details & Members → separate UC actions; Back restores Travel Groups.

## Open Questions

Exact permitted member profile fields remain governed by privacy rules.

## UX Suggestions

Use Host and sharing status chips; avoid exposing contact details unless explicitly permitted.

