# Web Screen Inventory

| # | Screen | Related UC | Actor | Route Suggestion |
|---|---|---|---|---|
| 1 | Public Password Recovery | UC-06 | Unauthenticated Traveler or Tour Operator | `/forgot-password` |
| 2 | Admin Password Recovery | UC-06 | Unauthenticated Administrator | `/admin/forgot-password` |

Each row is the same progressive Password Recovery page in a different Web shell. `Forgot Password`, `Enter Reset Code`, and `Set New Password` are sequential states, not independent pages or routes.

# Screen: Password Recovery

## Related UC

UC-06 Reset Password.

Report 3 is canonical for Web behavior: an email-delivered, single-use reset code is verified before a new password is accepted. Older repository artifacts describe an email reset-link flow; that flow is not mixed into this Web specification.

## Actor

Unauthenticated registered Traveler, Tour Operator, or Administrator who cannot recall the account password.

## Platform

Responsive Next.js Web.

## Purpose

Provide one coherent recovery task that requests a reset code, verifies it, accepts a valid new password, invalidates existing sessions, and returns the user to the matching Sign In context.

## Entry Conditions

- User is unauthenticated.
- User selects Forgot Password from Public Sign In or Admin Login.

## Entry Points

- Public Sign In → Public Password Recovery.
- Admin Login → Admin Password Recovery.

## Exit / Navigation

- Successful password reset → matching Public Sign In or Admin Login.
- Back to Sign In from the request state → matching Login.
- Invalid/expired/consumed reset code → remain in recovery and allow a new code request.
- Validation/system failure → remain in the current recovery state.

## Suggested Route

- Public: `/forgot-password`.
- Admin: `/admin/forgot-password`.

`/verify-reset-code`, `/reset-password`, `/admin/verify-reset-code`, and `/admin/reset-password` are unnecessary route proposals because their content is represented by progressive states.

## Main Layout Regions

1. Matching public authentication or Admin operational authentication header/context.
2. Recovery progress/context area.
3. One active form state at a time: Request, Reset Code Entry, or Set New Password.
4. Inline validation and form-level status region.
5. Primary and state-appropriate secondary actions.

The public and Admin contexts share form controls, typography, validation placement, and accessibility. Public context may use restrained TripMate travel identity; Admin context remains operational and does not show consumer registration/promotional content.

## Required Data

### Request state

- Email Address.

### Reset Code Entry state

- Reset Code.
- Reset request context required to verify or resend without disclosing whether an account exists.

### Set New Password state

- New Password.
- Confirm New Password.
- Verified reset context.

System-only reset code, expiry, consumed flag, password hash, invalidated sessions, and timestamps are not editable UI data.

## Components

### Inputs

- Request state: Email Address.
- Reset Code Entry state: Reset Code.
- Set New Password state: New Password and Confirm New Password, masked with accessible visibility controls.

The code length is not fixed by the detailed UC. MSG15 mentions six digits, but this is insufficient to establish the input length because the detailed flow does not. Use a generic code input that does not visually force a digit count.

### Read-only Information

- Request guidance explaining that a code is sent to the registered Email Address when an account exists.
- Non-disclosing request acknowledgement; exact locked copy is missing.
- Reset-code 15-minute expiry guidance.
- Password-policy guidance.
- Exact locked messages listed below.

### Primary Actions

- Send Reset Code in Request state.
- Verify in Reset Code Entry state.
- Reset Password in Set New Password state.
- Sign In after Password Reset Success.

### Secondary Actions

- Back to Sign In in Request state.
- Resend Code in Reset Code Entry state.
- Request New Code after an invalid/expired/consumed reset context.

## Table / List Columns

Not applicable.

## Filters / Search

Not applicable.

## Modal / Dialog Behavior

None. State changes occur within the same recovery page and shell.

## Validation

### Request state

- Email Address is required and must use a valid format.
- Account existence is not disclosed.

### Reset Code Entry state

- Reset Code is required.
- Code must be correct, unexpired, and unconsumed.
- Reset code is single-use and expires 15 minutes after issue.
- Resend behavior exists; cooldown/rate limit and prior-code invalidation are not defined.

### Set New Password state

- New Password and Confirm New Password are required.
- New Password must satisfy locked MSG05: at least 8 characters with uppercase, lowercase, number, and special character. Detailed BR-02 omits lowercase; the UI uses the locked policy copy without rewriting the BR.
- Confirm New Password must match.
- Reset context must remain valid and unconsumed when the password is submitted.

## Business Rules

