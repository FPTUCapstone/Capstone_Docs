# Screen Inventory

1. Sign In Screen — email/password authentication (MVP scope; phone/OTP and Google Social Login
   are Should/Could Have and not designed here).

Only 1 screen is needed for the Must Have MVP journey.

---

# Screen: Sign In Screen

## Purpose

Authenticate an existing account holder via email and password, and route them into the app
under their resolved role.

## User

Guest holding a registered TripMate account (Traveler, Tour Operator, or Administrator).

## Entry Conditions

Guest is unauthenticated and navigates to Sign In (specific entry points not enumerated in
source).

## Required Components

### Inputs

- Email
- Password

### Information Display

- Field-level or form-level error messages (see Error Handling).

### Primary Actions

- Sign In (submit button).

### Secondary Actions

Not confirmed in source (e.g., a "Forgot password?" link into UC-06, or a "Create account" link)
— see UX Suggestions; do not treat as required.

## Validation

- Required-field check (email, password).
- Credential match against the stored account.
- Account status check (locked/inactive/restricted), independent of credential correctness.

## Error Handling

- **E1 — Invalid credentials:** generic error message, does not reveal whether email or password
  was wrong.
- **E2 — Locked/inactive/restricted account:** blocked with a status-specific message (exact
  wording undefined — see Open Questions).
- **E3 — System/authentication failure:** generic error; no session issued.

## States

- **Initial / Input** — empty fields, no errors.
- **Validation Error** — missing required field(s).
- **Business Error** — invalid credentials (E1) or restricted account (E2).
- **Loading** — submitting/validating.
- **Success** — transitions into the app (role-specific landing not defined here).

## Navigation

- On success → role-appropriate landing area (undefined in source, out of scope for this spec).
- On any error → stays on this screen.

## Business Rules

BR1, BR2, BR3 (see [requirements/sign-in.md](../../requirements/sign-in.md)).

## Open Questions

- Exact wording for a locked/inactive/restricted account error.
- Whether a "Forgot password?" link (into UC-06) or a link to registration belongs on this screen.
- Where each role lands after a successful sign-in.

## UX Suggestions

- A "Forgot password?" link near the password field, pointing into the Reset Password flow
  (UC-06) — common pattern, not a confirmed requirement here.
- A link for new users to reach account registration — not confirmed as required for this screen.
