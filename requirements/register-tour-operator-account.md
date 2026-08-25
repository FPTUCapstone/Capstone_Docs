# Overview

Allows a **Guest** to create a TripMate **Tour Operator** business account by submitting authentication credentials together with required business and legal information (Company Name, Tax Code, Travel Business License, business contact details). On submission, the system automatically assigns the **Tour Operator** role and creates the account with an application status of **Pending Approval**. The account is restricted — it cannot publish tours or receive customer bookings — until an **Administrator** approves the application. (Admin review/approval itself is a separate, downstream use case; this use case covers registration and submission only.)

# Actors

- **Guest** (primary) — an unauthenticated visitor registering a new Tour Operator business account.
- **Administrator** (secondary, referenced) — reviews and approves/rejects the application in a separate use case; not detailed here.
- **System (TripMate Platform)** — validates input, assigns the Tour Operator role, creates the account, sets Pending Approval status, and enforces publish/booking restrictions until approval.

# Preconditions

- The Guest does not currently hold an active TripMate account under the email/identifier they are registering with. *(Decided 2026-08-26 — see BR6: registration always creates a fully new, independent account regardless of any existing account.)*
- The Guest has access to the "Register as Tour Operator" registration form.
- The Guest has the required business/legal information available (Company Name, Tax Code, Travel Business License number, business contact details).

# Main Flow

1. Guest navigates to the Tour Operator registration form.
2. Guest enters authentication information (e.g., email and password).
3. Guest enters required business and legal information (Company Name, Tax Code, business contact details) and uploads a scanned copy of the Travel Business License (PDF or image file). *(Decided 2026-08-26 — see BR7.)*
4. Guest submits the form.
5. System validates all submitted fields (required fields present, correct formats).
6. System creates the account and automatically assigns the **Tour Operator** role.
7. System sets the application/account status to **Pending Approval**.
8. System confirms submission to the Guest and communicates that the account is awaiting Administrator approval and cannot publish tours or receive bookings yet.

# Alternative Flows

- **AF1 — Guest already has a TripMate account (e.g., Traveler role):** *(Decided 2026-08-26)* No upgrade or linking path exists. The Guest goes through the standard Main Flow and a fully new, independent Tour Operator account is created, unrelated to any existing account. See BR6.
- **AF2 — Save and resume later:** If supported, the Guest can save a partially completed form and return to finish it before final submission. *(Not confirmed as in-scope — see Missing Information.)*
- **AF3 — Resubmission after rejection:** *(Decided 2026-08-26)* When the Administrator rejects the application (in the downstream approval use case), the Guest edits the fields (including re-uploading the Travel Business License if needed) on the **same, existing application record** and resubmits it. This does not create a new registration and does not reset the account's identity. See BR8.

# Exception Flows

- **EF1 — Missing required fields:** System blocks submission and displays validation errors for incomplete fields (auth info or business/legal info).
- **EF2 — Duplicate authentication identifier:** Email (or other login identifier) is already registered. System rejects submission with an appropriate error.
- **EF3 — Invalid format:** Tax Code, Travel Business License, or contact details fail format validation. System rejects submission with field-level errors.
- **EF4 — Duplicate business identity:** Tax Code or Travel Business License number matches an already-registered operator. System rejects submission (assumes uniqueness is enforced — see Business Rules).
- **EF5 — System/submission failure:** A network or server error occurs during submission. System informs the Guest and does not create a partial/corrupt account record.
- **EF6 — Invalid license file:** *(Decided 2026-08-26)* Uploaded Travel Business License file is missing, an unsupported format (not PDF/image), or exceeds the allowed size. System blocks submission and shows a file-specific error. See BR7.

# Postconditions

**Success:**
- A new account exists with the Tour Operator role assigned.
- Application status is Pending Approval.
- The account cannot publish tours or receive customer bookings until an Administrator approves it.
- The Guest has received confirmation of submission.

**Failure (at initial submission):**
- No account is created.
- The Guest is informed of the specific reason for failure (validation error, duplicate identifier, duplicate business identity, invalid license file, or system error).

**Failure (Administrator rejects, downstream use case):** *(Decided 2026-08-26)*
- The application record persists in a Rejected state (not deleted).
- The account remains restricted from publishing tours and receiving bookings.
- The Guest can edit that same application record and resubmit it for re-review. See BR8.

# Functional Requirements

- FR1: The system shall provide a registration form capturing authentication information and business/legal information in a single flow.
- FR2: The system shall require Company Name, Tax Code, business contact details, and an uploaded Travel Business License file (PDF or image) as mandatory fields. *(Updated 2026-08-26 — see BR7.)*
- FR3: The system shall validate all required fields before allowing submission.
- FR4: The system shall automatically assign the Tour Operator role to the account upon successful submission — no manual role selection or admin action required at this step.
- FR5: The system shall set the application status to Pending Approval immediately upon successful account creation.
- FR6: The system shall prevent an account with Pending Approval status from publishing tours.
- FR7: The system shall prevent an account with Pending Approval status from receiving customer bookings.
- FR8: The system shall notify the Guest that their submission was received and is pending Administrator approval.
- FR9: The system shall reject registration if the authentication identifier (e.g., email) is already in use.
- FR10: The system shall accept only PDF or image file formats (within a defined size limit) for the Travel Business License upload and reject other formats. *(Decided 2026-08-26 — see BR7. Exact size limit and image formats accepted are still open — see Missing Information.)*
- FR11: The system shall allow a Guest whose application was rejected by an Administrator to edit and resubmit the same application record, rather than creating a new registration. *(Decided 2026-08-26 — see BR8.)*

