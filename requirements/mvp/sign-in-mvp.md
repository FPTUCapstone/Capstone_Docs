# MVP Goal

Let any existing TripMate account holder — Traveler, Tour Operator, or Administrator — get
authenticated and land in the app with the correct role-based access, using one reliable method.

# Target User

Guest holding a registered account of any role (Traveler, Tour Operator, Administrator).

# Core User Problem

Every other use case that requires "an authenticated user" (UC-03, UC-05 through UC-10, and more)
is blocked until sign-in exists. It is the single highest-leverage, most foundational flow in the
whole product.

# Core User Journey

Enter credentials → system validates → system checks account status → session issued → user
enters the app in their assigned role.

# Must Have

- Email/password sign-in. *(AF1)*
- Credential validation with a generic "invalid credentials" error (not confirming which part was
  wrong) — *(assumption, standard security practice)*.
- Account status check (locked/inactive/restricted) that blocks sign-in independently of
  credential correctness. *(BR2, FR4)*
- Role resolution (Traveler/Tour Operator/Administrator) after successful authentication. *(FR3)*
- Session/JWT issuance only after both credential and status checks pass. *(FR5)*
- Sign-in must succeed for a Tour Operator with a Pending Approval or Rejected application — it is
  a hard dependency for UC-03. *(BR3)*

# Should Have

- Phone number/OTP sign-in — a second supported method named in the source, but carries several
  undefined mechanics (delivery, expiry) that should be resolved before or during build, not
  blocking a first MVP release.

# Could Have

- Google Social Login — most complex method: undefined identity-matching behavior for both
  "no matching account" and "matches a manually-registered account" cases (also open in the
  Traveler registration analysis). Reasonable to defer until those are resolved.
- Failed-attempt lockout / rate limiting.

# Out of Scope

- Any UI/flow for what happens after a Google identity has no matching account (registration
  triggered from sign-in) — that would be new scope, not part of this use case.
- Session expiry / refresh UX (e.g., "you were signed out" messaging) — implementation-adjacent,
  not confirmed as a requirement here.

# MVP User Journey

1. Guest opens Sign In.
2. Guest enters email + password.
3. System validates credentials.
4. System checks account status (locked/inactive/restricted).
5. On success: system resolves role, issues session/JWT, grants role-based access.
6. On failure: generic error shown, no session issued.

# Dependencies

- Every other authenticated use case in this batch (UC-03, UC-05, UC-06 for the reset link's
  eventual re-entry, UC-07, UC-08, UC-09, UC-10) depends on this one existing first.
- Depends on account creation flows (register-traveler-account, register-tour-operator-account)
  already producing accounts with credentials to sign in with.

# Risks

- This is the most foundational use case in the batch — if its Missing Information items (account
  access rules, Google identity matching) aren't resolved early, every downstream authenticated
  flow inherits the same ambiguity.
- BR3 (Tour Operator with Pending/Rejected status can still sign in) is an assumption I made to
  keep UC-03 reachable; if wrong, UC-03's entire premise (sign in to resubmit) breaks.

# Open Questions

- OTP delivery mechanism, format, and expiry time.
- Google identity matching behavior (no match / matches manual account).
- Concrete "account access rules" for locked/inactive/restricted accounts.
- Failed-attempt lockout or rate-limiting policy.
- Whether Pending Approval/Rejected Tour Operator status shows special messaging at sign-in time.
