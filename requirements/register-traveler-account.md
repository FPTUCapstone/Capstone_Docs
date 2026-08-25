# Use Case Analysis: Register Traveler Account

> Source use case (verbatim, unmodified):
> **Register Traveler Account** — Actor: Guest
> "Allows a new Traveler to create a TripMate account by providing required personal and authentication information such as full name, email address or phone number, and password. The system validates the submitted information, checks account uniqueness, and automatically assigns the account role as Traveler. If Google Social Login is supported, verified identity information may be obtained from Google Authentication Service during registration."

# Overview

A Guest (unauthenticated visitor) self-registers for a TripMate account, either by submitting personal/authentication details directly, or optionally via Google Social Login. On success, the system creates an account and assigns it the **Traveler** role automatically.

# Actors

- **Guest** (primary) — an unauthenticated visitor attempting to create an account. *(Explicit)*
- **TripMate System** — validates input, enforces uniqueness, creates the account, assigns role. *(Explicit, implied by "the system...")*
- **Google Authentication Service** (secondary/external) — supplies verified identity information when Social Login is used. *(Explicit, conditional: "If Google Social Login is supported")*

# Preconditions

- The Guest does not already hold a TripMate account with the same email or the same phone number. *(Assumption — required for "checks account uniqueness" to be meaningful; email/phone both being required and unique confirmed 2026-08-26, see BR2/BR3)*
- The Guest can reach the registration interface (web/app). *(Assumption)*
- If using Google Social Login: the Guest has a valid Google account and grants consent to share identity data. *(Assumption)*

# Main Flow

