# Product Context

TripMate provides a restricted partner area where a tour operator can view an application decision. A rejected applicant may review the rejection context, update company details and documents, and resubmit the same application for another review.

# Target Platform

Responsive desktop Web application. Design the connected Operator Application Status and Resubmit Application pages at 1440 px, 1280 px, and 768 px with no horizontal scrolling.

# Target User

- A tour-operator applicant checking a pending, rejected, or approved application.
- A rejected tour-operator applicant correcting and resubmitting the application.

# Screen Objective

Make application status and restrictions unambiguous, expose resubmission only for a rejected application, preserve relevant existing data, and return a successfully resubmitted application to Pending Review.

# Connected Screens

- Public Sign In.
- Operator Application Status.
- Resubmit Application.
- Approved operator workspace entry.

# Layout Structure

## Operator Application Status

1. Restricted partner header or shell.
2. Page title and application identity or submission metadata.
3. Prominent status summary and status-dependent guidance.
4. Rejection callout when applicable.
5. Status-dependent action region.

## Resubmit Application

1. Restricted partner shell and breadcrumb back to Application Status.
2. Rejection summary retained above the correction form.
3. Prefilled Company Information section.
4. Existing Documents and Replacement Documents section.
5. Primary Resubmit Application and secondary Cancel actions.
6. Inline validation, upload feedback, form-level error, and loading regions.

# Required Content

## Application Status

- Application ID.
- Account status and application status badges with text.
- Submitted or resubmitted timestamp where recorded.
- Review timestamp.
- Resubmission count where available.
- Pending Review guidance with no action to resubmit.
- Rejected guidance with visible rejection explanation and Resubmit Application action.
- Approved guidance with approved operator workspace entry.
- Sign Out.

## Resubmit Application

- Company Name.
- Business Licence Number.
- Tax Code.
- Business Address.
- Contact Person.
- Contact Phone Number.
- Existing business documents.
- Replace and add document actions.
- Document metadata: file name, type, size, status, and available action.
- Resubmit Application.
- Cancel.

# Required Components

- Restricted partner shell consistent with Tour Operator Registration.
- Status badge, metadata list, guidance panel, and rejection callout.
- Sign Out as a secondary status-screen action.
- Breadcrumb or clear return link.
- Persistently labeled prefilled fields.
- Existing-document list or table.
- File picker or drop zone for replacements and additional documents.
- Remove, replace, and add controls where applicable.
- Primary and secondary actions.
- Inline validation, upload status, loading state, and form-level alert.

# Required States

## Operator Application Status

- **Initial:** Restricted shell and empty status region are present before the latest application data is requested.
- **Loading:** Status area awaits the latest application data while the restricted shell remains stable.
- **Loaded / Success:** Pending Review shows account Pending Approval, `Your Tour Operator account is pending verification. You will be notified once approved.`, and no resubmit action; Rejected shows the rejection explanation and Resubmit Application; Approved shows approved status and operator-workspace entry.
- **Empty:** If no application exists, show a neutral safe fallback and navigation without inventing a locked message.
- **Business Error / Disabled Action:** Pending Review and Approved are ineligible for resubmission, so the action is absent; exact explanatory copy remains neutral.
- **System Error:** Use `TripMate is temporarily unable to process your request. Please check your connection and try again.`
- **Unauthorized:** Redirect unauthenticated users to Public Sign In and do not expose applicant data to a non-owner.
- **Responsive:** Status, rejection reason, metadata, and actions remain clear at every target width.

## Resubmit Application

- **Initial:** Rejected application context is established before editable data is ready.
- **Loading:** Existing application data and documents are being prepared.
- **Loaded:** Default prefilled form with existing documents.
- **Validation Error:** Required-field error uses `This field is required.` File and format error copy remains neutral.
- **Business Error:** An eligibility change or business-identity collision uses neutral placeholder copy; the application remains Rejected.
- **System Error:** `TripMate is temporarily unable to process your request. Please check your connection and try again.` Existing authoritative data remains visible.
- **Success:** Update the same application to Pending Review and return to Application Status; no separate success page.
- **Disabled Action:** Disable Resubmit while processing or after eligibility changes.
- **Unauthorized:** Unauthenticated or non-owning users cannot view or modify this application.
- **Responsive:** Form fields, rejection context, document metadata, and actions remain usable at every target width.
- Changed and unchanged field presentation without implying automatic validation.
- Existing document, replacement selected, uploading, uploaded, failed, removed, and added document treatments.
- Neutral placeholders for all unapproved field, document, status, and resubmission-success copy.

# Navigation Context

- Operator Application Status uses the proposed `/partner/application` destination.
- Resubmit Application uses the proposed `/partner/application/resubmit` destination.
- Cancel and successful resubmission return to the proposed `/partner/application` destination.
- Approved operator workspace entry may use the proposed `/operator` destination.
- If sign-in is required, the public entry is proposed `/sign-in`.
- Routes are design context, not implementation requirements.

# Responsive Behavior

- At 1440 px, use a controlled content width with an easy-to-scan status summary and a form that can use two columns where safe.
- At 1280 px, reduce gaps while preserving the rejection summary and document metadata hierarchy.
- At 768 px, use a single-column form, stack status metadata, and transform document rows into readable cards or stacked rows.
- Keep rejection copy, filenames, addresses, errors, and actions within the viewport without horizontal scrolling.

# Visual Direction

