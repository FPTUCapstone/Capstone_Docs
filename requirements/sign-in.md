# Overview

Allows a Guest holding a registered account to authenticate to TripMate using a supported
authentication method — email/password, phone number/OTP, or Google Social Login. After
successful authentication, the backend issues the required session or JWT authentication
information, and the user continues in their system-assigned role (Traveler, Tour Operator, or
Administrator), with access granted according to that role and the current account status.
Locked, inactive, or otherwise restricted accounts are handled according to account access rules.

# Actors

- **Guest** (primary) — a visitor holding a registered TripMate account, not yet authenticated in
  this session.
- **System (TripMate Platform)** — validates credentials, issues session/JWT authentication
  information, resolves the account's role and status, and enforces account access rules.
- **Google Identity Provider** (secondary, external) — authenticates the user for the Google
  Social Login method. *(Explicit — referenced as a supported method)*

# Preconditions

- The Guest holds a registered TripMate account (Traveler, Tour Operator, or Administrator).
  *(Explicit)*
- The Guest has access to at least one of the supported authentication methods (email/password
  credentials, a phone number capable of receiving OTP, or a linked Google account). *(Explicit)*

# Main Flow

1. Guest navigates to the Sign In screen. *(Explicit)*
2. Guest selects an authentication method and provides the required credentials (email + password,
   phone number + OTP, or initiates Google Social Login). *(Explicit)*
3. System validates the provided credentials against the supported method. *(Explicit)*
4. System resolves the account's role (Traveler, Tour Operator, or Administrator) and current
   status. *(Explicit)*
5. System checks the account is not locked, inactive, or otherwise restricted. *(Explicit)*
6. System issues session/JWT authentication information. *(Explicit)*
7. System grants access according to the resolved role and account status. *(Explicit)*

# Alternative Flows

- **AF1 — Email/password method:** Guest enters email and password directly. *(Explicit)*
- **AF2 — Phone/OTP method:** Guest enters a phone number, receives an OTP, and enters it to
  authenticate. Exact OTP delivery/expiry mechanics are undefined. *(See Missing Information)*
- **AF3 — Google Social Login:** Guest authenticates via Google; system matches the returned
  identity to an existing TripMate account. Behavior when no matching account exists is undefined.
  *(See Missing Information)*

# Exception Flows

- **EF1 — Invalid credentials (email/password):** System rejects the attempt and shows an error,
  without revealing whether the email or the password was incorrect. *(Assumption — common
  security practice, not stated explicitly)*
- **EF2 — Invalid or expired OTP:** System rejects the attempt and allows the Guest to request a
  new OTP. *(Assumption)*
- **EF3 — Google authentication failure/cancellation:** System informs the Guest the sign-in could
  not be completed. *(Assumption)*
- **EF4 — Locked/inactive/restricted account:** System blocks sign-in and communicates the
  account's restricted status, per account access rules (rules themselves not detailed here — see
  Missing Information). *(Explicit reference, rules undefined)*
- **EF5 — Account not found for the given identifier:** System rejects the attempt with an
  appropriate error. *(Assumption)*
- **EF6 — System/authentication service failure:** System informs the Guest that sign-in could not
  be completed and does not issue partial/invalid session information. *(Assumption)*

# Postconditions

**Success:**
- Session/JWT authentication information has been issued to the client.
- The user continues in their system-assigned role, with access according to that role and
  account status.

**Failure:**
- No session/JWT authentication information is issued.
- The Guest is informed of the failure reason (to the extent allowed by security practice — e.g.,
  generic "invalid credentials" rather than confirming which part was wrong).

# Functional Requirements

- FR1: The system shall support sign-in via email/password, phone number/OTP, and Google Social
  Login.
- FR2: The system shall validate submitted credentials before issuing authentication information.
- FR3: The system shall resolve and apply the account's system-assigned role after successful
  authentication.
- FR4: The system shall check account status (locked/inactive/restricted) and block sign-in
  accordingly.
- FR5: The system shall issue session/JWT authentication information only after successful
  validation and status checks.
- FR6: The system shall grant access to protected functions according to the authenticated user's
  role and account status.

# Business Rules

- BR1: A single Sign In use case supports all three roles (Traveler, Tour Operator,
  Administrator) — there is no separate sign-in flow per role. *(Explicit)*
- BR2: Account status (locked/inactive/restricted) is checked and enforced independently of
  credential validity — correct credentials do not guarantee access if the account is restricted.
  *(Explicit)*
- BR3 *(assumption)*: A Tour Operator account in Pending Approval or Rejected status can still
  sign in (needed for UC-03 Resubmit), but remains restricted from publishing tours/receiving
  bookings per register-tour-operator-account.md — sign-in itself is not blocked by Pending
  Approval or Rejected status, only by "locked/inactive/restricted" in the account-access sense.

# Edge Cases

- Guest attempts Google Social Login with a Google identity that has no matching TripMate account
  — create one, block, or prompt to register? Undefined.
- Guest attempts Google Social Login with a Google identity matching an account that was
  originally created via manual (email/password) registration — link, block, or treat as a
  separate identity? Undefined (also flagged as open in register-traveler-account.md).
- Guest's account is a Tour Operator with a Pending Approval or Rejected application — should
  sign-in succeed (per BR3 assumption) with restricted access, or should the system communicate
  this differently at sign-in time itself?
- Repeated failed sign-in attempts — no lockout/rate-limiting policy defined.
- OTP requested but not received (delivery failure) — no fallback defined.
- Guest is already signed in and attempts to sign in again (e.g., in another tab) — session
  handling behavior undefined.

# Missing Information

- What are the OTP delivery mechanism, format, and expiry time for phone/OTP sign-in?
- What happens when Google Social Login returns an identity with no matching TripMate account —
  auto-create a Traveler account, block, or redirect to registration?
- What happens when a Google identity matches an account created via manual registration — link
  automatically, block, or require explicit linking?
- What are the specific "account access rules" for locked/inactive/restricted accounts (who sets
  them, what triggers them, what message is shown)?
- Is there a failed-attempt lockout or rate-limiting policy?
- Does signing in as a Tour Operator with a Pending Approval or Rejected application show any
  special messaging at sign-in time, or only after landing in the app?
- What does "session or JWT authentication information" concretely include (expiry, refresh
  mechanism) — likely an implementation detail, but session **expiry behavior visible to the
  user** may affect UX (e.g., "you were signed out" messaging).

# MVP Scope

**Suggestion** *(not explicit in source — for discussion)*:
- Email/password sign-in only for MVP; defer phone/OTP and Google Social Login as they each carry
  significant undefined behavior (OTP delivery, Google-identity matching).
