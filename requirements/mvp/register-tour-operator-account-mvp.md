# MVP Goal

Let a Guest submit a complete Tour Operator business registration (identity + legal/business
info) in one sitting, so the business can enter the Pending Approval queue and — once an
Administrator approves it — start publishing tours and receiving bookings.

# Target User

Guest (unauthenticated visitor) who owns or represents a travel business and wants to sell tours
through TripMate.

# Core User Problem

A travel business has no way to get onto the TripMate platform as a seller. They need a single,
trustworthy path to submit their identity and legal credentials for review, without being
blocked by features (payment, multi-business, phone verification) that aren't required to prove
the core value: "submit once, get reviewed, get approved."

# Core User Journey

Guest fills the registration form (auth info + business/legal info + license file) → submits →
system validates and creates the account in Pending Approval status → Guest is told to wait for
Administrator approval. If rejected, the Guest corrects the same application and resubmits it,
re-entering the same review queue.

# Must Have

- Single registration form: email + password, Company Name, Tax Code, business contact details.
- Mandatory Travel Business License file upload (PDF or image), with format/size validation only
  — no content verification at this step. *(BR7)*
- Required-field and basic format validation before submission. *(EF1, EF3)*
- Duplicate authentication identifier (email) check. *(EF2, FR9)*
- Registration always creates a brand-new, independent account — no upgrade/linking from an
  existing account (e.g., Traveler). *(BR6)*
- Automatic Tour Operator role assignment on successful submission. *(FR4)*
- Automatic Pending Approval status on account creation. *(FR5)*
- Enforcement: a Pending Approval account cannot publish tours or receive bookings. *(FR6, FR7)*
- Submission confirmation notification to the Guest (e.g., email) stating the application is
  pending review. *(FR8)*
- Edit-and-resubmit flow for a rejected application, reusing the same application record — this
  closes the loop for a rejected Guest and is required for a complete core journey. *(BR8, FR11)*

# Should Have

- Duplicate business identity check (Tax Code / License number already used by another
  operator). Currently only an **assumption** (BR5), not stakeholder-confirmed — worth locking
  down before build, but the happy path doesn't strictly depend on it for MVP demo purposes.
- Uniqueness check on resubmission correctly excludes the Guest's own existing record (see Edge
  Cases in the requirement analysis) — needed once BR5 is confirmed, to avoid a Guest being
  blocked by their own rejected application.

# Could Have

- Jurisdiction-specific format validation for Tax Code / Travel Business License number (format
  rules not yet defined — see Open Questions).
- Specific accepted image formats (e.g., JPG/PNG) and a defined max file size for the license
  upload, beyond a reasonable placeholder default.
- Save-and-resume for a partially completed form. *(AF2, not confirmed as in-scope)*
- Cap on the number of resubmission attempts after rejection (anti-abuse).

# Out of Scope

- Upgrading or linking an existing account (e.g., Traveler) to Tour Operator — permanently out of
  scope by design, not a future-phase deferral. *(BR6)*
- Multi-business registration under a single Guest.
- Payment/fee collection at registration.
- Phone/OTP-based verification.
- The Administrator review/approval workflow itself (separate downstream use case).
- Automated content verification of the uploaded license file (OCR, authenticity checks).
- Double-submit / rate-limiting protection on the registration form.

# MVP User Journey

1. Guest opens the "Register as Tour Operator" form.
2. Guest enters email + password.
3. Guest enters Company Name, Tax Code, business contact details.
4. Guest uploads the Travel Business License file (PDF or image).
5. Guest submits.
6. System validates required fields, formats, and the uploaded file; blocks submission with a
   field-specific error if anything fails.
7. System creates a new, independent account, assigns the Tour Operator role, and sets status to
   Pending Approval.
8. System confirms submission and tells the Guest the account is awaiting Administrator approval.
9. **If later rejected** (downstream Admin use case): Guest reopens the same application, edits
   the failed fields/file, and resubmits — returning to step 6 on the same record.

# Dependencies

- File storage for uploaded license documents (even a minimal MVP needs somewhere to persist the
  file for the Administrator to later review).
- Notification/email delivery for the submission confirmation.
- The system-wide account/email uniqueness rule already established for Traveler registration
  (BR6 relies on it to make "always a new account" meaningful).
- The downstream Administrator Approval use case: without at least a minimal way for an Admin to
  approve/reject, every Tour Operator account is permanently stuck in Pending Approval and the
  MVP cannot demonstrate its core value end-to-end.

# Risks

- Admin Approval (out of scope here) not shipping in the same release window leaves this MVP
  unable to show a complete, provable outcome — accounts would never leave Pending Approval.
- BR5 (Tax Code/License uniqueness) is still an assumption; if wrong, duplicate/fraudulent
  business identities could pass registration and only get caught manually during Admin review.
- No defined format/size limit for the license upload (Could Have) risks inconsistent
  implementation choices if build starts before this is locked down.
- No resubmission cap (Could Have) could let a rejected Guest resubmit indefinitely without
  addressing the rejection reason.

# Open Questions

- Does "authentication information" mean email + password only, or also phone/OTP verification?
- What validation rules/format apply to Tax Code and Travel Business License number
  (jurisdiction-specific)?
- What exactly constitutes "business contact details" (phone, address, contact person name, or
  all of the above)?
- What are the exact accepted image formats and maximum file size for the license upload?
- Is there a cap on the number of resubmission attempts after rejection?
- Is email verification required, and if so, before or independently of Administrator approval?
- Is there a fee or payment step associated with Tour Operator registration?
- Can one Guest register multiple Tour Operator businesses (as separate accounts, per BR6)?
- What is the notification channel for approval/rejection outcomes (email, in-app, both)?
