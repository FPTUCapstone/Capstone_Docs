# Web Screen Inventory

| # | Screen | Related UC | Actor | Route Suggestion |
|---|---|---|---|---|
| 1 | Operator Application Status | UC-02, UC-03 | Authenticated Tour Operator applicant | `/partner/application` |
| 2 | Resubmit Application | UC-03 | Tour Operator with latest application in Rejected status | `/partner/application/resubmit` |

The Resubmit Application form reuses the company-information and document-upload patterns from `register-tour-operator-account-web-screen-spec.md`. Authentication fields are not repeated because UC-03 uses the existing account.

# Screen: Operator Application Status

## Related UC

- UC-02 Register Tour Operator Account.
- UC-03 Resubmit Tour Operator Application.

## Actor

Authenticated Tour Operator applicant.

## Platform

Responsive Next.js Web.

## Purpose

Show the latest Tour Operator application state and the information needed to understand the next allowed step. When the latest application is Rejected, display the recorded rejection reason and offer the UC-03 resubmission action.

## Entry Conditions

- Tour Operator has authenticated through UC-04.
- An application exists for the account.

## Entry Points

- Post-sign-in role/status routing for Pending Approval or Rejected applicants.
- Post-authentication continuation after the UC-02 Application Submitted state.
- Tour Operator workspace restriction handoff.
- Application outcome notification link where an approved channel exists.

## Exit / Navigation

- Rejected + Resubmit Application → Resubmit Application screen.
- Pending Review → remain on status screen; no resubmission action.
- Approved → Tour Operator workspace entry.
- Sign Out → UC-05 action.

## Suggested Route

`/partner/application`

## Main Layout Regions

1. Restricted Tour Operator workspace header.
2. Application status summary.
3. Review information area.
4. Rejection-reason callout when status is Rejected.
5. Contextual action area.

Do not expose the full operational Tour Operator workspace navigation while the account remains restricted.

## Required Data

- Application ID.
- Latest application status.
- Account status.
- Submitted/resubmitted timestamp where recorded.
- Review timestamp.
- Rejection reason when Rejected.
- Resubmission count where available.

## Components

### Inputs

None on the status screen.

### Read-only Information

- Account status.
- Application status.
- Review/submission timestamps.
- Rejection reason.
- Restriction explanation.

### Primary Actions

- Resubmit Application, shown only when the latest application status is Rejected (BR-09).
- Enter Tour Operator Workspace when account/application approval permits it.

### Secondary Actions

- Sign Out.

## Table / List Columns

Not applicable for the latest-status view. A full rejection history is not required by the canonical UC.

## Filters / Search

Not applicable.

## Modal / Dialog Behavior

None required.

## Validation

- Application must exist for the current account.
- Resubmit Application is offered only when the latest application status is Rejected.
- Pending Review and approved applications cannot be resubmitted.

## Business Rules

- **BR-07:** Mandatory business documents are required and operator functions remain locked until approval.
- **BR-09:** Only the latest application in Rejected status may be resubmitted; Pending or approved application cannot be resubmitted.

## Application Messages

- MSG10: `Your Tour Operator account is pending verification. You will be notified once approved.` may be used for the Pending Approval restriction context.
- MSG127: `TripMate is temporarily unable to process your request. Please check your connection and try again.` applies to generic loading/system failure.

UC-03 references MSG25 when no eligible rejected application exists. The locked MSG25 content is `POI added to the master catalog successfully.`, so it must not be shown here. No matching locked status/ineligible-resubmission copy currently exists.

See `docs/batch-1-message-reconciliation.md`. Use neutral placeholder status copy only as a UX suggestion where no valid locked message exists.

## States

### Initial

Restricted partner shell with status area awaiting data.

### Loading

Latest application and review information are being retrieved.

### Loaded

Current status and permitted next action are visible.

### Empty

No application exists for the authenticated account. No locked message exists; offer only safe navigation while the data/state conflict is investigated.

