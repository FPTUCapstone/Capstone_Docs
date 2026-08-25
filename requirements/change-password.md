# Overview

Allows an authenticated user (Traveler, Tour Operator, or Administrator) to replace their current
account password from account settings. The user provides the existing password and a valid new
password. TripMate verifies the current credential and validates the new password before securely
updating the account credential.

# Actors

- **Traveler / Tour Operator / Administrator** (primary) — any authenticated user.
- **System (TripMate Platform)** — verifies the current password, validates the new password, and
  updates the credential.

# Preconditions

- The user is authenticated (has a valid session, per UC-04 Sign In). *(Explicit)*
- The user knows their current password. *(Explicit)*

# Main Flow

1. User navigates to account settings and initiates Change Password. *(Explicit)*
2. User provides their current password and a new password. *(Explicit)*
3. System verifies the current password is correct. *(Explicit)*
4. System validates the new password against the password policy. *(Explicit)*
5. System updates the account credential. *(Explicit)*
6. System confirms the password has been changed. *(Explicit)*

# Alternative Flows

None described in source — this is a single, linear action available to all authenticated roles.

# Exception Flows

- **EF1 — Current password incorrect:** System rejects the change and shows an error; the
  existing password remains unchanged. *(Explicit)*
- **EF2 — New password fails policy validation:** System rejects and shows the specific
  requirement not met (reusing the password policy from register-traveler-account.md BR4).
  *(Assumption — reuses existing policy, not restated)*
- **EF3 — New password same as current password:** Whether this is blocked or allowed is
  undefined. *(See Missing Information)*
- **EF4 — System/update failure:** System informs the user and does not partially update the
  credential. *(Assumption)*

# Postconditions

**Success:**
- The account's password has been updated to the new value.
- The user can continue using the current session, or must re-authenticate (undefined — see
  Missing Information).

**Failure:**
- The account's password is unchanged.
- The user is informed of the specific failure reason.

# Functional Requirements

- FR1: The system shall require the current password before allowing a change.
- FR2: The system shall validate the new password against the account password policy.
- FR3: The system shall verify the current password is correct before updating the credential.
- FR4: The system shall confirm successful password change to the user.
- FR5: The system shall be available to all authenticated roles (Traveler, Tour Operator,
  Administrator).

# Business Rules

- BR1: Changing the password requires knowledge of the current password — there is no
  "change without current password" path (that's Reset Password, UC-06, for forgotten
  passwords). *(Explicit)*
- BR2 *(assumption)*: The new password must meet the same policy as registration and reset (see
  BR4 in register-traveler-account.md).
- BR3: This use case is identical across all three roles — no role-specific behavior is
  described. *(Explicit)*

# Edge Cases

- User submits a new password identical to their current password — accept or reject? Undefined
  (EF3).
- User changes their password while signed in on multiple devices — are other sessions
  invalidated? Undefined (mirrors the same open question in sign-out.md BR2).
- User's account becomes locked/restricted between opening the Change Password form and
  submitting — undefined interaction.

# Missing Information

- Is the user required to re-authenticate (sign in again) after changing their password, or does
  the current session remain valid?
- Are other active sessions/devices signed out when the password changes?
- Is submitting the same password as the current one blocked or silently accepted (EF3)?
- Is there a confirmation notification (e.g., email) sent when the password changes, as a
  security measure?

# MVP Scope

**Suggestion** *(not explicit in source — for discussion)*:
- Current password + new password form, reusing the existing password policy validation, no
  session invalidation logic or notification email at MVP.
