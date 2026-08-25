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

- The Guest does not already hold a TripMate account with the same email/phone. *(Assumption — required for "checks account uniqueness" to be meaningful, not stated explicitly)*
- The Guest can reach the registration interface (web/app). *(Assumption)*
- If using Google Social Login: the Guest has a valid Google account and grants consent to share identity data. *(Assumption)*

# Main Flow

1. Guest initiates registration ("create a TripMate account"). *(Explicit)*
2. Guest submits required information: full name, email address **or** phone number, and password. *(Explicit)*
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

- **E1 – Validation failure**: Submitted information fails validation (e.g., malformed email, missing required field). System rejects submission and (assumed) returns an error to the Guest. *(Partly explicit: validation is mentioned; failure handling is assumed)*
- **E2 – Duplicate account**: Email or phone number already associated with an existing account. Registration is rejected. *(Assumption — uniqueness check is explicit, but the rejection behavior is assumed)*
- **E3 – Google Authentication Service failure/unavailable**: The external service cannot be reached or returns an error. *(Assumption — not addressed in source)*
- **E4 – Guest cancels/denies Google consent**: Guest aborts the Google OAuth flow or denies data sharing. *(Assumption — not addressed in source)*

# Postconditions

- A new account exists in the system with role = **Traveler**. *(Explicit)*
- (Assumed) The Guest is either automatically authenticated or directed to log in / verify the account. *(Assumption — not stated; see Missing Information)*

# Functional Requirements

- FR1: The system shall allow a Guest to submit full name, (email address or phone number), and password to register. *(Explicit)*
- FR2: The system shall validate submitted registration information before creating an account. *(Explicit)*
- FR3: The system shall enforce account uniqueness (reject registration if the email/phone is already in use). *(Explicit)*
- FR4: The system shall automatically assign the role "Traveler" to accounts created through this use case. *(Explicit)*
- FR5: If Google Social Login is supported, the system shall be able to obtain verified identity information from the Google Authentication Service during registration. *(Explicit, conditional)*

# Business Rules

- BR1: Self-registered accounts are always assigned the Traveler role (no role selection by the Guest). *(Explicit)*
- BR2: An account's email or phone number must be unique across the system. *(Explicit)*
- BR3: Full name, an identifier (email or phone), and a password are the minimum required fields for manual registration. *(Explicit)*

*No additional business rules (e.g., password complexity, verification requirements) are invented here — see Missing Information.*

# Edge Cases

- Guest supplies both email and phone — unclear whether both are stored or only one is required. *(See Missing Information)*
- Guest attempts registration with an email/phone already used by an account created via a different method (e.g., manual signup vs. Google login) — conflict resolution unclear.
- Google-provided email matches an existing manually-registered (non-Google) account.
- Concurrent registration attempts submitting the same email/phone simultaneously (race condition on uniqueness check).
- Phone number provided in varying formats/country codes without a stated normalization rule.
- Password provided does not meet an (unstated) strength policy.

# Missing Information

- Is email verification or phone number (OTP) verification required after registration, before the account becomes active?
- What are the password complexity/strength requirements?
- Is providing **both** email and phone required, or is exactly one of the two sufficient (source says "email address or phone number")?
- What is the resolution when a Google-authenticated identity matches an existing manually-registered account (merge, block, or link)?
- Is acceptance of Terms of Service / Privacy Policy required during registration?
- Are there anti-abuse measures (rate limiting, CAPTCHA) for registration?
- Exactly which fields are retrieved from the Google Authentication Service (email only, profile photo, etc.)?
- Does the Guest become logged in automatically after registration, or must they log in separately afterward?
- What error messaging/behavior is expected on validation failure or duplicate account (E1/E2)?

# MVP Scope

**Suggestion** *(not explicit in source — for discussion)*:
- Manual registration only: full name + email + password, with uniqueness check on email and automatic Traveler role assignment.
- Defer Google Social Login and phone-number-based registration to a later iteration unless already confirmed as must-have for launch.
- Defer email/phone verification workflows unless a compliance or security requirement mandates them for MVP.
