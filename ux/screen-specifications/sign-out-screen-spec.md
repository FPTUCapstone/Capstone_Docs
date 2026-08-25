# Screen Inventory

No dedicated screen is required for the Must Have MVP scope. Sign Out is a single action
available from within the existing authenticated app shell (e.g., a menu item) that immediately
clears the session and redirects to the Sign In screen (see sign-in-screen-spec.md).

A confirmation dialog ("Are you sure you want to sign out?") is a **Could Have** per the MVP
scope, not a required screen — noted below for completeness, not specified as a full screen.

---

# Screen: Sign Out Confirmation (Optional — Could Have, not required for MVP)

## Purpose

Optionally confirm the user's intent before terminating their session.

## User

Any authenticated user (Traveler, Tour Operator, Administrator).

## Entry Conditions

User taps the Sign Out action.

## Required Components

### Inputs

None.

### Information Display

Confirmation prompt text (e.g., "Sign out of TripMate?").

### Primary Actions

- Confirm Sign Out.

### Secondary Actions

- Cancel (dismiss, remain signed in).

## Validation

Not applicable.

## Error Handling

Not applicable at this dialog level; see sign-out.md EF1 for the underlying action's own failure
case (client-side clearing when offline — undefined).

## States

- **Prompt** — the only state, if this optional dialog is built at all.

## Navigation

- Confirm → proceeds with the Sign Out action → Sign In screen.
- Cancel → dismisses, user remains on their current screen, still authenticated.

## Business Rules

BR1 (see [requirements/sign-out.md](../../requirements/sign-out.md)).

## Open Questions

- Whether this confirmation step is built at all is unresolved (source doesn't specify one) — see
  MVP Scope in requirements/mvp/sign-out-mvp.md.

## UX Suggestions

- Skip the confirmation dialog entirely for MVP and sign out immediately on tap — reduces friction
  for a low-risk, easily-reversible action (the user just signs back in). This is a suggestion,
  not a requirement.
