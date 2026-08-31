# Web Screen Inventory

| # | Screen | Related UC | Actor | Route Suggestion |
|---|---|---|---|---|
| 1 | Traveler Registration | UC-01 | Guest | `/register` |
| 2 | Verify Account | UC-01 | Guest with Pending Verification account | `/verify-account` |

Google Authentication Service is an external authorization handoff, not a TripMate screen to design.

# Screen: Traveler Registration

## Related UC

UC-01 Register Traveler Account.

## Actor

Guest.

## Platform

Responsive Next.js Web.

## Purpose

Create a Traveler account from approved personal/authentication information, enforce uniqueness and password rules, obtain required policy consent, and begin account verification. The Traveler role is system-assigned and must not be selectable.

## Entry Conditions

- Guest is unauthenticated.
- Guest selects Register on the public Landing Page or Sign In screen.

## Entry Points

- TripMate Landing Page.
- Public Sign In.

## Exit / Navigation

- Standard registration success → Verify Account.
- Successful verification → Public Sign In.
- Continue with Google success → Traveler Home / Dashboard after automatic sign-in.
- Back to Sign In → Public Sign In.

## Suggested Route

`/register`

## Main Layout Regions

1. Public TripMate header and return navigation.
2. Registration context/benefit panel appropriate for a Traveler.
3. Account-registration form.
4. Terms and Privacy consent area.
5. Google registration separator/action.
6. Validation and system-message regions.

## Required Data

- Full Name.
- Email Address.
- Password.
- Confirm Password.
- Optional Phone Number.
- Terms of Service and Privacy Policy acceptance.
- Verified Google identity/provider link when Continue with Google is selected.

System-created values such as User ID, role, account status, password hash, profile record, verification code, and timestamps are not editable UI fields.

## Components

### Inputs

- Full Name.
- Email Address.
- Password, masked with accessible visibility control.
- Confirm Password, masked with accessible visibility control.
- Phone Number, optional.
- Required checkbox accepting the Terms of Service and Privacy Policy.

### Read-only Information

- Password-policy guidance.
- Links to the Terms of Service and Privacy Policy.
- Explanation that standard registration requires account verification.
- Exact application messages below.

### Primary Actions

- Register.
- Continue with Google.

### Secondary Actions

- Back to Sign In.

## Table / List Columns

Not applicable.

## Filters / Search

Not applicable.

## Modal / Dialog Behavior

- Policy links may open approved policy pages; modal versus page behavior is not specified.
- Continue with Google performs an external authorization handoff. Do not design Google credentials inside TripMate.

## Validation

- Full Name, Email Address, Password, and Confirm Password are required.
- Email Address must be valid and unique across TripMate.
- Password must contain at least 8 characters, at least one uppercase letter, one lowercase letter, one number, and one special character. The lowercase requirement is present in locked MSG05 even though detailed UC BR-02 omits it; preserve the locked copy and report the rule inconsistency.
- Confirm Password must match Password.
- If Phone Number is supplied, the locked message list defines the valid format as 10 digits beginning with `0`.
- Terms of Service and Privacy Policy must be accepted before submission.
- Google account email remains subject to the email uniqueness rule.

## Business Rules

- **BR-01:** An email address may be registered to at most one TripMate account.
- **BR-02:** Password must contain at least 8 characters and include at least one uppercase letter, one digit, and one special character. See the lowercase inconsistency noted under Validation.
- **BR-03:** Password is stored only as a hash; plain password is never stored, logged, or returned.
- **BR-04:** Traveler role is assigned automatically; no role-selection UI is allowed.
- **BR-05:** Standard registration remains Pending Verification and cannot access protected functions until verification.
- **BR-06:** Google registration creates an Active account; BR-01 still applies to the Google email.

## Application Messages

Use the exact locked content from Report 3 section 5.3:

| Code | Content | Web Usage |
|---|---|---|
| MSG01 | `This field is required.` | Empty required input or consent control. |
| MSG02 | `Invalid email format. Please enter a valid email address (e.g., user@example.com).` | Invalid Email Address. |
| MSG03 | `An account with this email already exists. Please sign in or use another email.` | Duplicate Email Address. |
| MSG04 | `Invalid phone number. Phone number must be 10 digits starting with 0.` | Supplied optional Phone Number is invalid. |
| MSG05 | `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.` | Password-policy failure. |
| MSG06 | `Passwords do not match. Please re-enter.` | Confirm Password mismatch. |
| MSG07 | `Account registered successfully! Please verify your email/OTP to activate your account.` | Standard registration success. |
| MSG127 | `TripMate is temporarily unable to process your request. Please check your connection and try again.` | Account persistence, Google authorization, or other generic failure. |

The detailed UC references MSG03 for password policy, MSG04 for password mismatch, MSG05 for duplicate email, MSG06 for missing consent, MSG07 for an invalid verification code, and MSG08 for registration success. Those references conflict with the locked message list. This Web spec follows the locked message content by semantic context and leaves the missing consent-specific copy unresolved.

See `docs/batch-1-message-reconciliation.md`. The missing consent-specific message may use neutral placeholder wording only as a UX suggestion and does not change the required checkbox or blocked-submit behavior.

## States

### Initial

Empty required fields, optional Phone Number empty, consent unchecked.

### Loading

Registration or Google authorization is in progress; prevent duplicate submission.

### Loaded

Form ready for input.

### Validation Error

MSG01, MSG02, MSG04, MSG05, or MSG06 appears adjacent to the relevant control. Missing-consent copy is unresolved; MSG01 is the only locked generic required-field message.

### Business Error

MSG03 for an email already registered, including a Google email already linked elsewhere.

### System Error

MSG127; no account/profile is created when persistence fails.

### Success

