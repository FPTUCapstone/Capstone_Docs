# Overview

Allows a Traveler to update supported personal profile information such as full name, avatar,
phone number, email address, and other configurable profile fields. TripMate validates the
updated values before saving them to the Traveler profile. This use case modifies profile
information but does not allow the Traveler to change the system-assigned account role.

# Actors

- **Traveler** (primary) — an authenticated user with the Traveler role.
- **System (TripMate Platform)** — validates and saves updated profile values.

# Preconditions

- The Traveler is authenticated (UC-04 Sign In). *(Explicit)*
- The Traveler's account has existing profile data (from registration, per
  register-traveler-account.md). *(Explicit)*

# Main Flow

1. Traveler navigates to their profile settings. *(Explicit)*
2. System displays the current profile values. *(Explicit)*
3. Traveler edits one or more supported fields (full name, avatar, phone number, email address,
   or other configurable fields). *(Explicit)*
4. Traveler submits the changes. *(Explicit)*
5. System validates the updated values. *(Explicit)*
6. System saves the updated profile. *(Explicit)*
7. System confirms the update. *(Explicit)*

# Alternative Flows

- **AF1 — Traveler updates only a subset of fields:** Explicit — the source describes multiple
  independently updatable fields, implying partial updates are allowed.
- **AF2 — Traveler updates email address:** Since email is the login identifier (per BR6 in
  register-traveler-account.md), changing it may require re-verification. Undefined. *(See
  Missing Information)*

# Exception Flows

- **EF1 — Invalid field value:** System rejects the specific field and shows a validation error
  (e.g., malformed email/phone, name too long). *(Assumption on exact rules — validation itself
  is explicit, specific rules are not)*
- **EF2 — New email already in use by another account:** System rejects, reusing the uniqueness
  rule from register-traveler-account.md BR2. *(Assumption — reuses existing rule)*
- **EF3 — New phone already in use by another account:** Same reasoning as EF2. *(Assumption)*
- **EF4 — System/save failure:** System informs the Traveler and does not partially save the
  update. *(Assumption)*

# Postconditions

**Success:**
- The Traveler's profile reflects the updated values.
- The account role remains unchanged (Traveler). *(Explicit)*

**Failure:**
- The profile remains unchanged.
- The Traveler is informed of the specific validation failure.

# Functional Requirements

- FR1: The system shall display the Traveler's current profile values for editing.
- FR2: The system shall allow updating full name, avatar, phone number, email address, and other
  configurable profile fields.
- FR3: The system shall validate updated values before saving.
- FR4: The system shall not allow this use case to change the Traveler's account role.
- FR5: The system shall confirm a successful update.

# Business Rules

- BR1: Updating a profile never changes the account's system-assigned role. *(Explicit)*
- BR2 *(assumption)*: Email and phone uniqueness rules (BR2 in register-traveler-account.md)
  apply equally when updating these fields, not just at registration.
- BR3 *(assumption)*: "Other configurable profile fields" beyond the four named ones are not
  specified — do not invent additional fields without confirmation.

# Edge Cases

- Traveler changes their email to one already used by another account — see EF2.
- Traveler changes their email address itself — does this affect their login identifier
  immediately, or require re-verification first (mirrors the account-security concern in
  reset-password.md)?
- Traveler uploads an avatar image — file type/size limits undefined.
- Traveler clears a field that was previously required at registration (e.g., empties full name)
  — is it still required post-registration, or can it become blank?

# Missing Information

- What exactly are "other configurable profile fields" beyond full name, avatar, phone, email?
- Does changing the email address require re-verification (similar concern to registration's
  deferred email verification)?
- What are the avatar upload constraints (file type, size, dimensions)?
- Is there a confirmation step or notification when contact info (email/phone) changes, as a
  security measure?

# MVP Scope

**Suggestion** *(not explicit in source — for discussion)*:
- Full name and phone number editable at MVP; defer avatar upload (file handling) and email
  change (re-verification complexity) to a later phase.
