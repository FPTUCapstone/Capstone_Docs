# Screen Inventory

1. Change Password Screen — reached from Account Settings.

Only 1 screen is needed for the Must Have MVP journey.

---

# Screen: Change Password Screen

## Purpose

Let an authenticated user replace their password, verifying identity via their current password.

## User

Any authenticated user — Traveler, Tour Operator, or Administrator (identical for all roles).

## Entry Conditions

User is authenticated and navigates to Account Settings → Change Password.

## Required Components

### Inputs

- Current password (masked).
- New password (masked).

### Information Display

- Password policy hints (reusing the standard policy from registration).

### Primary Actions

- Change Password (submit).

### Secondary Actions

None defined.

## Validation

- Current password must match the stored credential.
- New password must meet the standard password policy.
- (Should Have) New password must differ from the current password.

## Error Handling

- **E1 — Current password incorrect:** inline error on that field.
- **E2 — New password fails policy:** inline field-level error.
- **E3 — System/update failure:** generic error; credential unchanged.

## States

- **Initial / Input** — empty fields.
- **Validation Error** — policy not met (E2) or current password incorrect (E1).
- **Loading** — submitting.
- **Success** — confirmation shown.

## Navigation

- On success → confirmation, then back to Account Settings (assumption) — or Sign In, if
  re-authentication turns out to be required (unresolved).
- On error → stays on this screen.

## Business Rules

BR1, BR2, BR3 (see [requirements/change-password.md](../../requirements/change-password.md)).

## Open Questions

- Is re-authentication required after a successful change?
- Are other active sessions signed out on change?

## UX Suggestions

- Show the same password policy hint styling used on the registration and reset-password screens,
  for visual consistency.
