# Stitch Prompt: Register Tour Operator Account

> Source documents:
> - Screen Specification: `ux/screen-specifications/register-tour-operator-account-screen-spec.md`
> - User Flow: `ux/user-flows/register-tour-operator-account-user-flow.md`
> - MVP Planning: `requirements/mvp/register-tour-operator-account-mvp.md`
>
> Scope note: unlike the Traveler flow's Login stub, both screens in the approved screen spec are
> real, in-scope MVP screens (not unconfirmed references), so this brief covers **both**: the
> Registration Form and the Pending Approval Confirmation screen.

---

# Product Context

TripMate is a travel companion product. This flow lets a Guest register their travel business as
a Tour Operator: submit identity, business/legal info, and a license file, then wait for
Administrator approval before they can publish tours or receive bookings. It is a longer, more
document-heavy form than the Traveler signup, aimed at a business user rather than a casual
first-time visitor.

# Target User

Guest — a travel business owner/representative registering their company. Assume they have the
required paperwork on hand (Tax Code, Travel Business License file) but are not necessarily
tech-savvy; mobile usage should still be supported, though this is a denser form than a typical
consumer signup.

---

## Screen 1: Tour Operator Registration Form

### Screen Objective

Let the Guest submit authentication info, business/legal info, and a scanned Travel Business
License file in one form, get clear inline/field-level feedback on any error, and on success be
handed off to the Pending Approval Confirmation screen. The same screen is reused, pre-filled,
when a Guest edits and resubmits a previously rejected application.

### Screen Content

- Screen heading (e.g., "Register as Tour Operator")
- Form fields, in order:
  1. Email address
  2. Password (masked)
  3. Company Name
  4. Tax Code
  5. Business contact details
  6. Travel Business License — file upload (PDF or image)
- Primary action: **Submit** button (Register on first submission; functionally the same action
  resubmits on the edit/resubmission path — relabeling it, e.g. "Resubmit Application", is a UX
  suggestion, not a confirmed requirement)

### Required Components

- Text input — Email address (email keyboard type)
- Password input — masked, with a show/hide toggle
- Text input — Company Name
- Text input — Tax Code
- Text input(s) — Business contact details
- File upload control — Travel Business License (accepts PDF or image; show selected filename or
  a thumbnail preview once attached)
- Primary button — Submit (full-width, single dominant action)
- Inline field-level error message component (beneath each field, including the file upload
  control)

### Required States

- **Initial / Input (empty)** — all fields empty, no errors, new registration
- **Input (pre-filled)** — resubmission path: previously submitted values loaded, including the
  previously uploaded file reference
- **Validation error** — one or more fields (including the file) show an inline error (e.g.,
  missing field, invalid format, unsupported file type/size)
- **Business error** — duplicate email, or duplicate business identity (Tax Code/License) —
  **note: the duplicate-business-identity error is a Should Have, not confirmed for MVP launch;
  design the error style so it's easy to add later, but do not treat it as guaranteed present**
- **Loading** — Submit button shows a loading/submitting indicator (design suggestion, not a
  confirmed requirement)
- **Success** — submission succeeds; screen transitions to the Pending Approval Confirmation
  screen

### Navigation Context

- Entry: Guest arrives here directly for new registration, or via an external rejection
  notification (channel not confirmed) for the pre-filled resubmission path
- On success → Pending Approval Confirmation screen (Screen 2)
- On any validation/business/system error → stays on this same screen with the relevant error
  shown

### UX Constraints

- Mobile-first, single-column layout — this form is longer than a typical signup, so group
  related fields (e.g., business/legal info together, separate from auth info) to reduce
  perceived complexity
- One clear primary action (Submit); no competing buttons
- Inline, field-level validation errors — not a single generic error banner
- File upload control must clearly show upload success/failure and the accepted file types
- Password field masked by default
- Consistent spacing and accessible form controls (adequate tap targets, labeled inputs, visible
  focus/error states)

