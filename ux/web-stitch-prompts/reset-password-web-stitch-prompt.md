# Product Context

TripMate provides password recovery in both public and administrator sign-in contexts. Each context uses one progressive recovery page that preserves its own shell while moving through email request, code entry, new-password creation, and success.

# Target Platform

Responsive desktop Web application. Design public and administrator password-recovery variants at 1440 px, 1280 px, and 768 px with no horizontal scrolling.

# Target User

- A traveler or tour operator who cannot access the public sign-in flow.
- A TripMate administrator who cannot access the administrator login flow.

# Screen Objective

Help the user request password recovery, enter a reset code, set a compliant new password, and return to the matching sign-in context without disclosing whether an account exists.

# Connected Screens

- Public Sign In.
- Admin Login.
- Email Request state.
- Reset Code Entry state.
- Set New Password state.
- Password Reset Success state.

# Layout Structure

## Public Recovery Shell

1. Shared public authentication identity region with restrained Central Vietnam detail.
2. Constrained progressive recovery panel.
3. Back to Public Sign In navigation.

## Administrator Recovery Shell

1. Restrained operational administrator identity area.
2. Constrained progressive recovery panel using the same form system.
3. Back to Admin Login navigation.

## Progressive Panel States

1. Request: email field and recovery action.
2. Reset Code Entry: generic code input, Continue, and Resend Code.
3. Set New Password: new password and confirmation fields.
4. Success: concise confirmation and return-to-sign-in action.

# Required Content

## Request

- Email Address.
- Send Reset Code primary action.
- Neutral non-disclosing acknowledgement after submission.

## Reset Code Entry

- Generic Reset Code input with no fixed digit count in the visual treatment.
- Continue.
- Resend Code.
- Request New Code after an invalid, expired, or consumed reset context.
- Explanation that the code expires after 15 minutes and is single-use.

## Set New Password

- New Password.
- Confirm New Password.
- Visible password requirements.
- Reset Password.

## Success

- `Your password has been reset successfully. Please sign in with your new password.`
- Return to the matching Public Sign In or Admin Login context.

# Required Components

- Shared progressive authentication panel.
- Persistently labeled email, reset-code, password, and confirmation fields.
- Accessible password visibility controls.
- Password requirement guidance.
- Primary and secondary buttons.
- Resend action.
- Request New Code action for an invalid, expired, or consumed reset context.
- Subtle state-progress context that does not imply separate routes.
- Inline field errors, form-level alert, loading state, acknowledgement panel, and success panel.

# Required States

- **Initial:** Public or administrator Request state with an empty Email Address.
- **Loading:** The current primary action is processing; prevent duplicate submission and preserve non-sensitive context.
- **Loaded:** The current progressive recovery state is ready for input.
- **Empty:** Not applicable; each state contains its required form or success content.
- **Validation Error:** Request uses `This field is required.` or `Invalid email format. Please enter a valid email address (e.g., user@example.com).` Reset Code Entry uses `This field is required.` or `Invalid or expired verification code. Please request a new OTP.` Set New Password uses `This field is required.`, `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.`, or `Passwords do not match. Please re-enter.`
- **Business Error:** An invalid, expired, or consumed code uses `Invalid or expired verification code. Please request a new OTP.` A non-matching email does not produce a distinct account-existence error.
- **System Error:** `TripMate is temporarily unable to process your request. Please check your connection and try again.`
- **Success:** Request moves to neutral acknowledgement and Reset Code Entry; verified code moves to Set New Password; completed reset uses `Your password has been reset successfully. Please sign in with your new password.` and returns to matching sign-in.
- **Confirmation:** Password Reset Success is a state in this page, not a separate page or modal.
- **Disabled Action:** Disable the active primary action only while processing or when recovery context is no longer valid. Do not invent a resend cooldown.
- **Unauthorized:** An already authenticated user does not require this recovery design; redirect behavior is outside the Stitch target.
- **Responsive:** The active form, progress context, messages, and actions remain accessible at every target width.
- **Resend Success:** `A new 6-digit verification code has been sent to your email/phone.` Preserve this locked copy while keeping the input control visually length-neutral.

# Navigation Context

- Public recovery uses the proposed `/forgot-password` destination for every progressive state.
- Administrator recovery uses the proposed `/admin/forgot-password` destination for every progressive state.
- Public success returns to the proposed `/sign-in` destination.
- Administrator success returns to the proposed `/admin/login` destination.
- Reset Code Entry, Set New Password, and Password Reset Success are states of the recovery page, not separate route designs.
- Routes are design context, not implementation requirements.

# Responsive Behavior

- At 1440 px, use a centered two-region public layout and a narrower focused administrator panel.
- At 1280 px, reduce outer spacing while keeping the progressive form calm and readable.
- At 768 px, stack or simplify the public identity region; keep both contexts in one column with full-width primary actions where appropriate.
- Long error and acknowledgement text wraps naturally with no horizontal scrolling.
- State transitions do not cause large unpredictable layout shifts.

# Visual Direction