# Business Rules

- BR1: Every Tour Operator registration is automatically assigned the Tour Operator role — no other role can result from this use case.
- BR2: Every new Tour Operator application starts in Pending Approval status; no self-service or automatic approval path exists.
- BR3: An account in Pending Approval status is restricted from publishing tours and from receiving customer bookings, regardless of any other account activity.
- BR4: Company Name, Tax Code, an uploaded Travel Business License file, and business contact details are mandatory for registration. *(Updated 2026-08-26 — License is a file upload per BR7, not a text field)*
- BR5 *(assumption)*: Tax Code and Travel Business License number must be unique per Tour Operator account — duplicates are rejected.
- BR6 *(Decided 2026-08-26)*: Tour Operator registration always creates a new, fully independent account. There is no upgrade, linking, or merge path from any existing TripMate account (including an existing Traveler account) — registering as a Tour Operator never modifies or extends an existing account.
- BR7 *(Decided 2026-08-26)*: The Travel Business License must be submitted as an uploaded scanned file (PDF or image); free-text entry alone does not satisfy this requirement. Content verification of the uploaded file is performed manually by an Administrator in the downstream approval use case — no automated verification (e.g., OCR, authenticity check) is part of MVP.
- BR8 *(Decided 2026-08-26)*: When an Administrator rejects an application, the Guest edits and resubmits the same application record. Resubmission does not create a new registration, a new account, or a new application ID.

# Edge Cases

- Guest submits a Tax Code or Travel Business License number that matches their **own** existing rejected application (now being resubmitted) vs. matching a **different** applicant's record — the uniqueness check (EF4/BR5) must exclude the Guest's own record during resubmission.
- Guest is currently authenticated as another role (e.g., logged-in Traveler) and attempts to reach the Tour Operator registration form — since BR6 guarantees no linking, this should behave identically to an unauthenticated Guest.
- Company Name or contact details contain non-ASCII characters, emojis, or exceed expected field length.
- Guest submits the form multiple times in quick succession (double-submit / duplicate application risk).
- Administrator never acts on the application (indefinite Pending Approval) — no timeout/expiry behavior defined.
- Guest's business contact details differ from their own personal authentication contact info (e.g., different email/phone) — is that permitted?
- Guest resubmits a rejected application repeatedly without addressing the rejection reason — no cap on resubmission attempts is defined (see Missing Information).
- Uploaded license file passes format/size checks (EF6) but is corrupted or unreadable when the Administrator later opens it — out of scope for this use case, but a risk for the downstream approval use case.

# Missing Information

**Resolved 2026-08-26** *(kept here for traceability — see linked Business Rules for the decisions):*
- ~~Is a document upload required for the Travel Business License, or free-text only?~~ → Resolved: mandatory file upload (PDF or image). See BR7.
- ~~Can an existing Traveler account be upgraded/extended to Tour Operator?~~ → Resolved: no, always a fully new account. See BR6.
- ~~What happens on rejection — edit and resubmit, or start over?~~ → Resolved: edit and resubmit the same application record. See BR8.

**Still open:**
- Does "authentication information" mean email + password only, or also phone number/OTP verification?
- What validation rules/format apply to Tax Code and Travel Business License number (jurisdiction-specific)?
- What exactly constitutes "business contact details" (phone, address, contact person name, all of the above)?
- What are the exact accepted image formats (e.g., JPG/PNG) and maximum file size for the Travel Business License upload?
- Is there a cap on the number of resubmission attempts after rejection?
- Is email verification required, and if so, does it happen before or independently of Administrator approval?
- Is there a fee or payment step associated with Tour Operator registration?
- Can one Guest/person register multiple Tour Operator businesses (as multiple, fully separate accounts, per BR6)?
- What is the notification channel for approval/rejection outcomes (email, in-app, both)?

# MVP Scope

**In scope for MVP:**
- Single registration form capturing authentication info + Company Name, Tax Code, business contact details.
- Mandatory Travel Business License file upload (PDF or image), with format/size validation only — content verification is manual and happens in the downstream Admin approval use case. *(Decided 2026-08-26 — BR7.)*
- Field-level validation for required fields and basic format checks.
- Automatic Tour Operator role assignment on submission.
- Automatic Pending Approval status on account creation, always as a brand-new independent account. *(Decided 2026-08-26 — BR6.)*
- Enforcement: Pending Approval accounts cannot publish tours or receive bookings.
- Basic confirmation notification (e.g., email) that the application was submitted and is pending review.
- Duplicate authentication identifier check (email already registered).
- Edit-and-resubmit flow for a rejected application (same application record, not a new registration). *(Decided 2026-08-26 — BR8.)*

**Deferred / out of scope for MVP:**
- Multi-business registration under a single Guest.
- Payment/fee collection at registration.
- Phone/OTP-based verification.
- Administrator review/approval workflow itself (separate use case).
- Automated content verification of the uploaded license file (OCR, authenticity checks).

**Permanently out of scope (not just MVP):** *(Decided 2026-08-26)*
- Upgrading or linking an existing account (e.g., Traveler) to Tour Operator — no such path will exist. See BR6.
