# Web Screen Inventory

| # | Screen | Related UC | Actor | Route Suggestion |
|---|---|---|---|---|
| 1 | Tour Operator Registration | UC-02 | Guest representing a travel business | `/partner/register` |

`Application Submitted` is the success state of this page, not a standalone Web page. After authentication, the durable status destination is the Operator Application Status page specified under UC-02/UC-03.

# Screen: Tour Operator Registration

## Related UC

UC-02 Register Tour Operator Account.

UC-02 ends when the account and initial application are created in Pending Approval/Pending Review and submission confirmation is shown. Rejected-application correction and resubmission belong exclusively to UC-03 and are not embedded in this form.

## Actor

Guest representing a travel business.

## Platform

Responsive Next.js Web.

## Purpose

Collect the required credentials, company/legal information, business documents, and agreements needed to create one Tour Operator account and one initial application for Administrator review.

## Entry Conditions

- Guest is unauthenticated.
- Guest selects Register as Tour Operator on the Web application.
- Required business information and documents are available for submission.

## Entry Points

- TripMate Landing Page.
- Public authentication navigation.

## Exit / Navigation

- Successful submission → Application Submitted state on the same page.
- From the success state → Public Sign In; after authentication, a Pending Approval operator is directed to Operator Application Status.
- Back to Sign In → Public Sign In.
- Validation/business/system error → remain on Tour Operator Registration.

## Suggested Route

`/partner/register`

`/partner/application/submitted` is unnecessary because confirmation is a state. `/partner/application` remains the proposed durable status destination after authentication.

## Main Layout Regions

1. Public TripMate header with authentication handoff.
2. Partner-onboarding context and approval restriction summary.
3. Account information section.
4. Company and business-contact section.
5. Business-document upload section.
6. Terms, Privacy Policy, and Partner Agreement consent area.
7. Primary action and validation/status region.
8. Application Submitted success state replacing or summarizing the completed form after a valid submission.

At 1440px and 1280px, related fields may use a two-column form grid. This remains one business submission flow.

## Required Data

- Email Address.
- Password.
- Confirm Password.
- Company Name.
- Business Licence Number.
- Tax Code.
- Business Address.
- Contact Person.
- Contact Phone Number.
- Business Licence document.
- Supporting documents required by the approved document policy.
- Acceptance of Terms of Service, Privacy Policy, and Partner Agreement.

System-created User ID, role, account/application statuses, Application ID, storage references, and timestamps are not editable fields.

## Components

### Inputs

- Email Address.
- Password.
- Confirm Password.
- Company Name.
- Business Licence Number.
- Tax Code.
- Business Address.
- Contact Person.
- Contact Phone Number.
- Business Licence file upload.
- Supporting-document file upload.
- Required consent checkbox for Terms of Service, Privacy Policy, and Partner Agreement.

### Read-only Information

- Approval-process explanation.
- Restriction notice: Tour Operator Workspace functions remain locked until Administrator approval.
- Selected-file metadata and validation status.
- In Application Submitted state: account status Pending Approval, application status Pending Review, and MSG08.

### Primary Actions

- Submit Application in Initial/Input/Error states.
- Go to Sign In in Application Submitted state is an architecture/UX proposal that enables the approved later status handoff; UC-02 does not establish automatic sign-in.

### Secondary Actions

- Back to Sign In.
- Remove or replace a selected upload before final submission.
- Return to Landing Page from the success state is an optional UX suggestion.

## Table / List Columns

If multiple supporting documents are selected, use a compact upload list with:

- File name.
- File type.
- File size.
- Upload/validation status.
- Remove/replace action.

No document-verification result is produced at registration; content review belongs to Administrator moderation.

## Filters / Search

Not applicable.

## Modal / Dialog Behavior

- Policy/Partner Agreement links may open approved pages or dialogs; the destination is not specified.
- No pre-submit confirmation dialog is required.
- Application Submitted is an inline/page success state, not a modal.

## Validation

- Account, company, contact, mandatory document, and consent inputs explicitly listed by Report 3 are required.
- Email Address format must be valid and unique.
- Password must satisfy the password policy; Confirm Password must match.
- Business Licence Number and Tax Code must each be unique across Tour Operator accounts.
- Every uploaded file must satisfy the approved type and size rules. Report 3 does not state the exact formats or maximum size.
- A duplicate application must not be created where an application is already Pending Review for the submitted identity.
- Persistence/storage failure must not present a partial account or application as successful.

## Business Rules

