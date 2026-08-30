# Screen Inventory

1. **Invite Group Members** — Flutter Mobile invitation display/share screen for UC-18.

# Screen: Invite Group Members

## Related UC

UC-18 — Invite Group Members

## Actor

Traveler acting as Group Host

## Platform

Flutter Mobile only

## Purpose

Generate and display a unique invitation code and QR invitation for the selected travel group without creating membership.

## Entry Conditions

Selected group exists; current Traveler is authenticated and is its Group Host.

## Entry Points

Travel Group Details & Members → **Invite Members**.

## Exit / Navigation

Back → Travel Group Details & Members. Copy/share leaves the Host on this screen.

## Required Data

Group ID/name; Host Traveler ID; Invitation ID/code/status; QR invitation data; created timestamp; expiration timestamp where applicable.

## Main Layout Regions

1. Top app bar.
2. Group identity card using “Hoi An Weekend Crew”.
3. QR invitation panel.
4. Invitation code field with Copy action.
5. Share action and optional expiry/status metadata.
6. Locked message region.

## Components

### User Inputs

None.

### Read-only Information

Invitation code, QR representation, group name, usability/status, and expiry where supplied.

### Primary Actions

- **Copy Invite Code**.
- **Share Invitation**.

### Secondary Actions

- Back.
- Retry generation after failure.

## Validation

Group exists; current Traveler is Host; invitation uniquely references the correct group and is usable. No usage quota is inferred. Apply CR-13 while generating.

## Business Rules

BR-43, BR-44, BR-46. Generating/sharing never creates membership. Apply CR-10 to CR-13 and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG55 | Invite code "{Invite_Code}" copied to clipboard. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG126 | You do not have permission to access this function. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Permission/group check in progress.

### Loading State

Invitation skeleton/progress; copy/share disabled.

### Loaded State

Code and QR are visible and shareable.

### Empty State

No invitation content before generation completes; represent as Loading, not a domain empty result.

### Validation Error State

Not applicable to user input.

### Business Error State

Non-Host access uses MSG126.

### System Error State

MSG127 with Retry; no invitation is displayed as usable.

### Success State

Copying shows MSG55; invitation remains displayed.

### Confirmation State

Not required.

### Disabled Action State

Copy/share disabled until a usable invitation exists and during generation.

### Offline State

Generation requires connectivity; MSG127 on network failure.

## Navigation Map

Travel Group Details & Members → Invite Group Members → Back to group. Invitation recipient later uses UC-23.

## Open Questions

Expiration is displayed only where provided. No usage-quota behavior exists.

## UX Suggestions

Place QR and code in one share card with generous scanning contrast; do not add member-count or quota controls.