### Business Error

Ineligible resubmission because status is Pending Review or Approved. The action is not offered; exact locked copy is unresolved.

### System Error

MSG127.

### Success

- Pending Review: status is visible; no workspace access.
- Approved: workspace entry becomes available.
- Rejected: rejection reason and Resubmit Application action are visible.

### Disabled

Resubmit action is absent or disabled unless status is Rejected. Prefer absence with a clear status explanation when the action is not applicable.

### Unauthorized

- Unauthenticated user → Public Sign In.
- Authenticated non-operator → role-appropriate entry screen.

### Responsive

Status, rejection reason, and action hierarchy remain clear at all widths.

## Navigation Map

```text
Public Sign In
└── Operator Application Status
    ├── Pending Review → Status only
    ├── Rejected → Resubmit Application
    ├── Approved → Tour Operator Workspace
    └── No application / error → Safe status fallback
```

## Responsive Behavior

### 1440px

- Use a restricted partner workspace shell with a central status/detail panel.
- Show status metadata and the rejection callout side by side only when readability is preserved.

### 1280px

- Keep status metadata compact and the primary next action prominent.

### 768px

- Stack status metadata, rejection reason, and action.
- Do not expose a full desktop sidebar if all workspace modules are locked.

## Open Questions

- What exact locked message replaces the invalid MSG25 reference for no eligible application?
- Is prior rejection history visible, or only the latest rejection reason required by UC-03?
- Which notification channels link back to this screen?
- What exact Tour Operator workspace route becomes available after approval?

## UX Suggestions

- Use distinct, accessible badges for Pending Review, Rejected, and Approved.
- Keep the rejection reason prominent but non-punitive and place Resubmit Application directly after it.

# Screen: Resubmit Application

## Related UC

UC-03 Resubmit Tour Operator Application.

## Actor

Authenticated Tour Operator whose latest application status is Rejected.

## Platform

Responsive Next.js Web.

## Purpose

Show the latest rejection context, load the existing company/application data, allow supported corrections and document replacement/addition, and return the same application to Pending Review without creating a duplicate account or application.

## Entry Conditions

- User is authenticated as the owning Tour Operator.
- Latest application exists and is Rejected.

## Entry Points

- Resubmit Application action on Operator Application Status.

## Exit / Navigation

- Valid resubmission → Operator Application Status in Pending Review state.
- Cancel → Operator Application Status without applying changes.
- Validation/business/system error → remain on Resubmit Application; application stays Rejected.

## Suggested Route

`/partner/application/resubmit`

## Main Layout Regions

1. Restricted partner workspace header and breadcrumb.
2. Read-only rejection summary.
3. Editable company-information section.
4. Existing and new/replacement document area.
5. Validation/status region.
6. Resubmit and Cancel action bar.

## Required Data

### Read-only application context

- Application ID.
- Current status.
- Rejection reason.
- Reviewer ID where the approved UI is permitted to expose it.
- Review timestamp.
- Resubmission count.

### Correctable input

- Company Name.
- Business Licence Number.
- Tax Code.
- Business Address.
- Contact Person.
- Contact Phone Number.
- Existing business documents and new replacement/additional files.

## Components

### Inputs

- Pre-filled company/contact fields listed above.
- File controls to replace or add business documents.

Authentication Email Address and Password are not editable in this flow.

### Read-only Information

- Current Rejected status.
- Recorded rejection reason.
- Review timestamp.
- Existing document metadata/version references appropriate for the applicant.

### Primary Actions

- Resubmit Application.

### Secondary Actions

- Cancel.
- Replace/remove a newly selected file before submission.

## Table / List Columns

For existing and selected documents:

- Document/file name.
- Document type.
- Current/new version indicator.
- File size where available.
- Validation/upload status.
- Replace/remove action where permitted.

## Filters / Search

Not applicable.

## Modal / Dialog Behavior

