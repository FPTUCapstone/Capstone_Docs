# Screen Inventory

1. **Travel Group Details & Members** — group context and Leave entry.
2. **Leave Travel Group Confirmation** — Material 3 modal/dialog whose content changes by BR-49 Host state.

# Screen: Travel Group Details & Members

## Related UC

UC-21 — Leave Travel Group

## Actor

Traveler (active group member)

## Platform

Flutter Mobile only

## Purpose

Provide the Leave Group entry from the selected active membership.

## Entry Conditions

Authenticated Traveler is an active member of the selected group.

## Entry Points

Travel Groups → selected group.

## Exit / Navigation

Leave Group → confirmation. Successful leave → Travel Groups or prior safe destination outside the group.

## Required Data

Group ID/name/status; current Traveler membership/Host state; active remaining members and Joined Timestamps.

## Main Layout Regions

Top app bar; group summary; member/Host status; group action area; Leave Group action.

## Components

### User Inputs

None before confirmation.

### Read-only Information

Current Group Host/member status and group identity.

### Primary Actions

- **Leave Group**.

### Secondary Actions

Back and other separate-UC group navigation.

## Validation

Traveler must still be active. Current Host state and active remaining members must be loaded before confirmation.

## Business Rules

BR-48 and BR-49. Apply CR-05 to CR-07, CR-10 to CR-13, and CR-15.

## Application Messages

See confirmation screen.

## States

### Initial State

Group details loaded with Leave action.

### Loading State

Membership/Host-state loading.

### Loaded State

Current state confirmed.

### Empty State

Not applicable.

### Validation Error State

Not applicable to input.

### Business Error State

No longer active: block leave and refresh/exit group.

### System Error State

MSG127; membership unchanged.

### Success State

Traveler redirected away; no member-only data remains accessible.

### Confirmation State

Handled by Leave Travel Group Confirmation.

### Disabled Action State

Leave disabled while state is unresolved or request active.

### Offline State

No leave operation is applied; MSG127.

## Navigation Map

Group Details → Leave Confirmation → Travel Groups/safe destination.

## Open Questions

None.

## UX Suggestions

Keep Leave Group low-emphasis/destructive in the group action menu until invoked.

# Screen: Leave Travel Group Confirmation

## Related UC

UC-21 — Leave Travel Group

## Actor

Traveler (active member or Group Host)

## Platform

Flutter Mobile only; modal/dialog

## Purpose

Confirm leaving with the correct locked message and deterministic BR-49 Host outcome.

## Entry Conditions

Active membership and current Host state have been resolved.

## Entry Points

Travel Group Details & Members → Leave Group.

## Exit / Navigation

Cancel → group. Confirm → process, revoke access, redirect outside group.

## Required Data

Current Traveler/Host state; remaining active members; Joined Timestamps; selected Next Host; final-member/group-closure state.

## Main Layout Regions

Modal title; exact confirmation body; successor card where MSG61 applies; Cancel; Leave Group; in-flight state.

## Components

### User Inputs

Confirmation choice only. There is no Host picker.

### Read-only Information

- For Host with members: selected successor, determined as earliest Joined Timestamp.
- Final-member closure consequence.

### Primary Actions

- **Leave Group** (confirm).

### Secondary Actions

- **Cancel**.

## Validation

Revalidate active membership and Host state. Transfer Host before marking current Host Left. If no other active member remains, close group. Never permit manual successor selection.

## Business Rules

BR-48, BR-49; CR-05, CR-06, CR-11 to CR-13.

## Application Messages

| Code | Locked content |
|---|---|
| MSG61 | You are the Group Host. Leaving will transfer Host privileges to {Next_Member_Name}. Confirm leave? |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |
| MSG129 | Operation completed successfully. |
| MSG130 | Are you sure you want to continue? This action may not be reversible. |

## States

### Initial State

Correct confirmation variant shown.

### Loading State

Leave action in progress; modal actions disabled.

### Loaded State

Same as Initial.

### Empty State

Not applicable.

### Validation Error State

Not applicable.

### Business Error State

Membership/Host state changed; stop and keep the Traveler active.

### System Error State

MSG127; current membership and Host state remain unchanged.

### Success State

Membership Left, end timestamp recorded, access revoked, redirect; MSG129 where no more specific success message exists.

### Confirmation State

- Non-Host: MSG130.
- Host with remaining members: MSG61 naming the earliest-Joined active member.
- Final Host: MSG130 and group-closure consequence.

### Disabled Action State

Leave disabled while submitting or if a required successor cannot be consistently assigned.

### Offline State

MSG127; no local optimistic leave.

## Navigation Map

Group → correct confirmation variant → cancel/group or success/outside group.

## Open Questions

None; BR-49 locks successor selection.

## UX Suggestions

For MSG61, show the successor's permitted display name in a compact read-only row; never expose a selection control.