- Standard: MSG07, then Verify Account.
- Google: account is Active, the user is signed in, and navigation continues to Traveler Home / Dashboard; exact post-registration success copy beyond authenticated sign-in is ambiguous.

### Disabled

Register is unavailable while submission is in progress. The SRS does not require disabling it before consent; validation may occur on submit.

### Unauthorized

An authenticated user does not need this public registration flow. Redirect behavior is a UX proposal.

### Responsive

The form and policy consent remain readable without converting to Mobile-app navigation.

## Navigation Map

```text
Landing / Public Sign In
└── Traveler Registration
    ├── Standard success → Verify Account → Public Sign In
    ├── Continue with Google success → Traveler Home / Dashboard
    ├── Back to Sign In → Public Sign In
    └── Error → Traveler Registration
```

## Responsive Behavior

### 1440px

- Use a constrained registration panel with an optional adjacent travel-oriented context region.
- Keep form labels, policy text, and password guidance visible without overly wide controls.

### 1280px

- Preserve the same hierarchy with reduced gutters and a narrower context region.

### 768px

- Stack the context and form, prioritizing the form.
- Keep policy links, Google action, and Back to Sign In visible and keyboard accessible.

## Open Questions

- What exact message should be displayed when Terms/Privacy is not accepted?
- Does detailed BR-02 need to add the lowercase requirement already locked in MSG05?
- What is the lifetime of the Traveler account verification code?
- What URL path will implement the approved Traveler Home / Dashboard destination?
- Should standard registration preserve submitted Email Address when navigating to Verify Account?

## UX Suggestions

- Visually group identity inputs, credential inputs, and consent without creating a multi-step business flow.
- Display password criteria before submission and update their visual status while typing.
- Preserve non-sensitive values on correctable errors; always clear Password and Confirm Password after an uncertain system failure.

# Screen: Verify Account

## Related UC

UC-01 Register Traveler Account.

This remains an independent page rather than a transient state because a Pending Verification user may return from Sign In and request/resubmit a code after the initial registration request has ended.

## Actor

Guest whose standard Traveler registration created a Pending Verification account.

## Platform

Responsive Next.js Web.

## Purpose

Accept the emailed verification code, activate the Pending Verification account after successful verification, and direct the Guest to Sign In.

## Entry Conditions

- Standard Traveler account exists in Pending Verification status.
- A limited-lifetime verification code was issued to the submitted Email Address.

## Entry Points

- Automatic continuation after standard Traveler Registration.
- Resend-verification handoff from Sign In for a Pending Verification account, as described by UC-04.

## Exit / Navigation

- Verified → Public Sign In.
- Resend → remain on Verify Account after requesting a new code.
- Back to Sign In → Public Sign In; account remains Pending Verification.

## Suggested Route

`/verify-account`

## Main Layout Regions

1. Public authentication header.
2. Verification explanation and masked destination context where supported.
3. Verification-code input.
4. Verify and resend actions.
5. Status/error message region.

## Required Data

- Verification code.
- Pending Verification account context.
- Code validity/expiry result.

## Components

### Inputs

- Verification Code.

The SRS does not lock the number of digits for Traveler account verification; do not hard-code a six-cell control as a requirement.

### Read-only Information

- Explanation that protected functions remain unavailable until verification.
- Exact application messages below.

### Primary Actions

- Verify.

### Secondary Actions

- Resend Code.
- Back to Sign In.

## Table / List Columns

Not applicable.

## Filters / Search

Not applicable.

## Modal / Dialog Behavior

None required.

## Validation

- Verification Code is required.
- Code must be correct and unexpired.
- Repeated resend behavior and throttling are not defined.

## Business Rules

- BR-05: Pending Verification account cannot access protected functions.
- Successful code verification records the verification timestamp and changes account status to Active.

## Application Messages

- MSG01: `This field is required.`
- MSG14: `Invalid or expired verification code. Please request a new OTP.`
- MSG15: `A new 6-digit verification code has been sent to your email/phone.`
- MSG127: `TripMate is temporarily unable to process your request. Please check your connection and try again.`

MSG14 and MSG15 are the only locked verification-code messages whose content matches invalid/expired and resend behaviors. The UC-01 reference to MSG07 for invalid/expired code conflicts with the locked list, where MSG07 is registration success.

The code length, expiry duration, and resend cooldown remain unspecified. The page must use a generic verification-code input and must not visually imply a fixed number of digits.

## States

### Initial

Empty verification-code input with delivery guidance.

### Loading

Verification or resend request is processing.

### Loaded

Ready for code input.

### Validation Error

MSG01 for empty code; MSG14 for incorrect/expired code.

### System Error

MSG127.

### Success

Account becomes Active and the Guest is directed to Sign In. No additional locked verification-success message is defined.

### Disabled

Verify/Resend actions are disabled while their request is processing. Cooldown behavior is not specified.

### Responsive

Verification content remains centered and readable at all widths.

## Navigation Map

```text
Traveler Registration → Verify Account
Verify Account ├── Verified → Public Sign In
               ├── Invalid/expired → Verify Account
               ├── Resend → Verify Account
               └── Back → Public Sign In
```

## Responsive Behavior

### 1440px

- Use a compact verification panel within the shared public authentication shell.

### 1280px

- Preserve compact width and visible resend guidance.

### 768px

- Single-column panel; avoid oversized segmented-code controls without a confirmed code length.

## Open Questions

- What is the code length and expiry duration for Traveler account verification?
- What resend cooldown/rate limit applies?
- Is the destination Email Address shown in masked form?
- What exact success message is required after verification?

## UX Suggestions

- Provide clear focus order and paste support for the code input.
- Keep resend feedback visually separate from invalid-code feedback.