### Open Questions

- Exact accepted file formats and max size for the license upload (design a generic "PDF or
  image" uploader without inventing a specific limit).
- Whether the duplicate-business-identity error (Tax Code/License) is actually present at launch.
- Exact set of fields inside "business contact details" — design a reasonable minimal set (e.g.,
  phone number) without inventing extra fields as firm requirements.

---

## Screen 2: Pending Approval Confirmation

### Screen Objective

Confirm to the Guest that their application was received and clearly communicate that the
account is now Pending Approval and restricted (cannot publish tours or receive bookings) until
an Administrator reviews it.

### Screen Content

- Confirmation message (e.g., "Your application has been submitted")
- Explanatory text that the account is Pending Approval and restricted until reviewed
- No further required content — where the Guest goes next is unresolved (see Open Questions)

### Required Components

- Confirmation heading/message (display only, no inputs)
- Supporting body text explaining the Pending Approval restriction

### Required States

- **Success** — the only confirmed state for this screen

### Navigation Context

- Entry: only reached immediately after a successful submission on Screen 1
- Exit: unresolved — no confirmed next destination (see Open Questions)

### UX Constraints

- Mobile-first, single-column, centered confirmation layout
- Minimal, no decorative elements — this is a status/confirmation message, not a marketing moment

### Open Questions

- Where the Guest navigates next from this screen (e.g., Login, Home) is not confirmed in source
  — design a self-contained confirmation screen; a single next-step button is a UX suggestion
  only (see UX Suggestions), not a confirmed requirement.

---

# UX Suggestions (apply to both screens, not confirmed requirements)

- Screen 1: show a short explanatory note near the Submit button that the account will enter
  Pending Approval and cannot publish/receive bookings until reviewed.
- Screen 2: include a single clear next-step action (e.g., a button back to Home or Login) so the
  Guest isn't left on a dead end.

---

# Stitch Prompt

Design two connected mobile-first screens for a travel app called TripMate: a Tour Operator
business registration form, and its post-submission confirmation screen.

## Screen 1 — Tour Operator Registration Form

**Layout:** Single-column, vertically stacked form on a mobile viewport. Clear heading at the top
(e.g., "Register as Tour Operator"). Group fields into two visual sections: "Account" (email,
password) and "Business Information" (Company Name, Tax Code, business contact details, license
file upload), to keep the longer form scannable.

**Fields, in this order:**
1. Email address — text input
2. Password — masked text input with a show/hide toggle
3. Company Name — text input
4. Tax Code — text input
5. Business contact details — text input
6. Travel Business License — file upload control (accepts PDF or image), showing the selected
   filename or a small preview once a file is attached

Each field has a clear label above it. Below any field with an error (including the file upload
control), show a small inline error message in a distinct error color.

**Primary action:** a single full-width "Submit" button below the form, styled as the clear
dominant call to action.

**States to show:**
- Default/empty state — all fields empty, no errors
- Pre-filled state — fields populated with example business data, representing the
  edit-and-resubmit case
- Error state — at least one field and the file upload control showing inline validation errors
- Loading state — Submit button showing a subtle in-progress indicator

**Style:** Clean, simple, minimal decoration. Clear visual grouping between account fields and
business fields. Generous spacing. Accessible contrast and tap target sizes. No marketing
content, illustrations, or unrelated UI beyond the form, heading, and section labels.

## Screen 2 — Pending Approval Confirmation

**Layout:** Single-column, centered confirmation layout on a mobile viewport.

**Content:** A confirmation heading (e.g., "Application Submitted") and supporting body text
explaining that the account is now Pending Approval and cannot publish tours or receive bookings
until an Administrator reviews it.

**Style:** Minimal, calm, status-message tone — not celebratory/marketing. No form fields. Keep
it visually consistent (typography, spacing, color palette) with Screen 1.
