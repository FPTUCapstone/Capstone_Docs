# Screen Inventory

1. Profile Settings Screen — view and edit full name and phone number (MVP scope; avatar and
   email are Should/Could Have, not detailed here).

Only 1 screen is needed for the Must Have MVP journey.

---

# Screen: Profile Settings Screen

## Purpose

Let a Traveler view and update their basic profile information.

## User

Authenticated Traveler.

## Entry Conditions

Traveler is authenticated and navigates to Profile Settings.

## Required Components

### Inputs

- Full name.
- Phone number.

### Information Display

- Current profile values, pre-filled into the inputs.

### Primary Actions

- Save.

### Secondary Actions

None defined.

## Validation

- Format check on each editable field (e.g., phone format).
- Phone uniqueness check against other accounts, if phone is changed. *(reuses BR2 from
  register-traveler-account.md)*

## Error Handling

- **E1 — Invalid field value:** inline field-level error.
- **E2 — New phone already in use:** inline error on that field.
- **E3 — System/save failure:** generic error; profile unchanged.

## States

- **View / Input** — current values shown, editable.
- **Validation Error** — E1 or E2.
- **Loading** — saving.
- **Success** — confirmation; updated values reflected.

## Navigation

- Stays on this screen throughout (view, edit, save, confirmation all happen here).

## Business Rules

BR1, BR2 (see [requirements/update-traveler-profile.md](../../requirements/update-traveler-profile.md)).

## Open Questions

- Whether avatar and email editing belong on this same screen once in scope, or a separate
  "Account" vs. "Profile" split.
- Does changing email require re-verification?

## UX Suggestions

- Show a subtle "saved" confirmation inline (e.g., a toast) rather than a full navigation away,
  since this is a settings screen the Traveler may return to repeatedly — not a confirmed
  requirement.
