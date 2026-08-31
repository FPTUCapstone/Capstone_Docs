# Screen Inventory

1. **Travel Group Details & Members** — Host member-management context.
2. **Remove Group Member Confirmation** — Material 3 modal/dialog for the irreversible UC-20 action.

# Screen: Travel Group Details & Members

## Related UC

UC-20 — Remove Group Member

## Actor

Traveler acting as Group Host

## Platform

Flutter Mobile only

## Purpose

Let the Host select an eligible active member and initiate removal from the current member list.

## Entry Conditions

Authenticated Group Host opens an existing group's active members.

## Entry Points

Travel Groups → selected group → Members.

## Exit / Navigation

Remove action → confirmation dialog. Success returns to refreshed member list.

## Required Data

Group/member data from UC-19 plus selected active Member ID/name and current Host permission.

## Main Layout Regions

Top app bar; group summary; active member list; total count/pagination; member action menu; toast/message region.

## Components

### User Inputs

Member selection and list page/filter/sort controls.

### Read-only Information

Permitted member details, Host/member status, Joined Timestamp.

### Primary Actions

- **Remove Member** on eligible active member.

### Secondary Actions

Back and other separate-UC group actions.

## Validation

CR-01 list preservation; current Traveler must be Host; target must remain active.

## Business Rules

BR-45 and BR-48. Apply CR-01, CR-05, CR-06, CR-07, CR-10 to CR-13, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG126 | You do not have permission to access this function. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |
| MSG128 | No records found matching your criteria. |

## States

### Initial State

Member list context.

### Loading State

Loading members or refreshing after removal.

### Loaded State

Active members displayed.

### Empty State

MSG128 if applicable.

### Validation Error State

Not applicable to text input.

### Business Error State

MSG126 when not permitted; stale target refreshes without removal.

### System Error State

MSG127; previous membership remains.

### Success State

Updated list and MSG60 toast.

### Confirmation State

Handled by Remove Group Member Confirmation.

### Disabled Action State

Remove hidden/disabled for ineligible members and while request is active.

### Offline State

MSG127; no optimistic removal.

## Navigation Map

Members → member action → Remove Confirmation → updated Members.

## Open Questions

None.

## UX Suggestions

Use a compact overflow action on each eligible member rather than a persistent destructive button.

# Screen: Remove Group Member Confirmation

## Related UC

UC-20 — Remove Group Member

## Actor

Traveler acting as Group Host

## Platform

Flutter Mobile only; modal/dialog

## Purpose

Obtain explicit confirmation before changing membership to Removed and revoking access.

## Entry Conditions

Host has selected an active eligible member.

## Entry Points

Travel Group Details & Members → Remove Member.

## Exit / Navigation

Cancel → close dialog. Confirm → process and return to refreshed members.

## Required Data

Selected Member ID and permitted display name.

## Main Layout Regions

Modal title, exact MSG59 body, Cancel action, destructive Remove Member action, in-flight state.

## Components

### User Inputs

Confirmation choice.

### Read-only Information

Selected member name in MSG59.

### Primary Actions

- **Remove Member** (confirm).

### Secondary Actions

- **Cancel**.

## Validation

Revalidate Host permission and active membership after confirmation and before applying.

## Business Rules

BR-45, BR-48; CR-05, CR-06, CR-11 to CR-13.

## Application Messages

| Code | Locked content |
|---|---|
| MSG59 | Are you sure you want to remove {Member_Name} from this group? |
| MSG60 | {Member_Name} has been removed from the group. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG126 | You do not have permission to access this function. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Confirmation modal open with MSG59.

### Loading State

Confirm action shows progress; both actions disabled.

### Loaded State

Same as Initial.

### Empty State

Not applicable.

### Validation Error State

Not applicable.

### Business Error State

Permission/stale membership prevents update; MSG126 where applicable.

### System Error State

MSG127; modal may close or remain for retry, but membership stays unchanged.

### Success State

Dialog closes, list refreshes, MSG60 toast appears.

### Confirmation State

Required MSG59 modal before request.

### Disabled Action State

Confirm disabled while request is active.

### Offline State

MSG127; no removal.

## Navigation Map

Members → Confirmation → Cancel/Updated Members.

## Open Questions

None.

## UX Suggestions

Use Material 3 destructive-action styling without changing the locked message.
