# Product Context

TripMate lets a prospective tour operator create an account and submit a business profile for administrator review. Submission does not grant immediate operator access; the account and application remain restricted until approval.

# Target Platform

Responsive desktop Web application. Design Tour Operator Registration, its in-page Application Submitted state, and the connected Operator Application Status screen at 1440 px, 1280 px, and 768 px with no horizontal scrolling.

# Target User

- A representative registering a tour-operator business with TripMate.
- A newly submitted operator reviewing the application's current status.

# Screen Objective

Collect approved account and company information, support required document submission, communicate review expectations, and give the applicant a clear status destination without implying approval or automatic sign-in.

# Connected Screens

- TripMate Landing Page.
- Public Sign In.
- Application Submitted state within Tour Operator Registration.
- Operator Application Status.
- Resubmit Application for a rejected application.
- Approved operator workspace entry after approval.

# Layout Structure

## Tour Operator Registration

1. Public partner-registration header and concise application guidance.
2. Account Information section.
3. Company Information section.
4. Business document upload section with uploaded-file list.
5. Required Terms, Privacy Policy, and Partner Agreement consent.
6. Registration actions and form feedback.
7. Application Submitted replaces or follows the form in the same page context.

## Application Submitted State

1. Clear submission confirmation and pending status badges.
2. Review-time expectation and access restriction explanation.
3. Proposed Go to Sign In action, followed after authentication by application-status continuation.

## Connected Operator Application Status

1. Restricted partner shell.
2. Application summary and status badge.
3. Status-dependent guidance for Pending Review, Rejected, and Approved.
4. Resubmit action only when Rejected.

# Required Content

## Account Information

- Email Address.
- Password.
- Confirm Password.

## Company Information

- Company Name.
- Business Licence Number.
- Tax Code.
- Business Address.
- Contact Person.
- Contact Phone Number.

## Documents and Consent

- Business Licence upload.
- Supporting documents required by approved policy, without inventing the exact document list.
- Uploaded-file metadata: file name, type, size, upload status, and remove or replace action.
- Required acceptance of Terms, Privacy Policy, and Partner Agreement.

## Submission and Status

- Register or Submit Application action.
- Back to Sign In.
- Account status: Pending Approval after submission.
- Application status: Pending Review after submission.
- Pending Review, Rejected, and Approved status presentations on the connected status screen.

# Required Components

- Partner registration shell consistent with the public TripMate product.
- Section headings and progress-friendly grouping without inventing a multi-step workflow.
- Persistently labeled account and company fields.
- Accessible password visibility controls and password guidance.
- File drop zone or file picker.
- Uploaded-document list or table with metadata and remove/replace controls.
- Required agreement checkbox and policy links.
- Primary submit action, secondary navigation, loading treatment, inline validation, and form-level alert.
- Status badges, confirmation panel, status summary, and status-dependent actions.

# Required States

## Registration

- **Initial:** Registration sections are empty, with no uploads and consent unchecked.
- **Loading:** Files or the full application are being submitted or validated; preserve completed non-sensitive fields, prevent repeat submission, and show per-file progress where supported.
- **Loaded:** Form and upload controls are ready.
- **Validation Error:** Required field uses `This field is required.` Invalid email uses `Invalid email format. Please enter a valid email address (e.g., user@example.com).` Weak password uses `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.` Password mismatch uses `Passwords do not match. Please re-enter.`
- **Business Error:** Existing email uses `An account with this email already exists. Please sign in or use another email.` Duplicate business identity or an existing pending application uses neutral placeholder copy.
- **System Error:** `TripMate is temporarily unable to process your request. Please check your connection and try again.` Do not imply partial account or application creation.
- **Success:** Transition to the Application Submitted state described below.
- **Disabled Action:** Disable the primary submission action only while submission is active.
- **Responsive:** All sections, upload metadata, consent text, errors, and success content remain usable at every target width.
- Upload in progress, uploaded, failed, remove, and replace treatments without invented file constraints.
- Neutral placeholders for missing document, unsupported file, duplicate business, pending application, and agreement-validation messages where exact approved copy is unavailable.

## Application Submitted

- Confirmation: `Your business profile has been submitted for verification. Admin review takes 1-2 business days.`
- Account Pending Approval badge.
- Application Pending Review badge.
- Clear restricted-access explanation.

## Connected Application Status

- Pending Review with account Pending Approval, no resubmit action, and `Your Tour Operator account is pending verification. You will be notified once approved.`
- Rejected with a visible rejection explanation area and Resubmit Application action.
- Approved with approved operator workspace entry.

# Navigation Context

- Tour Operator Registration uses the proposed `/partner/register` destination.
- Application Submitted is a state within that registration page, not a separate route.
- Go to Sign In leads to the proposed `/sign-in` destination.
- After authentication, a Pending Approval or Rejected operator proceeds to the proposed `/partner/application` destination.
- Rejected status may lead to proposed `/partner/application/resubmit`.
- Approved status may lead to a proposed operator destination such as `/operator`.
- Routes are design context, not implementation requirements.

# Responsive Behavior

- At 1440 px, use a wide but controlled registration container. Account and company fields may use a two-column grid where labels and errors remain clear; the document region has enough width for metadata.
- At 1280 px, reduce gaps and preserve field grouping without crowding.
- At 768 px, use a single-column form, stack document metadata and actions, keep status summaries readable, and avoid horizontal scrolling.
- Long filenames, error text, agreements, and business addresses wrap or truncate accessibly without breaking layout.

# Visual Direction

- Deep navy structure, teal interactive and status accents, selective coral emphasis, and light-neutral surfaces.
- Professional and trustworthy partner onboarding with restrained Central Vietnam geographic or tourism detail.
- More businesslike than traveler registration while remaining part of the same public TripMate design system.
- Status colors supplement labels and icons; color never carries meaning alone.

