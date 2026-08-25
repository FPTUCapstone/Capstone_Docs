# MVP Goal

Let a Guest who forgot their password regain access to their existing account on their own,
without contacting support.

# Target User

Guest holding a registered account, currently unable to sign in due to a forgotten password.

# Core User Problem

A forgotten password is a full account lockout unless there's a self-service way back in.

# Core User Journey

Request reset with email → receive reset link → follow link → set new password → sign in with
the new password.

# Must Have

- Request reset via registered email address. *(AF1)*
- System sends a reset link via email. *(FR2)*
- Link verification before allowing a new password to be set. *(BR1, FR3)*
- New password validated against the same policy as registration. *(BR2)*
- Confirmation of successful reset. *(FR5)*
- Generic response regardless of whether the email matches an account, to avoid account
  enumeration. *(security-driven assumption, EF1)*

# Should Have

- Resend option if the email doesn't arrive, with reasonable rate-limiting to prevent abuse.
- Single-use reset link (BR3 assumption) — important for security, should be confirmed and built
  even if not explicitly tested at MVP demo time.

# Could Have

- Phone/OTP-based reset (AF2) — deferred, same reasoning as deferring phone/OTP sign-in (adds an
  SMS delivery dependency).
- Explicit expiry countdown or messaging on the reset link.

# Out of Scope

- Any additional identity re-verification beyond the link itself (e.g., security questions).
- Locked/restricted account interaction with password reset — treat as a later decision, don't
  special-case it at MVP without a confirmed rule.

# MVP User Journey

1. Guest selects "Forgot password?" and enters their email.
2. System responds generically (does not confirm/deny the email is registered) and, if it
   matches an account, sends a reset link.
3. Guest opens the email and follows the link.
4. System verifies the link.
5. Guest enters a new password (validated against the standard password policy).
6. System updates the credential and confirms success.
7. Guest signs in with the new password (UC-04).

# Dependencies

- Depends on UC-04 Sign In existing as the destination after a successful reset.
- Reuses the password policy defined in register-traveler-account.md (BR4) rather than defining a
  new one.
- Depends on an email delivery mechanism (shared with the registration confirmation email).

# Risks

- If the "generic response" behavior (Must Have) isn't actually implemented, the reset-request
  endpoint becomes an account-enumeration vector — a real security risk, not just a UX gap.
- If the reset link isn't single-use (BR3, only an assumption), a leaked/shared link could be
  replayed.

# Open Questions

- Exact reset link expiry time.
- Is the link single-use?
- Resend behavior/rate-limiting if delivery fails.
- Can a locked/restricted account still reset its password?
- Is the Guest auto-signed-in after reset, or must they sign in separately?