- Deep navy restricted-shell structure, teal interactive accents and pending/approved support, selective coral for rejection emphasis, and light-neutral surfaces.
- Professional partner operations tone with the same typography, form elements, status badges, and file controls as Tour Operator Registration.
- Restrained geographic detail is acceptable, but this is an application-management experience rather than a tourism marketing page.
- Status meaning always appears in text as well as color.

# Accessibility

- Status and rejection reason are announced through text, not color or icon alone.
- Persistent labels, associated errors, visible focus, logical keyboard order, and comfortable targets.
- Document actions use clear text labels and keyboard-operable controls.
- Long document names have an accessible full-name treatment when visually truncated.
- Loading and success transitions preserve context and are perceivable.

# UX Constraints

- Resubmit Application is available only when the application status is Rejected.
- Pending Review has no resubmit action.
- Successful resubmission updates the same application to Pending Review and returns to status; do not create a separate success page.
- Preserve prefilled company information and existing document context.
- Do not invent rejection reasons, document requirements, file formats, file-size limits, upload-count limits, field-edit policy, or validation rules.
- Use exact approved messages only in matching contexts and neutral placeholders for unresolved copy.

# Things NOT to Design

- No administrator review or approval interface.
- No new operator registration flow.
- No resubmit action for Pending Review or Approved status.
- No separate resubmission-success page.
- No invented document checklist, upload constraints, rejection reason, or review timeline.
- No full operator dashboard, tour management, payments, or inventory features.
- No mobile application screen.
- No API, database, authentication, authorization, or storage implementation.

# Open Questions

- Exact rejection reason content comes from the application decision and is not predefined.
- Exact editable-field policy and document requirements for resubmission are unresolved.
- File formats, file-size limits, upload counts, and detailed document validation messages are not approved.
- Exact resubmission-success and status-transition copy is not approved.
- Whether applicants see only the latest rejection or full rejection history is unresolved.
- Reviewer ID visibility, notification-channel entry points, and any cap on resubmission attempts are unresolved.
- Whether every stated rejection issue must be resolved or only current validation must pass is unresolved.
- Final production routes and approved workspace behavior remain implementation decisions.

# Stitch Prompt

Design a responsive desktop Web application, not a mobile app.

Create two connected TripMate partner screens: Operator Application Status and Resubmit Application. Produce responsive layouts at 1440 px, 1280 px, and 768 px with no horizontal scrolling.

Use the same partner design system as Tour Operator Registration: deep navy structure, teal interactive accents and positive-status support, selective coral for rejection emphasis, light-neutral surfaces, generous whitespace, accessible typography, consistent status badges, and consistent document controls. Keep the tone professional and operational. Restrained Central Vietnam geographic detail and carefully placed professional tourism photography may identify TripMate, but they must not compete with application status or turn this into a tourism marketing page.

Operator Application Status screen:

- Use a restricted partner shell with page title, application identity or submission metadata, and a prominent status summary.
- Show Application ID, Account Status, Application Status, submitted or resubmitted time where recorded, review time, and resubmission count where available. Include Sign Out as a secondary action.
- Design three state variants. Pending Review shows guidance and no resubmit action. Rejected shows a clearly labeled rejection callout and a Resubmit Application action. Approved shows approved guidance and an action to enter the approved operator workspace.
- In the Pending Review / account Pending Approval variant, use the exact restriction message `Your Tour Operator account is pending verification. You will be notified once approved.`
- Include initial/loading, loaded, no-application safe fallback, generic system-error, and unauthorized treatments. A user who does not own the application must not see applicant data.
- Use text status labels in addition to color.
- The rejection reason is data supplied by the decision; use a neutral content placeholder and do not invent a reason.

Resubmit Application screen:

- Reuse the restricted shell and include a breadcrumb or clear link back to Application Status.
- Keep the rejection summary visible above the correction form so the applicant retains context.
- Prefill Company Name, Business Licence Number, Tax Code, Business Address, Contact Person, and Contact Phone Number.
- Show existing documents with file name, type, size, status, and available action.
- Allow Replace and Add document actions. Include visual treatments for existing, replacement selected, uploading, uploaded, failed, removed, and newly added documents.
- Do not specify document types beyond existing approved business-document context, and do not state file extensions, file-size limits, or upload-count limits.
- Include a primary Resubmit Application button and secondary Cancel action.
- Include initial/loading, loaded, validation-error, business-error, system-error, disabled-submit, unauthorized, and successful-return treatments. Eligibility change and business-identity collision copy remains neutral because no matching locked message is approved.

Show required-field validation with the exact message `This field is required.` Show temporary failure with the exact message `TripMate is temporarily unable to process your request. Please check your connection and try again.` For every other field, document, status, and resubmission-success message, use a neutral placeholder such as “Message to be confirmed” rather than inventing requirements.

Show a loading state that prevents duplicate resubmission. Successful resubmission must update the same application to Pending Review and return to the Application Status screen; do not create a separate success page.

Represent proposed navigation relationships without exposing technical paths as user-facing copy: Application Status `/partner/application`, Resubmit Application `/partner/application/resubmit`, approved workspace `/operator`, and public sign-in `/sign-in`.

At 1440 px use a controlled content width, easy-to-scan status summary, and safe two-column form groupings. At 1280 px tighten gaps while preserving the rejection and document hierarchy. At 768 px switch to one form column and present document rows as stacked accessible cards or rows. Ensure persistent labels, associated errors, strong contrast, visible keyboard focus, logical tab order, comfortable targets, text-based document actions, accessible filename truncation, and no clipped content or horizontal scrolling.
