# Screen Inventory

1. Request Reset Screen — Guest enters their registered email.
2. Set New Password Screen — reached via the emailed reset link; Guest sets a new password.

2 screens are needed for the Must Have MVP journey (email-based reset only).

---

# Screen: Request Reset Screen

## Purpose

Let the Guest request a password reset by submitting their registered email address.

## User

Guest who cannot sign in due to a forgotten password.

## Entry Conditions

Guest taps "Forgot password?" (e.g., from the Sign In screen).

## Required Components

### Inputs

- Email address.

### Information Display

- A generic confirmation message after submission (e.g., "If an account exists for this email,
  a reset link has been sent") — shown regardless of whether the email actually matched an
  account, per the account-enumeration-avoidance assumption.

### Primary Actions

- Send Reset Link.

### Secondary Actions

Not confirmed in source (e.g., a link back to Sign In) — see UX Suggestions.

## Validation

- Email format check.

## Error Handling

- **E3 — System/delivery failure:** generic error; Guest can retry.
- Note: a non-matching email does **not** produce a distinct error (see Information Display) —
  this is a deliberate security choice, not a missing validation.

## States

- **Initial / Input** — empty email field.
- **Validation Error** — invalid email format.
- **Loading** — submitting.
- **Success** — generic "check your email" confirmation shown.

## Navigation

- On submission (regardless of match) → generic confirmation state, same screen.
- No further in-app navigation until the Guest opens the emailed link (external).

## Business Rules

BR1 (see [requirements/reset-password.md](../../requirements/reset-password.md)).

## Open Questions

- Resend behavior/rate-limiting if the email doesn't arrive.

## UX Suggestions

- A link back to Sign In, for a Guest who remembers their password after all — not confirmed as
  required.

---

# Screen: Set New Password Screen

## Purpose

Let the Guest, having followed a valid reset link, set a new password for their account.

## User

Guest who followed a valid, unexpired reset link.

## Entry Conditions

Reached only via a valid reset link from the Request Reset email.

## Required Components

### Inputs

- New password (masked).
- Confirm new password — **UX suggestion, not confirmed in source as a required second field**;
  see UX Suggestions.

### Information Display

- Password policy hints (reusing the same policy as registration — see BR4 in
  register-traveler-account.md) — not explicitly restated in source for this screen but reused,
  not invented.

### Primary Actions

- Reset Password (submit).

### Secondary Actions

None defined.

## Validation

- New password validated against the standard password policy (min 8 characters, 1 uppercase, 1
  lowercase, 1 number, 1 special character).
- Reset link validity re-checked at submission (not just at initial page load) — assumption, to
  prevent a stale link from being used after it expires mid-session.

## Error Handling

- **E1 — Invalid/expired reset link:** shown if the Guest arrives with (or the link becomes) an
  invalid/expired link; no password change occurs; Guest can request a new one.
- **E2 — New password fails policy:** inline field-level error.

## States

- **Input** — empty new-password field.
- **Validation Error** — policy not met (E2).
- **Link Error** — invalid/expired link (E1) — may be shown immediately on screen load rather
  than after an input attempt.
- **Loading** — submitting.
- **Success** — confirmation shown, then navigates toward Sign In.

## Navigation

- On success → confirmation → Sign In screen (per the User Flow's success state).
- On link error (E1) → cannot proceed; offer a way back to Request Reset (see UX Suggestions).

## Business Rules

BR1, BR2, BR3 (see [requirements/reset-password.md](../../requirements/reset-password.md)).

## Open Questions

- Whether the Guest is auto-signed-in after reset, or must sign in separately.
- Exact link expiry time (affects how "Link Error" state is triggered).

## UX Suggestions

- A "Confirm new password" second field to catch typos — common pattern, not a confirmed source
  requirement.
- On Link Error, a direct action back to the Request Reset screen — not confirmed as required.