- **BR-01:** An email address may be registered to at most one TripMate account.
- **BR-02:** Password must contain at least 8 characters and include at least one uppercase letter, one digit, and one special character. Locked MSG05 additionally states lowercase; the UI uses the locked policy wording without silently changing BR-02.
- **BR-03:** Password is stored in hashed form.
- **BR-07:** Mandatory business documents are required and the account remains locked from operator functions until Administrator approval.
- **BR-08:** Business Licence Number and Tax Code may each belong to at most one Tour Operator account.

## Application Messages

Use the exact locked content where an existing message matches:

| Code | Content | Web Usage |
|---|---|---|
| MSG01 | `This field is required.` | Missing required field, file, or consent. |
| MSG02 | `Invalid email format. Please enter a valid email address (e.g., user@example.com).` | Invalid Email Address. |
| MSG03 | `An account with this email already exists. Please sign in or use another email.` | Duplicate Email Address. |
| MSG05 | `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.` | Password-policy failure. |
| MSG06 | `Passwords do not match. Please re-enter.` | Confirm Password mismatch. |
| MSG08 | `Your business profile has been submitted for verification. Admin review takes 1-2 business days.` | Application Submitted state. |
| MSG127 | `TripMate is temporarily unable to process your request. Please check your connection and try again.` | Storage, persistence, or generic system failure. |

Detailed UC-02 references unrelated locked codes for missing/invalid documents, duplicate business identity, an already-pending application, and success. These are recorded in `docs/batch-1-message-reconciliation.md`. Required states remain visible, but missing message copy uses neutral placeholder wording marked as a UX suggestion until the message catalogue is corrected.

## States

### Initial

Empty registration sections; no uploads or consent selected.

### Loading

Files or the full application are being submitted/validated. Preserve completed non-sensitive fields and show per-file progress where supported.

### Loaded

Form and upload controls are ready.

### Validation Error

MSG01, MSG02, MSG05, or MSG06 where applicable. Missing/invalid document copy uses a neutral placeholder UX suggestion because no valid locked message exists.

### Business Error

Duplicate Email Address uses MSG03. Duplicate business identity or existing Pending Review application uses a neutral placeholder UX suggestion pending locked copy.

### System Error

MSG127; neither a partial account nor application is presented as successfully created.

### Success

Application Submitted is a state of this page:

- Display MSG08.
- Show account status Pending Approval.
- Show application status Pending Review.
- Explain that operator functions unlock only after approval.
- Offer the proposed Sign In handoff; do not imply automatic authentication.

### Confirmation

No pre-submit confirmation is required. The Success state provides post-submit confirmation.

### Disabled

Submit Application is disabled during active submission. No other pre-submit disablement rule is required.

### Unauthorized

An authenticated user reaching this Guest registration route must not silently upgrade or modify the existing account. Exact redirect behavior is not established and does not change the unauthenticated Stitch state.

### Responsive

All sections, upload metadata, consent text, errors, and the success state remain accessible without horizontal scrolling.

## Navigation Map

```text
Landing / Public Sign In
└── Tour Operator Registration
    ├── Success → Application Submitted state
    │   └── Sign In (proposed) → Operator Application Status
    ├── Back to Sign In → Public Sign In
    └── Error → Tour Operator Registration
```

## Responsive Behavior

### 1440px

- Use a business-onboarding layout with a persistent approval note and constrained form.
- Pair naturally related fields; keep document uploads full-width.
- Render Application Submitted as a focused status panel in the same partner/public shell.

### 1280px

- Retain section grouping; reduce grid columns when labels or validation copy become cramped.

### 768px

- Stack all inputs.
- Render file metadata as cards/list rows instead of a wide table.
- Stack success statuses and next-step action without adopting Mobile-app chrome.

## Open Questions

- Which supporting documents are mandatory in addition to the Business Licence?
- What file types and maximum size are approved?
- What precise format rules apply to Business Licence Number and Tax Code?
- What locked messages should cover missing/invalid documents, duplicate business identity, and an already-pending application?
- Is `Go to Sign In` the approved success-state action, or should only a passive status confirmation be shown?

These questions do not change the required field groups, success-state existence, validation placement, or page decomposition. Neutral helper/error placeholders may be used in Stitch.

## UX Suggestions

- Use a business operations onboarding tone focused on trust and verification.
- Show the approval restriction before submission and in the success state.
- Keep file metadata and replace/remove actions explicit.
- Use a neutral placeholder such as “Please review the highlighted document requirement.” where locked document-error copy is missing; this is not a product requirement.
