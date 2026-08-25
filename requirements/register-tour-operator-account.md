# Overview

Allows a **Guest** to create a TripMate **Tour Operator** business account by submitting authentication credentials together with required business and legal information (Company Name, Tax Code, Travel Business License, business contact details). On submission, the system automatically assigns the **Tour Operator** role and creates the account with an application status of **Pending Approval**. The account is restricted — it cannot publish tours or receive customer bookings — until an **Administrator** approves the application. (Admin review/approval itself is a separate, downstream use case; this use case covers registration and submission only.)

# Actors

- **Guest** (primary) — an unauthenticated visitor registering a new Tour Operator business account.
- **Administrator** (secondary, referenced) — reviews and approves/rejects the application in a separate use case; not detailed here.
- **System (TripMate Platform)** — validates input, assigns the Tour Operator role, creates the account, sets Pending Approval status, and enforces publish/booking restrictions until approval.

# Preconditions

- The email/identifier the Guest registers with is not already in use by any existing TripMate account (Traveler or Tour Operator) — Tour Operator registration always creates a brand-new account; there is no upgrade path from an existing account. *(Decided 2026-08-26 — see BR6; consistent with the system-wide email uniqueness rule used for Traveler registration)*
- The Guest has access to the "Register as Tour Operator" registration form.
- The Guest has the required business/legal information available (Company Name, Tax Code, Travel Business License number, business contact details) **and** a scanned copy of the Travel Business License document ready to upload (e.g., PDF or image file). *(Decided 2026-08-26 — document upload is mandatory; see BR7)*

# Main Flow

1. Guest navigates to the Tour Operator registration form.
2. Guest enters authentication information (e.g., email and password).
3. Guest enters required business and legal information: Company Name, Tax Code, Travel Business License, business contact details, and uploads a scanned copy of the Travel Business License document. *(Decided 2026-08-26 — document upload is mandatory, see BR7)*
4. Guest submits the form.
5. System validates all submitted fields (required fields present, correct formats).
6. System creates the account and automatically assigns the **Tour Operator** role.
7. System sets the application/account status to **Pending Approval**.
8. System confirms submission to the Guest and communicates that the account is awaiting Administrator approval and cannot publish tours or receive bookings yet.

# Alternative Flows

- **AF1 — Guest already has a TripMate account (e.g., Traveler role):** The Guest must use a different email/identifier than any existing account. Tour Operator registration always creates a new, separate account — there is no upgrade path from an existing Traveler (or any other) account. *(Decided 2026-08-26 — see BR6)*
- **AF2 — Save and resume later:** If supported, the Guest can save a partially completed form and return to finish it before final submission. *(Not confirmed as in-scope — see Missing Information.)*
- **AF3 — Resubmission after rejection:** If the Administrator rejects the application, the Guest edits the same (existing) application and resubmits it — no new registration/account record is created. *(Decided 2026-08-26 — see BR8)*

# Exception Flows

- **EF1 — Missing required fields:** System blocks submission and displays validation errors for incomplete fields (auth info or business/legal info).
- **EF2 — Duplicate authentication identifier:** Email (or other login identifier) is already registered. System rejects submission with an appropriate error.
- **EF3 — Invalid format:** Tax Code, Travel Business License, or contact details fail format validation. System rejects submission with field-level errors.
- **EF4 — Duplicate business identity:** Tax Code or Travel Business License number matches an already-registered operator. System rejects submission (assumes uniqueness is enforced — see Business Rules).
- **EF5 — System/submission failure:** A network or server error occurs during submission. System informs the Guest and does not create a partial/corrupt account record.
- **EF6 — Invalid or missing document upload:** The Travel Business License file is missing, an unsupported format, too large, or corrupted. System rejects submission and prompts the Guest for a valid file. *(Decided 2026-08-26 — logical consequence of BR7; exact accepted formats/size limit still open, see Missing Information)*

# Postconditions

**Success:**
- A new account exists with the Tour Operator role assigned.
- Application status is Pending Approval.
- The account cannot publish tours or receive customer bookings until an Administrator approves it.
- The Guest has received confirmation of submission.

**Failure:**
- No account is created.
- The Guest is informed of the specific reason for failure (validation error, duplicate identifier, duplicate business identity, or system error).

# Functional Requirements

- FR1: The system shall provide a registration form capturing authentication information and business/legal information in a single flow.
- FR2: The system shall require Company Name, Tax Code, Travel Business License, business contact details, and a scanned Travel Business License document as mandatory fields. *(Decided 2026-08-26 — supersedes prior text-only assumption; see BR7)*
- FR3: The system shall validate all required fields before allowing submission.
- FR4: The system shall automatically assign the Tour Operator role to the account upon successful submission — no manual role selection or admin action required at this step.
- FR5: The system shall set the application status to Pending Approval immediately upon successful account creation.
- FR6: The system shall prevent an account with Pending Approval status from publishing tours.
- FR7: The system shall prevent an account with Pending Approval status from receiving customer bookings.
- FR8: The system shall notify the Guest that their submission was received and is pending Administrator approval.
- FR9: The system shall reject registration if the authentication identifier (e.g., email) is already in use.
- FR10: The system shall always create a new account for Tour Operator registration; it shall not modify or extend an existing account (e.g., an existing Traveler account). *(Decided 2026-08-26 — see BR6)*
- FR11: When an Administrator rejects an application, the system shall allow the Guest to edit and resubmit the same application rather than requiring a new registration. *(Decided 2026-08-26 — see BR8)*