1. Guest initiates registration ("create a TripMate account"). *(Explicit)*
2. Guest submits required information: full name, email address, phone number, and password. *(Decided 2026-08-26 — both email and phone are required, superseding source's "or" phrasing; see BR3)*
3. System validates the submitted information. *(Explicit — validation rules themselves not specified, see Missing Information)*
4. System checks account uniqueness (no existing account with the same email/phone). *(Explicit)*
5. System creates the account and automatically assigns the role **Traveler**. *(Explicit)*
6. Registration completes. *(Assumption — outcome/confirmation to Guest not described)*

# Alternative Flows

**A1 – Google Social Login** *(Explicit, conditional on "If Google Social Login is supported")*
1. Guest chooses to register via Google Social Login instead of the manual form.
2. System requests verified identity information from the Google Authentication Service.
3. Google returns verified identity data (e.g., name, email). *(Assumption — exact fields returned not specified)*
4. System uses this verified data in place of (or to prefill) manually entered fields.
5. Flow rejoins Main Flow at uniqueness check / account creation (steps 4–5).

# Exception Flows

- **E1 – Validation failure**: Submitted information fails validation (e.g., malformed email, missing required field, password not meeting policy). System rejects submission and shows a clear inline error message (e.g., "Password is not 8 characters long"). *(Decided — error messaging confirmed by stakeholder during MVP planning, 2026-08-26; failure handling itself was previously assumed)*
- **E2 – Duplicate account**: Email or phone number already associated with an existing account. Registration is rejected with a clear inline error message (e.g., "Email already exists"). *(Decided — error messaging confirmed by stakeholder during MVP planning, 2026-08-26; uniqueness check itself is explicit in source)*
- **E3 – Google Authentication Service failure/unavailable**: The external service cannot be reached or returns an error. *(Assumption — not addressed in source)*
- **E4 – Guest cancels/denies Google consent**: Guest aborts the Google OAuth flow or denies data sharing. *(Assumption — not addressed in source)*

# Postconditions

- A new account exists in the system with role = **Traveler**. *(Explicit)*
- The Guest is redirected to the Login screen and must log in manually (no auto-login). *(Decided — provided by stakeholder during MVP planning, 2026-08-26)*

# Functional Requirements

- FR1: The system shall allow a Guest to submit full name, email address, phone number, and password to register. *(Decided 2026-08-26 — supersedes source's "email or phone" phrasing; see BR3)*
- FR2: The system shall validate submitted registration information before creating an account. *(Explicit)*
- FR3: The system shall enforce account uniqueness (reject registration if the email/phone is already in use). *(Explicit)*
- FR4: The system shall automatically assign the role "Traveler" to accounts created through this use case. *(Explicit)*
- FR5: If Google Social Login is supported, the system shall be able to obtain verified identity information from the Google Authentication Service during registration. *(Explicit, conditional)*

# Business Rules

- BR1: Self-registered accounts are always assigned the Traveler role (no role selection by the Guest). *(Explicit)*
- BR2: Both an account's email address and phone number must be unique across the system. *(Decided 2026-08-26 — supersedes prior "email or phone" reading; email is the unique login identifier, phone is unique but not used for login)*
- BR3: Full name, email address, phone number, and a password are all required fields for manual registration (both email and phone are mandatory, not either/or). *(Decided 2026-08-26 — resolves prior "email or phone" ambiguity)*
- BR4: Password must be at least 8 characters and include at least 1 uppercase letter, 1 lowercase letter, 1 number, and 1 special character. *(Decided — provided by stakeholder during MVP planning, 2026-08-26)*
- BR5: After successful registration, the Guest is redirected to the Login screen and must log in manually (no auto-login). *(Decided — provided by stakeholder during MVP planning, 2026-08-26)*
- BR6: Email address is the identifier used for login; phone number is stored for contact/verification purposes and is not a login credential. *(Decided — provided by stakeholder during MVP planning, 2026-08-26)*
- BR7: Terms of Service / Privacy Policy consent is presented as a text annotation with a link below the Register button (e.g., "By registering, you agree to the Terms & Privacy Policy"); no separate checkbox is required at MVP. *(Decided — provided by stakeholder during MVP planning, 2026-08-26)*

# Edge Cases

- ~~Guest supplies both email and phone — unclear whether both are stored or only one is required.~~ **Resolved 2026-08-26**: both are required and both stored. See BR2/BR3.
- Guest attempts registration with an email or phone already used by an account created via a different method (e.g., manual signup vs. Google login) — conflict resolution unclear.
- Google-provided email matches an existing manually-registered (non-Google) account.
- Concurrent registration attempts submitting the same email or phone simultaneously (race condition on uniqueness check).
- Phone number provided in varying formats/country codes without a stated normalization rule. *(Still open — see Missing Information)*
- Password provided does not meet the policy defined in BR4.

# Missing Information

- ~~Is email verification or phone number (OTP) verification required after registration, before the account becomes active?~~ **Resolved 2026-08-26**: deferred to Post-MVP, out of scope for MVP.
- ~~What are the password complexity/strength requirements?~~ **Resolved 2026-08-26**: min 8 characters, at least 1 uppercase, 1 lowercase, 1 number, 1 special character. See BR4.
- ~~Is providing **both** email and phone required, or is exactly one of the two sufficient (source says "email address or phone number")?~~ **Resolved 2026-08-26**: both are required and both unique; email is the login identifier, phone is for contact/verification only. See BR2/BR3/BR6.
- ~~What is the resolution when a Google-authenticated identity matches an existing manually-registered account (merge, block, or link)?~~ **Resolved 2026-08-26**: deferred to Post-MVP, out of scope for MVP (moot until Google Social Login is scoped in).
- ~~Is acceptance of Terms of Service / Privacy Policy required during registration?~~ **Resolved 2026-08-26**: yes, presented as a text annotation with a link below the Register button; no separate checkbox required at MVP. See BR7.
- ~~Are there anti-abuse measures (rate limiting, CAPTCHA) for registration?~~ **Resolved 2026-08-26**: deferred to Post-MVP, out of scope for MVP.
- What normalization rule (if any) applies to phone numbers submitted in varying formats/country codes? *(Still open — acceptable risk for MVP)*
- Exactly which fields are retrieved from the Google Authentication Service (email only, profile photo, etc.)? *(Still open — moot until Google Social Login is scoped in)*
- ~~Does the Guest become logged in automatically after registration, or must they log in separately afterward?~~ **Resolved 2026-08-26**: redirected to Login screen, no auto-login. See BR5.
- ~~What error messaging/behavior is expected on validation failure or duplicate account (E1/E2)?~~ **Resolved 2026-08-26**: clear inline messages, e.g. "Password is not 8 characters long", "Email already exists". See E1/E2.

# MVP Scope

See `requirements/mvp/register-traveler-account.md` for the full MVP plan. Summary of confirmed MVP decisions (2026-08-26):
- Manual registration only: full name + email + phone number + password (both email and phone required and unique), with automatic Traveler role assignment.
- Defer Google Social Login to a later iteration.
- Defer email/phone verification workflows.
- ToS/Privacy shown as a text annotation with a link; no checkbox required.
