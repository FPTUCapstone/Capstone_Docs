# Screen Inventory

1. Rejection Review & Resubmission Screen — combined view of the rejection reason and the
   editable, pre-filled application form. Reuses the same field set as the Tour Operator
   Registration Form (register-tour-operator-account) rather than introducing a new layout.

Only 1 screen is needed for the Must Have MVP journey (Sign In itself belongs to UC-04, not this
use case).

---

# Screen: Rejection Review & Resubmission Screen

## Purpose

Show the Administrator's rejection reason alongside the previously submitted application, and let
the Tour Operator correct and resubmit it.

## User

Tour Operator, authenticated, with an application in Rejected status.

## Entry Conditions

Tour Operator has successfully signed in (UC-04) and their account's latest application is
Rejected.

## Required Components

### Inputs

Same fields as the Tour Operator Registration Form, pre-filled: Company Name, Tax Code, business
contact details, Travel Business License file upload. *(Auth fields — email/password — are not
part of this screen; identity is already established via Sign In.)*

### Information Display

- Administrator's rejection reason (required — FR3).
- Previously submitted values, pre-filled into the corresponding inputs.

### Primary Actions

- Resubmit (validates and updates the same application record).

### Secondary Actions

None defined in source.

## Validation

Same required-field/format/file validation as the original registration (reused, not redefined —
see register-tour-operator-account-screen-spec.md).

## Error Handling

- **E1 — Corrected submission still fails validation:** field-level errors; stays on this screen.
- **E2 — System/submission failure:** generic error; application status unchanged; Tour Operator
  can retry.

## States

- **Rejection Review** — rejection reason + pre-filled form shown, no edits yet.
- **Editing** — Tour Operator has changed one or more fields/files.
- **Validation Error** — field-level errors shown (E1).
- **Loading** — submitting.
- **Success** — transitions to a confirmation state/screen (not detailed further; same open
  question as the original registration's post-submission destination).

## Navigation

- Entry: from Sign In (UC-04), only when the application is Rejected.
- On success → confirmation (destination undecided, see Open Questions).
- On error → stays on this screen.

## Business Rules

BR1, BR2, BR3 (see [requirements/resubmit-tour-operator-application.md](../../requirements/resubmit-tour-operator-application.md)).

## Open Questions

- What is shown/happens if the Tour Operator reaches this screen without a Rejected application?
- Where does the Tour Operator land after a successful resubmission?
- Is prior rejection history shown, or only the latest reason?
- Is there a cap on resubmission attempts?

## UX Suggestions

- Visually highlight which fields the rejection reason refers to, if the reason references
  specific fields — not a confirmed requirement, but reduces the chance of the Tour Operator
  fixing the wrong thing.
