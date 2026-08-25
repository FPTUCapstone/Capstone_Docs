# Overview

Allows a Guest who has forgotten the password of a registered account to request a password
reset using the registered email address or phone number. TripMate verifies the reset request
using the configured verification mechanism (e.g., a reset link or OTP), and allows a new
password to be established after successful verification.

# Actors

- **Guest** (primary) — holds a registered account but cannot sign in due to a forgotten
  password.
- **System (TripMate Platform)** — verifies the reset request, delivers the verification
  mechanism (link or OTP), and updates the account credential.

# Preconditions

- A registered TripMate account exists for the email address or phone number the Guest provides.
  *(Explicit)*
- The Guest has access to the registered email inbox or phone number, in order to receive the
  reset link/OTP. *(Explicit)*

# Main Flow

1. Guest initiates "Forgot password?" and provides their registered email address or phone
   number. *(Explicit)*
2. System verifies an account exists for the provided identifier. *(Explicit)*
3. System sends a reset link or OTP via the configured verification mechanism. *(Explicit)*
4. Guest follows the reset link or enters the OTP. *(Explicit)*
5. System verifies the reset link/OTP. *(Explicit)*
6. Guest provides a new password. *(Explicit)*
7. System validates the new password and updates the account credential. *(Explicit)*
8. System confirms the password has been reset. *(Explicit)*

# Alternative Flows

- **AF1 — Reset via email (reset link):** Guest receives and follows an emailed link. *(Explicit
  — one of two named mechanisms)*
- **AF2 — Reset via phone (OTP):** Guest receives and enters an OTP sent to their phone.
  *(Explicit — the other named mechanism)*

# Exception Flows

- **EF1 — No account found for the provided identifier:** Whether the system reveals this
  (security risk: account enumeration) or shows a generic "if an account exists, a reset was
  sent" message is undefined. *(See Missing Information)*
- **EF2 — Reset link/OTP expired or invalid:** System rejects the attempt and lets the Guest
  request a new one. *(Assumption)*
- **EF3 — New password fails validation:** System rejects and shows the specific requirement not
  met (consistent with the password policy from register-traveler-account.md BR4). *(Assumption
  — reuses existing password policy, not restated)*
- **EF4 — Delivery failure (email/SMS doesn't arrive):** No fallback defined. *(See Missing
  Information)*

# Postconditions

**Success:**
- The account's password has been updated to the new value.
- The Guest can sign in with the new password.

**Failure:**
- The account's password is unchanged.
- The Guest is informed of the failure reason (to the extent allowed by security practice).

# Functional Requirements

- FR1: The system shall allow a Guest to request a password reset using a registered email or
  phone number.
- FR2: The system shall send a reset link or OTP via the configured verification mechanism.
- FR3: The system shall verify the reset link/OTP before allowing a new password to be set.
- FR4: The system shall validate the new password against the account password policy before
  updating it.
- FR5: The system shall confirm successful password reset to the Guest.

# Business Rules

- BR1: A password reset requires successful verification (link or OTP) before a new password can
  be set — no direct password change without verification. *(Explicit)*
- BR2 *(assumption)*: The new password must meet the same policy as registration (min 8
  characters, 1 uppercase, 1 lowercase, 1 number, 1 special character — see BR4 in
  register-traveler-account.md), since no separate policy is stated for reset.
- BR3 *(assumption)*: Successful password reset invalidates the reset link/OTP (single use) —
  not stated, but standard practice to prevent replay.

# Edge Cases

- Guest requests a reset for an email/phone with no matching account — see EF1, account
  enumeration risk.
- Guest requests multiple resets in quick succession — does each new request invalidate the
  previous link/OTP? Undefined.
- Guest follows an expired reset link — no explicit "request a new one" path confirmed, only
  assumed.
- Reset link/OTP is intercepted or shared — no additional verification (e.g., re-confirming
  identity beyond the link/OTP itself) is defined.
- Account being reset is locked/restricted (see sign-in.md EF4) — can a locked account still have
  its password reset? Undefined.

# Missing Information

- Does the system reveal whether an account exists for a given email/phone (EF1), or always show
  a generic "if an account exists..." message?
- What is the exact expiry time for the reset link/OTP?
- Is the reset link/OTP single-use (BR3 assumption) or can it be reused until expiry?
- What happens if delivery fails (email/SMS doesn't arrive) — is there a resend option, and is it
  rate-limited?
- Can a locked/restricted account still go through password reset?
- Is the Guest automatically signed in after a successful reset, or must they sign in separately
  (consistent question to the one resolved for registration in register-traveler-account.md)?

# MVP Scope

**Suggestion** *(not explicit in source — for discussion)*:
- Email-based reset link only for MVP; defer phone/OTP reset given the added SMS delivery
  dependency, similar reasoning to deferring phone/OTP sign-in.