- Cancel with unsaved changes may use a confirmation dialog, but no approved operator-specific locked message exists. Treat it as a UX suggestion until message/content is approved.
- Successful resubmission does not require a separate success page; return to Application Status with updated state.

## Validation

- Latest application status must still be Rejected at submit time.
- Required company fields and mandatory documents must remain present.
- Uploaded files must satisfy approved type and size rules.
- Corrected Business Licence Number and Tax Code must not belong to another account; the current application must be excluded from its own uniqueness check.
- Persisted update must retain the previous rejection/review record for audit.

## Business Rules

- **BR-07:** Mandatory documents remain required; account remains locked until Administrator approval.
- **BR-08:** Business Licence Number and Tax Code are unique per Tour Operator account.
- **BR-09:** Only the latest Rejected application may be resubmitted.
- Successful resubmission updates the existing application to Pending Review, records timestamp/count, keeps the account Pending Approval, and retains the prior rejection record.

## Application Messages

Matching locked messages:

- MSG01: `This field is required.`
- MSG127: `TripMate is temporarily unable to process your request. Please check your connection and try again.`

The UC references MSG22 for missing documents, MSG19 for invalid uploads, MSG26 for Licence/Tax Code collision, MSG25 for ineligible status, and MSG24 for successful resubmission. The locked list assigns all five codes to unrelated preference/profile/POI events. These message references must be corrected before production copy is finalized; do not display the unrelated locked content.

The visible validation and success behaviors are nevertheless determined. Stitch may represent those states with neutral placeholder copy explicitly treated as a UX suggestion; see `docs/batch-1-message-reconciliation.md`.

## States

### Initial

Rejection context and existing values are being prepared.

### Loading

Existing application/documents load or corrected files upload.

### Loaded

Rejected application is shown with pre-filled editable values.

### Validation Error

MSG01 for missing input. File and format error copy remains unresolved because of the catalogue conflict.

### Business Error

- Latest application status changed and is no longer eligible.
- Business Licence Number or Tax Code belongs to another account.

Exact locked copy is unresolved.

### System Error

MSG127; prior Rejected status and existing data remain authoritative.

### Success

Same application becomes Pending Review, timestamp/count is recorded, prior rejection is retained, and navigation returns to Operator Application Status. Exact locked success copy is unresolved.

### Confirmation

Optional unsaved-change confirmation on Cancel; not a canonical business requirement.

### Disabled

Resubmit is disabled while a submission is processing or after eligibility changes.

### Unauthorized

Unauthenticated or non-owning users cannot view or modify the application.

### Responsive

Long business forms, rejection context, and file metadata remain usable at all supported widths.

## Navigation Map

```text
Operator Application Status (Rejected)
└── Resubmit Application
    ├── Success → Operator Application Status (Pending Review)
    ├── Cancel → Operator Application Status (Rejected)
    └── Error → Resubmit Application (Rejected remains)
```

## Responsive Behavior

### 1440px

- Use a workspace form with a visible breadcrumb/status context.
- Keep rejection information in a prominent top callout or side context panel.
- Pair related fields in two columns; keep documents full-width.

### 1280px

- Reduce the number of form columns before labels/errors wrap excessively.

### 768px

- Stack all fields and document entries.
- Place Resubmit and Cancel after the complete form; sticky actions are optional only if they do not hide validation.

## Open Questions

- What exact locked messages replace MSG19/MSG22/MSG24/MSG25/MSG26 for this flow?
- Which file types and size limits apply to replacement/supporting documents?
- Is full rejection history visible or only the latest record?
- Is Reviewer ID visible to the applicant or administrative only?
- Is there a cap on resubmission attempts?
- Must every stated rejection issue be resolved, or is passing the current validation sufficient?

## UX Suggestions

- Reuse the same field groups and file controls as initial registration so applicants can focus on corrections.
- Visually distinguish existing files from new replacements and make the version that will be submitted unambiguous.
- Keep the recorded rejection reason visible while editing, especially on wide screens.