# Accessibility

- Persistent labels, explicit required indicators, associated errors, and keyboard-accessible document controls.
- File status and actions are readable by text, not icons or color alone.
- Agreement links and checkbox have a coherent accessible name.
- Strong contrast, visible focus, logical tab order, and comfortable targets.
- Status badges include status text; loading and submission confirmation are perceivable.

# UX Constraints

- Do not invent exact supporting-document requirements, file extensions, file-size limits, or upload-count limits.
- Do not imply immediate approval, automatic sign-in, or operator workspace access after submission.
- Application Submitted is a state of the registration page, not a standalone page.
- Only rejected applications receive a Resubmit action on the connected status screen.
- Use exact approved messages only in matching contexts; unresolved validation and policy copy must remain neutral placeholders.

# Things NOT to Design

- No separate Application Submitted page or route.
- No administrator review interface.
- No operator dashboard content beyond a labeled approved-workspace entry action.
- No fixed upload formats, sizes, document checklist, or number of supporting files.
- No payment, subscription, tour creation, or inventory-management workflow.
- No mobile application screen.
- No API, database, authentication, or storage implementation details.

# Open Questions

- The exact mandatory supporting-document list is not approved.
- Allowed file formats, size limits, and upload-count limits are unresolved.
- Exact format rules for Business Licence Number and Tax Code are unresolved.
- Exact messages for missing documents, unsupported files, duplicate businesses, existing pending applications, and agreement validation are not approved.
- Whether Go to Sign In or passive confirmation is the final approved submitted-state action remains unresolved; Go to Sign In is represented only as the current UX proposal.
- Final production routes and approved operator workspace behavior remain implementation decisions.

# Stitch Prompt

Design a responsive desktop Web application, not a mobile app.

Create a connected TripMate partner-onboarding experience containing Tour Operator Registration, an Application Submitted state inside the registration page, and an Operator Application Status screen. Produce responsive Web layouts at 1440 px, 1280 px, and 768 px with no horizontal scrolling.

Use a professional shared TripMate visual system: deep navy structure, teal interactions and positive status accents, limited coral for important emphasis or errors, light-neutral content surfaces, generous whitespace, accessible typography, restrained Central Vietnam geographic detail, and carefully placed professional tourism photography that never competes with the long form. This experience should feel businesslike and trustworthy while remaining visibly part of the public TripMate product.

Tour Operator Registration:

- Start with concise partner-application guidance.
- Group the form into Account Information, Company Information, Business Documents, and Agreements.
- Account fields: Email Address, Password, Confirm Password.
- Company fields: Company Name, Business Licence Number, Tax Code, Business Address, Contact Person, Contact Phone Number.
- Include accessible password visibility controls and visible password guidance.
- Provide a file drop zone or picker for a Business Licence and supporting documents required by approved policy. Do not name an unapproved supporting-document checklist and do not state file extensions, file-size limits, or upload-count limits.
- Show uploaded files with file name, type, size, upload status, and Remove or Replace actions. Include upload-in-progress, uploaded, failed, remove, and replace visual states.
- Include one required agreement accepting Terms, Privacy Policy, and Partner Agreement, with each policy available as a link.
- Provide a primary Register or Submit Application action and Back to Sign In.

Application Submitted state in the same page:

- Replace or follow the submitted form with a clear confirmation panel; do not create a separate submitted page.
- Show Account: Pending Approval and Application: Pending Review with text status badges.
- Use the exact message `Your business profile has been submitted for verification. Admin review takes 1-2 business days.`
- Explain that operator access remains restricted until approval.
- Include the proposed Go to Sign In action. After authentication, connect a Pending Approval or Rejected operator to Application Status. Do not add a direct unauthenticated status action or imply automatic sign-in.

Connected Operator Application Status screen:

- Use a restricted partner shell with application summary and status metadata.
- Show three coherent variants: Pending Review with no resubmit action; Rejected with a visible rejection-reason area and a Resubmit Application action; Approved with an action leading to the approved operator workspace.
- Do not design the resubmission form here; show only the connected action for the rejected state.

Show initial, loaded, validation-error, business-error, system-error, loading, disabled-submit, and success treatments. During upload or submission, preserve completed non-sensitive fields, prevent repeat submission, and show per-file progress where supported. Use these exact approved messages only in their matching registration states:

- `This field is required.`
- `Invalid email format. Please enter a valid email address (e.g., user@example.com).`
- `An account with this email already exists. Please sign in or use another email.`
- `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.`
- `Passwords do not match. Please re-enter.`
- `Your Tour Operator account is pending verification. You will be notified once approved.` Use this only for the connected Pending Approval restriction context.
- `TripMate is temporarily unable to process your request. Please check your connection and try again.`

For missing-document, unsupported-file, duplicate-business, existing-pending-application, and agreement-validation messages, use a neutral placeholder such as “Message to be confirmed” rather than inventing rules.

Represent proposed navigation relationships without exposing technical paths as user-facing copy: registration `/partner/register`, sign-in `/sign-in`, application status `/partner/application`, resubmission `/partner/application/resubmit`, and approved workspace `/operator`.

At 1440 px use a wide controlled container with a clear section hierarchy and optional two-column field grids. At 1280 px tighten gaps while preserving readable labels and errors. At 768 px use a single column and stack document metadata and actions. Long filenames, addresses, errors, and agreements must wrap or truncate accessibly. Include persistent labels, explicit required markers, associated error text, keyboard-accessible uploads, visible focus, strong contrast, textual status labels, logical tab order, and comfortable targets.