- Shared authentication palette: deep navy, teal, limited coral, and light-neutral surfaces.
- Public recovery may include restrained professional Central Vietnam imagery or geographic detail.
- Administrator recovery uses the same form and state system but is operational and restrained, without tourism marketing cards.
- Maintain calm, trustworthy spacing and avoid playful progress graphics.

# Accessibility

- Persistent input labels and text descriptions for current state.
- Associated error messages, strong contrast, visible focus, logical keyboard order, and comfortable target sizes.
- Password rules are available before submission.
- Show/hide password controls have accessible labels and state.
- Acknowledgement, errors, loading, and success are perceivable and do not rely on color alone.
- Resend and return links are keyboard accessible and clearly named.

# UX Constraints

- The initial acknowledgement must be neutral and must not reveal whether an account exists.
- Use a generic reset-code input; do not fix the code at a specific number of digits.
- The approved rule is a 15-minute, single-use reset code. Do not invent any additional code policy or resend cooldown.
- Keep all progressive states in the same public or administrator recovery shell.
- Use exact approved messages only in matching contexts.
- Use the locked resend message exactly in the successful resend state, while keeping the reset-code input visually length-neutral because the detailed flow does not establish a fixed input length.

# Things NOT to Design

- No separate routes or unrelated page shells for Reset Code Entry, Set New Password, or Password Reset Success.
- No Change Password screen for an already authenticated user.
- No fixed-length code boxes or six-digit code label.
- No account-existence disclosure.
- No Google authorization.
- No tourism marketing cards in the administrator context.
- No dashboards or signed-in product screens.
- No mobile application screen.
- No API, email service, authentication, session, or database implementation.

# Open Questions

- Exact neutral initial acknowledgement copy is not approved.
- Reset-code digit length and resend cooldown are unresolved.
- Whether resending immediately invalidates the prior code is unresolved.
- Whether the approved 15-minute lifetime appears as static guidance or a live countdown is unresolved; no countdown is required.
- The approved resend message mentions a six-digit code while the detailed flow leaves input length unresolved; preserve the locked message but do not translate it into a six-cell control requirement.
- Final production recovery routes and delivery-channel behavior remain implementation decisions.

# Stitch Prompt

Design a responsive desktop Web application, not a mobile app.

Create a progressive TripMate password-recovery experience in two visual contexts: Public Password Recovery and Admin Password Recovery. Produce responsive layouts at 1440 px, 1280 px, and 768 px with no horizontal scrolling. Each context must keep one stable page shell while progressing through Email Request, Reset Code Entry, Set New Password, and Password Reset Success; do not create unrelated page designs for those states.

Use the shared TripMate authentication system: deep navy structure, teal interactive accents, limited coral for important error emphasis, light-neutral form surfaces, generous whitespace, accessible typography, and consistent inputs, buttons, alerts, and focus states. Public recovery may include restrained professional Central Vietnam imagery or a subtle geographic motif. Admin recovery must be operational and restrained, with no tourism marketing cards.

For both contexts, design these connected states:

1. Email Request: a persistently labeled Email Address field, primary Send Reset Code action, matching return-to-sign-in link, loading treatment, email validation, and a neutral non-disclosing acknowledgement after submission. The acknowledgement must not confirm whether an account exists. Use a placeholder such as “Neutral acknowledgement copy to be confirmed” because exact approved copy is unavailable.
2. Reset Code Entry: one generic Reset Code input, Continue, Resend Code, Request New Code after an invalid, expired, or consumed context, and helper text stating that the reset code expires after 15 minutes and can be used once. Do not show a fixed number of digit boxes or add a digit count to the input label, helper text, or placeholder. For a successful resend, preserve the exact locked message listed below even though the input control itself remains visually length-neutral.
3. Set New Password: labeled New Password and Confirm New Password fields, accessible show/hide controls, visible password requirements, and a primary Reset Password action.
4. Password Reset Success: a concise success panel using the exact message `Your password has been reset successfully. Please sign in with your new password.` and an action back to the matching sign-in screen.

Use these exact approved messages only in their matching states:

- `This field is required.`
- `Invalid email format. Please enter a valid email address (e.g., user@example.com).`
- `Invalid or expired verification code. Please request a new OTP.`
- `A new 6-digit verification code has been sent to your email/phone.`
- `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.`
- `Passwords do not match. Please re-enter.`
- `Your password has been reset successfully. Please sign in with your new password.`
- `TripMate is temporarily unable to process your request. Please check your connection and try again.`

Represent proposed navigation relationships without exposing technical paths as user-facing copy: all public recovery states remain at `/forgot-password` and return to `/sign-in`; all administrator recovery states remain at `/admin/forgot-password` and return to `/admin/login`.

At 1440 px use a balanced public identity-and-form composition and a focused narrower admin panel. At 1280 px tighten outer spacing while keeping the progressive panel stable. At 768 px stack or simplify the public identity region and use a single-column form with clear primary actions. Keep state transitions visually stable. Ensure persistent labels, associated errors, visible password requirements, strong contrast, visible keyboard focus, logical tab order, comfortable targets, perceivable loading and success feedback, naturally wrapping messages, and no clipped content or horizontal scrolling.