# Business Rules

- BR1: Every Tour Operator registration is automatically assigned the Tour Operator role — no other role can result from this use case.
- BR2: Every new Tour Operator application starts in Pending Approval status; no self-service or automatic approval path exists.
- BR3: An account in Pending Approval status is restricted from publishing tours and from receiving customer bookings, regardless of any other account activity.
- BR4: Company Name, Tax Code, Travel Business License, and business contact details are mandatory for registration.
- BR5 *(assumption)*: Tax Code and Travel Business License number must be unique per Tour Operator account — duplicates are rejected.
- BR6: Tour Operator registration always creates a new, separate account; there is no upgrade/extension path from an existing account (e.g., Traveler) to Tour Operator. *(Decided — provided by stakeholder, 2026-08-26)*
- BR7: A scanned copy of the Travel Business License (e.g., PDF or image) is a mandatory upload for Tour Operator registration. *(Decided — provided by stakeholder, 2026-08-26; accepted file formats and size limit not yet specified, see Missing Information)*
- BR8: A rejected Tour Operator application can be edited and resubmitted by the Guest using the same application record; rejection does not require a brand-new registration. *(Decided — provided by stakeholder, 2026-08-26)*

# Edge Cases

- Guest submits a Tax Code or Travel Business License number that matches an existing **rejected** (not approved) application — should this be allowed to retry, or treated as a duplicate block?
- Guest is currently authenticated as another role (e.g., logged-in Traveler) and attempts to reach the Tour Operator registration form.
- Company Name or contact details contain non-ASCII characters, emojis, or exceed expected field length.
- Guest submits the form multiple times in quick succession (double-submit / duplicate application risk).
- Administrator never acts on the application (indefinite Pending Approval) — no timeout/expiry behavior defined.
- Guest's business contact details differ from their own personal authentication contact info (e.g., different email/phone) — is that permitted?

# Missing Information

- Does "authentication information" mean email + password only, or also phone number/OTP verification?
- ~~Is a document upload (e.g., scanned Travel Business License, proof of Tax Code) required, or are these free-text fields only?~~ **Resolved 2026-08-26**: yes, a scanned Travel Business License upload is mandatory; Tax Code remains a free-text/number field (no separate proof-of-tax-code document decided). See BR7.
- What validation rules/format apply to Tax Code and Travel Business License number (jurisdiction-specific)?
- What file formats and maximum size are accepted for the Travel Business License upload? *(New — follow-up from the BR7 decision, still open)*
- ~~Can an existing Traveler account be upgraded/extended to Tour Operator, or must this always be a brand-new account?~~ **Resolved 2026-08-26**: always a brand-new, separate account; no upgrade path. See BR6.
- What exactly constitutes "business contact details" (phone, address, contact person name, all of the above)?
- ~~What happens on rejection — can the Guest edit and resubmit the same application, or must they start over?~~ **Resolved 2026-08-26**: the Guest edits and resubmits the same application. See BR8.
- Is email verification required, and if so, does it happen before or independently of Administrator approval?
- Is there a fee or payment step associated with Tour Operator registration?
- Can one Guest/person register multiple Tour Operator businesses under different accounts?
- What is the notification channel for approval/rejection outcomes (email, in-app, both)?

# MVP Scope

**In scope for MVP:**
- Single registration form capturing authentication info + Company Name, Tax Code, Travel Business License, business contact details.
- Mandatory upload of a scanned Travel Business License document (storage only — automated content verification is out of scope; a human Administrator reviews it in the separate downstream approval use case). *(Decided 2026-08-26 — see BR7)*
- Field-level validation for required fields and basic format checks.
- Automatic Tour Operator role assignment on submission.
- Automatic Pending Approval status on account creation.
- Enforcement: Pending Approval accounts cannot publish tours or receive bookings.
- Basic confirmation notification (e.g., email) that the application was submitted and is pending review.
- Duplicate authentication identifier check (email already registered).
- Edit-and-resubmit flow for rejected applications — the Guest corrects and resubmits the same application record. *(Decided 2026-08-26 — see BR8; exact screen/UI mechanics to be detailed in Screen Specification)*

**Permanently out of scope (by design, not a future phase):**
- Upgrading/extending an existing Traveler (or any other) account to Tour Operator — registration always creates a brand-new account. *(Decided 2026-08-26 — see BR6)*

**Deferred / out of scope for MVP:**
- Automated verification of the uploaded Travel Business License content (OCR, authenticity checks, etc.) — verification stays manual via the Administrator approval use case.
- Multi-business registration under a single Guest.
- Payment/fee collection at registration.
- Phone/OTP-based verification.
- Administrator review/approval workflow itself (separate use case).
