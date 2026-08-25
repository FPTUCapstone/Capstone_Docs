# Screen Inventory

1. Tour Operator Registration Form — handles both a brand-new registration and an
   edit-and-resubmit after Administrator rejection (same screen, different pre-fill state), per
   the MVP rule to reuse screens where possible.
2. Pending Approval Confirmation — shown immediately after a successful submission.

Only 2 screens are needed to support the Must Have MVP journey.

---

# Screen: Tour Operator Registration Form

## Purpose

Capture authentication info and business/legal info (including the license file), validate it,
and submit it to create a Tour Operator application. Also used, in a pre-filled state, when a
Guest edits and resubmits a previously rejected application.

## User

Guest (unauthenticated visitor). Includes a Guest returning to resubmit a rejected application.

## Entry Conditions

- New registration: Guest navigates to "Register as Tour Operator".
- Resubmission: Guest arrives here after being notified of a rejection (notification channel not
  yet decided — see Open Questions) to edit their existing application.

## Required Components

### Inputs

- Email
- Password
- Company Name
- Tax Code
- Business contact details
- Travel Business License file upload (PDF or image)

### Information Display

- Field-level validation/error messages, shown per field when present.
- On the resubmission entry path: the previously submitted values are pre-filled.

### Primary Actions

- Submit (creates the application on first submission; resubmits the same application record on
  the edit/resubmission path — see BR8).

### Secondary Actions

None defined in the source User Flow or MVP scope.

## Validation

- Required-field check on all inputs (email, password, Company Name, Tax Code, business contact
  details, license file). *(E1)*
- Format check: email format; Tax Code / License format (jurisdiction-specific rules not yet
  defined — see Open Questions); license file type and size. *(E3, E6)*
- Duplicate email check. *(E2, Must Have)*
- Duplicate business identity check (Tax Code / License already registered) — **Should Have
  only**: BR5 is still an unconfirmed assumption, so this validation may not be present at MVP
  launch. *(E4)*
- On the resubmission path, any duplicate checks (email, business identity) must exclude the
  Guest's own existing application record.

## Error Handling

- **E1 — Missing required field(s):** inline error per field; submission blocked.
- **E2 — Duplicate email:** "Email already registered" error; submission blocked.
- **E3 — Invalid format:** field-level error for Tax Code, License, or contact details;
  submission blocked.
- **E4 — Duplicate business identity:** error indicating Tax Code/License already registered;
  submission blocked. *(conditional — see Validation above)*
- **E5 — System/submission failure:** generic error; no partial account is created; Guest can
  retry.
- **E6 — Invalid license file:** file-specific error (missing, unsupported format, too large);
  submission blocked.

## States

- **Initial / Input** — empty form (new registration).
- **Input (pre-filled)** — resubmission path, previous values loaded.
- **Validation Error** — one or more field/file errors shown (E1, E3, E6).
- **Business Error** — duplicate email or duplicate business identity (E2, E4).
- **Loading** — validating/submitting.
- **Success** — navigates away to the Pending Approval Confirmation screen.

## Navigation

- On successful submission → Pending Approval Confirmation screen.
- On any validation/business/system error → stays on this screen with the relevant error state.
- Entry from an external rejection notification → this screen, in the pre-filled Input state.

## Business Rules

BR4, BR6, BR7, BR8 (see [requirements/register-tour-operator-account.md](../../requirements/register-tour-operator-account.md)).

## Open Questions

- Exact accepted file formats and max size for the license upload (affects E6 messaging).
- Format/validation rules for Tax Code and Travel Business License number.
- Exact fields that make up "business contact details".
- Whether the duplicate business identity check (E4) is actually implemented at MVP launch, given
  BR5 is still an assumption.
- Whether there's a cap on resubmission attempts.
- The notification channel that brings a Guest back to this screen after a rejection.

## UX Suggestions

- Show a short explanatory note that the account will enter Pending Approval and cannot publish
  tours or receive bookings until an Administrator reviews it — not a stated requirement, but
  reduces confusion at submission time.
- Consider relabeling the Submit action (e.g., "Resubmit Application") on the resubmission path
  for clarity — cosmetic only, not a functional requirement.

---

# Screen: Pending Approval Confirmation

## Purpose

Confirm to the Guest that their Tour Operator application was submitted successfully and
communicate that it is now awaiting Administrator approval.

## User

Guest who just successfully submitted a Tour Operator registration (new or resubmitted).

## Entry Conditions

Reached only immediately after a successful submission on the Tour Operator Registration Form.

## Required Components

### Inputs

None — display-only screen.

### Information Display

- Confirmation that the application was received.
- Statement that the account cannot publish tours or receive customer bookings until an
  Administrator approves it.

### Primary Actions

Not defined in the source documents — where the Guest goes from here (e.g., redirected to Login,
or a "Return to Home" action) is unresolved. See Open Questions; not invented here.

### Secondary Actions

None defined.

## Validation

Not applicable — display-only screen.

## Error Handling

Not applicable — this screen is only reached on success.

## States

- **Success** — the only state for this screen.

## Navigation

Reached from: Tour Operator Registration Form, on successful submission.
Leads to: unresolved — see Open Questions.

## Business Rules

FR5, FR6, FR7, FR8 (see [requirements/register-tour-operator-account.md](../../requirements/register-tour-operator-account.md)).

## Open Questions

- Where does the Guest land/navigate to next from this screen (Login, Home, stay here)? Flagged
  as an open assumption in the User Flow document as well — not decided.

## UX Suggestions

- Provide a clear single next-step action (e.g., a button to Login or return Home) so the Guest
  isn't left on a dead-end screen — suggestion only, pending the open question above.
