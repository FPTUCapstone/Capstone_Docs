# Overview

Allows a **Tour Operator** whose application has been rejected to sign in with their existing
account, review the Administrator's rejection reason, correct the invalid or insufficient
business/legal information and documents, and submit the application again — without registering
a new account. On successful resubmission, the application returns to **Pending Approval**
status. The account remains unable to publish tours or receive customer bookings until an
Administrator approves the resubmitted application.

# Actors

- **Tour Operator** (primary) — an account holder whose most recent Tour Operator application was
  rejected by an Administrator.
- **Administrator** (secondary, referenced) — rejected the original application, and will later
  review the resubmission; both are separate, downstream use cases, not detailed here.
- **System (TripMate Platform)** — authenticates the Tour Operator, displays the rejection reason,
  validates the corrected submission, and updates the application status.

# Preconditions

- The Tour Operator holds an existing TripMate account created via Tour Operator registration
  (see [register-tour-operator-account.md](register-tour-operator-account.md)). *(Explicit)*
- The account's most recent application is in **Rejected** status. *(Explicit)*
- The Tour Operator can authenticate using a supported Sign In method. *(Explicit — depends on
  UC-04 Sign In, not detailed here)*

# Main Flow

1. Tour Operator signs in with their existing account. *(Explicit — see UC-04 Sign In)*
2. System authenticates the Tour Operator and confirms the account's application is Rejected.
   *(Explicit)*
3. System displays the Administrator's rejection reason. *(Explicit)*
4. Tour Operator reviews the rejection reason together with the previously submitted
   business/legal information and documents. *(Explicit)*
5. Tour Operator corrects the invalid or insufficient fields and/or re-uploads documents.
   *(Explicit)*
6. Tour Operator submits the corrected application. *(Explicit)*
7. System validates the corrected submission. *(Explicit)*
8. System sets the application status back to **Pending Approval**. *(Explicit)*
9. System confirms the resubmission and reiterates that the account remains restricted (cannot
   publish tours or receive bookings) until Administrator approval. *(Explicit)*

# Alternative Flows

- **AF1 — Account's application is not Rejected when the Tour Operator signs in** (e.g., already
  Pending Approval or Approved): whether the resubmission entry point is blocked, hidden, or
  redirected elsewhere is undefined. *(See Missing Information)*
- **AF2 — Partial correction:** whether the Tour Operator can resubmit having corrected only some
  of the flagged issues, or must address all of them first, is undefined. *(See Missing
  Information)*

# Exception Flows

- **EF1 — Invalid credentials at Sign In:** handled entirely by UC-04 Sign In; not detailed here.
- **EF2 — Corrected submission still fails validation:** System blocks resubmission and shows
  field-level errors, reusing the same validation rules as initial registration. *(Assumption —
  reuses EF1/EF3/EF6 from register-tour-operator-account.md; not restated as new rules)*
- **EF3 — System/submission failure during resubmission:** System informs the Tour Operator and
  does not change the application's status (stays Rejected). *(Assumption)*

# Postconditions

**Success:**
- The same, existing application record is updated with the corrected information/documents.
- Application status is Pending Approval.
- The account remains restricted from publishing tours or receiving bookings until Administrator
  approval.
- The Tour Operator receives confirmation of the resubmission.

**Failure:**
- The application record remains in Rejected status, unchanged.
- The Tour Operator is informed of the specific validation failure.

# Functional Requirements

- FR1: The system shall require the Tour Operator to be authenticated before accessing the
  resubmission flow.
- FR2: The system shall only allow resubmission when the account's most recent application is in
  Rejected status. *(Assumption on enforcement — implied by scope, not explicitly stated)*
- FR3: The system shall display the Administrator's rejection reason to the Tour Operator.
- FR4: The system shall allow the Tour Operator to edit previously submitted business/legal
  information and re-upload documents.
- FR5: The system shall validate the corrected submission using the same rules as initial
  registration.
- FR6: The system shall set the application status to Pending Approval upon successful
  resubmission.
- FR7: The system shall keep the account restricted from publishing tours/receiving bookings after
  resubmission, until Administrator approval.
- FR8: The system shall not create a new account or a new application record during resubmission.

# Business Rules

- BR1 *(assumption)*: Resubmission is only accessible to an authenticated Tour Operator whose
  latest application is Rejected.
- BR2: Resubmission updates the existing application record; it never creates a new account or
  application ID. *(Explicit — consistent with BR8 in register-tour-operator-account.md)*
- BR3: A resubmitted application always re-enters Pending Approval status; there is no
  auto-approval path. *(Explicit — consistent with BR2 in register-tour-operator-account.md)*

# Edge Cases

- Tour Operator signs in but their latest application is Approved, not Rejected — is the
  resubmission entry point even reachable?
- Tour Operator signs in but has never been rejected (e.g., still Pending Approval) — same
  question as above.
- Tour Operator addresses the rejection reason for one field but changes a different, unrelated
  field instead — no cross-check between rejection reason and corrected fields is defined.
- Multiple rejection cycles for the same account — is prior rejection history retained and
  visible, or only the latest reason?
- Tax Code/License uniqueness check on resubmission must exclude the Tour Operator's own record
  (already flagged as an open point on BR5 in register-tour-operator-account.md).

# Missing Information

- What happens if a Tour Operator without a Rejected application accesses this flow (blocked,
  redirected, hidden entry point)?
- Must all flagged issues be corrected before resubmission is allowed, or is partial correction
  accepted?
- Is there a limit on the number of resubmission cycles?
- Is full rejection history visible to the Tour Operator, or only the most recent reason?
- Does the Administrator see a diff/highlight of what changed since the last rejected version (a
  downstream Admin Approval concern, but affects what data this use case must preserve)?

# MVP Scope

**Suggestion** *(not explicit in source — for discussion)*:
- Sign in (depends on UC-04) → view rejection reason → edit business/legal fields and documents →
  resubmit → Pending Approval. No resubmission cap, no rejection-history view, no diff-highlighting
  at MVP.