- **BR-02:** New password contains at least 8 characters, one uppercase letter, one digit, and one special character; see the lowercase message inconsistency above.
- **BR-03:** New password is stored only as a hash.
- **BR-14:** Reset code is single-use and expires 15 minutes after issue.
- **BR-15:** Completed reset invalidates existing sessions.
- Account lookup results are never disclosed by the request response.

## Application Messages

Use exact locked content where it matches:

| Code | Content | Web Usage |
|---|---|---|
| MSG01 | `This field is required.` | Empty required input in any state. |
| MSG02 | `Invalid email format. Please enter a valid email address (e.g., user@example.com).` | Invalid Email Address. |
| MSG05 | `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.` | New Password policy failure. |
| MSG06 | `Passwords do not match. Please re-enter.` | Confirm New Password mismatch. |
| MSG14 | `Invalid or expired verification code. Please request a new OTP.` | Incorrect, expired, or consumed reset code. |
| MSG15 | `A new 6-digit verification code has been sent to your email/phone.` | Resend success, with the code-length inconsistency noted above. |
| MSG16 | `Your password has been reset successfully. Please sign in with your new password.` | Password Reset Success state. |
| MSG127 | `TripMate is temporarily unable to process your request. Please check your connection and try again.` | Generic request, delivery, verification, or persistence failure. |

The detailed UC points to wrong codes for request acknowledgement, invalid code, password policy, and password mismatch. `docs/batch-1-message-reconciliation.md` records each mismatch. The initial non-disclosing request acknowledgement has no valid locked message; use neutral placeholder copy only as a UX suggestion.

## States

### Initial

Request state with empty Email Address.

### Loading

Current primary action is processing. Prevent duplicate submission and preserve non-sensitive context.

### Loaded

Current recovery state is ready for input.

### Empty

Not applicable; every state contains its required form or success content.

### Validation Error

- Request: MSG01 or MSG02.
- Reset Code Entry: MSG01 or MSG14.
- Set New Password: MSG01, MSG05, or MSG06.

### Business Error

- Invalid, expired, or consumed code uses MSG14.
- A non-matching Email Address does not produce a distinct error and must not change the request acknowledgement.

### System Error

MSG127. Password remains unchanged when persistence fails; reset code is not consumed by that failure.

### Success

Progressive success behavior:

1. Request submitted → neutral non-disclosing acknowledgement and Reset Code Entry state.
2. Code verified → Set New Password state.
3. Password updated → MSG16, existing sessions invalidated, then matching Sign In.

### Confirmation

Password Reset Success is a state within this page, not a standalone confirmation page or modal.

### Disabled

The active state’s primary action is disabled only while processing or when the reset context is no longer valid. No resend cooldown UI is required until a rule is approved.

### Unauthorized

An already-authenticated user does not require this recovery page. Redirect behavior is an architecture proposal and not part of the Stitch target.

### Responsive

The active form, progress context, messages, and actions remain accessible at all supported widths.

## Navigation Map

```text
Public Sign In / Admin Login
└── Password Recovery — Request state
    ├── Submitted → Reset Code Entry state
    │   ├── Verified → Set New Password state
    │   │   └── Success → Password Reset Success → matching Sign In
    │   ├── Invalid/expired → Reset Code Entry state
    │   └── Resend → Reset Code Entry state
    ├── Back → matching Sign In
    └── System error → current state
```

## Responsive Behavior

### 1440px

- Use a constrained progressive form panel.
- Public context may have an adjacent restrained travel/product panel.
- Admin context uses a professional operational background and no consumer promotion.

### 1280px

- Preserve compact form width and visible progress/state heading.

### 768px

- Use a single-column layout.
- Keep the active primary action and state-specific secondary action visible without horizontal scrolling.
- Do not adopt Mobile-app navigation.

## Open Questions

- What exact locked non-disclosing acknowledgement should follow the initial reset request?
- Is the reset code definitively six digits?
- Does Resend Code invalidate the previous code immediately?
- What resend cooldown/rate limit applies?
- Should the 15-minute lifetime be static guidance or a live countdown?

These questions do not alter the progressive page structure, required controls, validation placement, primary actions, or success navigation. Stitch may use generic code input and neutral placeholder acknowledgement.

## UX Suggestions

- Show a concise state title such as “Reset your password,” “Enter reset code,” and “Set a new password” while keeping one shell.
- Support code paste without assuming a fixed digit layout.
- Use neutral placeholder acknowledgement such as “If an account matches this email, a reset code has been sent.” This wording is not a locked requirement.
- Keep Password Reset Success visible long enough for the user to understand that a new sign-in is required.
