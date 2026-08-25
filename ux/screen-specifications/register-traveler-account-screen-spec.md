# Screen Specification: Register Traveler Account

> Source documents:
> - Use Case Analysis: `requirements/register-traveler-account.md`
> - MVP Planning: `requirements/mvp/register-traveler-account.md`
> - User Flow: `ux/user-flows/register-traveler-account-user-flow.md`

# Screen Inventory

1. **Registration Screen** — primary screen for this feature; full spec below.
2. **Login Screen** — required navigation destination after successful registration (BR5). Only the elements referenced by the approved user flow are specified here; a full Login use case has not been analyzed, so this entry is intentionally minimal and heavily flagged with open questions.

**Not specified as a screen in this document:** the Traveler home/landing screen reached after login. The user flow explicitly leaves this destination as an open question ("destination screen not yet specified"), and no source document describes its content or requirements. Inventing one would violate the "do not invent requirements" rule, so it is excluded pending a use case/MVP decision.

---

# Screen: Registration Screen

## Purpose

Let a Guest submit the information required to create a TripMate account (full name, email, phone number, password) so the system can create the account and assign the Traveler role. *(MVP Must Have — FR1–FR4, BR1–BR4)*

## User

Guest (unauthenticated visitor).

## Entry Conditions

- Guest is unauthenticated and reaches the Registration screen. *(Explicit — user flow Entry Point)*
- Exact entry mechanism (e.g., a "Sign Up" link from Login or a landing page) is not specified in source. *(UX Suggestion — see user flow Entry Point)*

## Required Components

### Inputs

- Full name (text)
- Email address (text)
- Phone number (text)
- Password (masked text)

*(All four fields are required — BR3. No optional fields are defined for MVP.)*

### Information Display

- Terms of Service / Privacy Policy text annotation with a link, positioned below the Register button (e.g., "By registering, you agree to the Terms & Privacy Policy"). No separate consent checkbox. *(BR7)*
- Inline error messages, shown per triggering condition (see Error Handling).

### Primary Actions

- **Register** — submits the form for validation and account creation.

### Secondary Actions

- Link to the Login screen for a Guest who already has an account. *(UX Suggestion — not confirmed in source; flagged as Open Question in the user flow)*

## Validation

Performed on submit, before account creation *(FR2)*:

- All four fields are present (not empty).
- Email is a valid email format.
- Password meets policy: minimum 8 characters, at least 1 uppercase letter, 1 lowercase letter, 1 number, and 1 special character. *(BR4)*
- Phone number format: no normalization or format rule is defined for MVP. *(Open Question — still open in source; acceptable risk for MVP)*

If validation fails, the submission is rejected before any uniqueness check or account creation occurs.

## Error Handling

- **Validation Error (E1):** Inline, field-level error message (e.g., "Password is not 8 characters long"). Guest remains on the Registration screen to correct input.
- **Business Error — Duplicate Account (E2):** Triggered when email or phone number already exists on another account. Inline error message (e.g., "Email already exists"). Guest remains on the Registration screen to correct input.
- Whether previously entered (non-sensitive) field values are retained after an error, or the form clears, is **unresolved**. *(Open Question — carried from user flow)*

## States

- **Initial** — empty form, no input yet.
- **Input** — Guest is filling in fields.
- **Validation Error** — field-level error(s) shown after a failed submit (E1).
- **Business Error** — duplicate-account error shown after a failed submit (E2).
- **Loading** — submission in progress, awaiting validation/uniqueness/creation result. *(UX Suggestion — a loading state is not explicitly described in source but is implied by a server round-trip; not a confirmed requirement)*
- **Success** — account created; screen transitions to redirect (no confirmation screen is defined — redirect is immediate per BR5).

## Navigation

- On successful submission → redirect to **Login Screen** (no auto-login). *(BR5)*
- On validation or duplicate error → remain on **Registration Screen**.
- Secondary link to **Login Screen** (if included) → Login Screen. *(UX Suggestion)*

## Business Rules

- BR1: Role is always Traveler; no role selection UI.
- BR2 / BR3: Email and phone are both required and both unique.
- BR4: Password policy as stated above.
- BR5: Redirect to Login on success; no auto-login.
- BR6: Email is the login identifier; phone is not used for login.
- BR7: ToS/Privacy shown as text + link, no checkbox.

## Open Questions

- Does the form retain non-sensitive field values (name, email, phone) after a validation/duplicate error, or must the Guest re-enter everything?
- Is there a confirmed entry path (e.g., a "Sign Up" link) into this screen, and where does it originate?
- What phone number format/normalization, if any, is enforced at input time?
- Is a loading/submitting state required, or does the UI simply wait for a response?

## UX Suggestions

- Provide a "Sign Up" entry link from the Login screen or landing page.
- Show a loading indicator on the Register button while the request is in flight.
- Place inline validation messages directly beneath the associated field.
- Retain non-sensitive field values (name, email, phone) after an error so the Guest doesn't re-enter everything.

---

# Screen: Login Screen

> **Scope note:** Login is not the subject of the analyzed use case. It is included here only because the approved Register Traveler Account user flow requires it as the post-registration redirect destination (BR5) and shows the Guest logging in manually as part of reaching the Success State. Fields, states, and rules below are limited strictly to what the user flow states; no independent Login use case analysis exists. Do not treat this as a complete Login specification.

## Purpose

Let a newly registered (or returning) user authenticate with email and password after being redirected here from Registration. *(Explicit — user flow Step 5–6)*

## User

Guest who just completed registration (per this flow); more broadly, any user with an existing account. *(Only the post-registration case is in scope of the source documents.)*

## Entry Conditions

- Automatic redirect immediately after successful registration, with no auto-login. *(BR5)*

## Required Components

### Inputs

- Email address (text)
- Password (masked text)

*(Explicit — user flow Step 6: "Guest enters email + password and logs in manually.")*

### Information Display

- None specified in source beyond the login form itself.

### Primary Actions

- **Log in** — submits credentials for authentication.

### Secondary Actions

- Link back to the Registration screen for Guests without an account. *(UX Suggestion — flagged as an Open Question in the user flow, not confirmed)*

## Validation

Not specified in source. *(Open Question — field-level validation behavior for Login is undefined)*

## Error Handling

Not specified in source. *(Open Question — behavior on authentication failure, e.g., wrong password or unknown email, is undefined)*

## States

- **Initial** — empty form, awaiting credentials. *(Explicit — user flow "Login screen, awaiting credentials.")*
- **Input** — user entering credentials.
- **Success** — authentication succeeds; Traveler session granted. *(Explicit — user flow Step 6)*
- Error/failure state: **not defined** — see Open Questions.

## Navigation

- On success → Traveler home/landing screen. **Destination not specified** — open question, out of scope for this document. *(Explicit gap noted in user flow Open Questions)*
- Secondary link to Registration screen, if included → Registration Screen. *(UX Suggestion)*

## Business Rules

- BR6: Email is the login identifier (used here, not phone).

## Open Questions

- What screen does the user land on after a successful login (Traveler home/dashboard)? Not specified in any source document.
- What validation and error behavior applies to failed login attempts (wrong password, unknown email, etc.)?
- Is there a visible link from Login back to Registration for Guests who don't yet have an account?
- Does Login support anything beyond email/password for this flow (e.g., "remember me", password reset)? Not mentioned in source — assume out of scope until a Login use case is analyzed.

## UX Suggestions

- Provide a link from Login to Registration for new Guests.
