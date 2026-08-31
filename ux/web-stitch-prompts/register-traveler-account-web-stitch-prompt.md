# Product Context

TripMate lets a public visitor create a traveler account and then verify it before using the signed-in travel experience. Google registration is available only as an external provider handoff.

# Target Platform

Responsive desktop Web application. Design Traveler Registration and its connected Verify Account page at 1440 px, 1280 px, and 768 px with no horizontal scrolling.

# Target User

- A new traveler creating a TripMate account with email and password.
- A new traveler choosing the external Google account path.
- A newly registered traveler completing account verification.

# Screen Objective

Collect the approved minimum traveler account information, communicate validation clearly, obtain required policy consent, and move standard registration to account verification.

# Connected Screens

- TripMate Landing Page.
- Public Sign In.
- External Google authorization handoff.
- Verify Account.
- Traveler Home or Dashboard after a successful Google registration path, subject to final implementation.

# Layout Structure

## Traveler Registration

1. Shared public authentication shell with a restrained Central Vietnam identity region.
2. Focused registration panel with account fields grouped in a logical sequence.
3. Required policy-consent row.
4. Primary registration action, external Google handoff, and sign-in return link.
5. Inline and form-level feedback regions.

## Verify Account

1. The same public authentication shell and brand context.
2. Concise explanation of account verification.
3. Generic verification-code field.
4. Verify, Resend Code, and Back to Sign In actions.
5. Inline, loading, resend-feedback, and service-failure regions.

# Required Content

## Traveler Registration

- Full Name, required.
- Email Address, required.
- Phone Number, optional.
- Password, required.
- Confirm Password, required.
- Required acceptance of Terms and Privacy Policy.
- Register.
- Continue with Google.
- Back to Sign In.

## Verify Account

- Generic Verification Code input without a fixed digit count in the visual design.
- Verify.
- Resend Code.
- Back to Sign In.

# Required Components

- Shared public authentication shell.
- Persistently labeled text, email, phone, password, and confirmation fields.
- Accessible password visibility controls.
- Password guidance region.
- Required policy-consent checkbox with Terms and Privacy Policy links.
- Primary Register and Verify buttons.
- Continue with Google external-handoff button on registration only.
- Resend Code text button or secondary button.
- Inline field validation and form-level feedback.
- Loading states that prevent repeated submission.

# Required States

## Traveler Registration

- **Initial:** Required fields are empty, optional Phone Number is empty, and consent is unchecked.
- **Loading:** Registration or external Google authorization is in progress; prevent repeated submission.
- **Loaded:** Form is ready for input.
- **Validation Error:** Required field uses `This field is required.` Invalid email uses `Invalid email format. Please enter a valid email address (e.g., user@example.com).` Invalid optional phone uses `Invalid phone number. Phone number must be 10 digits starting with 0.` Weak password uses `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.` Password mismatch uses `Passwords do not match. Please re-enter.`
- **Business Error:** Existing account uses `An account with this email already exists. Please sign in or use another email.`
- **System Error:** `TripMate is temporarily unable to process your request. Please check your connection and try again.`
- **Success:** Standard registration uses `Account registered successfully! Please verify your email/OTP to activate your account.` and proceeds to Verify Account. The Google return outcome uses neutral copy where needed.
- **Disabled Action:** Disable Register only while registration is processing; no other persistent disabled rule is specified.
- **Responsive:** The form and consent controls remain readable and operable at every target width.
- Neutral placeholder for missing Terms and Privacy acceptance copy.
- Google registration handoff and return treatment without provider-owned UI.

## Verify Account

- **Initial / Loaded:** Verification form is ready with one generic code input.
- **Loading:** Verification or resend request is processing.
- **Validation Error / Business Error:** Empty code uses `This field is required.` Invalid or expired code uses `Invalid or expired verification code. Please request a new OTP.`
- **System Error:** `TripMate is temporarily unable to process your request. Please check your connection and try again.`
- **Success:** Account becomes active and the traveler is directed to Public Sign In. Use a neutral placeholder if the visual requires success copy because no exact locked verification-success message exists.
- **Disabled Action:** Disable Verify or Resend while its request is processing; do not invent a resend cooldown.
- **Responsive:** Keep verification content centered and readable at every target width.
- **Resend:** Use neutral placeholder confirmation without visually fixing a code length because this context's locked-message match is ambiguous.

# Navigation Context

- Traveler Registration uses the proposed `/register` destination.
- Standard registration success leads to the proposed `/verify-account` destination.
- Successful account verification returns to the proposed `/sign-in` destination.
- Back to Sign In leads to the proposed `/sign-in` destination.
- Terms and Privacy Policy links open the approved policy destinations when defined.
- Google authorization is an external handoff. A successful Google registration may continue to a proposed traveler destination such as `/traveler`; this is navigation context, not an implementation requirement.

# Responsive Behavior

- At 1440 px, use a centered public authentication composition with a clearly constrained form width.
- At 1280 px, reduce outer spacing while keeping field groups and consent text easy to scan.
- At 768 px, stack or simplify the identity region, keep fields in one column, wrap policy text without overlap, and keep primary actions prominent.
- Do not place form controls side by side when doing so compromises labels, errors, or touch targets.
- No state may introduce horizontal scrolling.

# Visual Direction

- Shared public TripMate authentication language: deep navy, teal, selective coral, and light-neutral form surfaces.
- Professional Central Vietnam imagery or subtle geographic detail may support the public identity region.
- Keep the experience modern, credible, welcoming, and spacious rather than playful.
- Use consistent field, alert, button, checkbox, and verification treatments with Public Sign In.

# Accessibility

- Every input has a persistent visible label and programmatically associated error region.
- Required and optional status is communicated in text, not color alone.
- Password requirements remain visible and readable.
- Policy links and checkbox form one understandable consent statement.
- Provide strong contrast, visible focus, logical keyboard order, and comfortable control targets.
- Verification and resend feedback is perceivable without relying only on color.

# UX Constraints

- Google provider UI is external and must not be designed.
- Do not define a verification-code digit length, individual digit boxes, or an expiry duration not approved by the specification.
- Use a generic single verification-code input or a visually length-neutral equivalent.
- Use exact approved messages only in their matching contexts.
- Where Terms acceptance or Google-success copy is unresolved, use neutral placeholder text instead of inventing policy or behavior.
- Standard email registration moves to Verify Account; do not present it as already active.

# Things NOT to Design

- No Google provider authorization screen.
- No mobile application screen.
- No traveler profile setup, onboarding questionnaire, itinerary creation, or dashboard.
- No fixed-length verification-code UI.
- No invented resend cooldown, expiry duration, password-strength score, or extra personal fields.
- No optional marketing-consent checkbox.
- No implementation details for API, database, authentication service, or session storage.

# Open Questions

- Exact Terms and Privacy acceptance validation copy is not approved.
- The detailed password rule and locked password message differ on the lowercase requirement; preserve the locked message and do not silently rewrite it.
- Exact Google registration success copy and final destination remain unresolved.
- Whether the submitted Email Address is preserved into Verify Account and whether the destination is displayed in masked form are unresolved.
- Verification-code length, expiry duration, resend cooldown, and verification-success copy are not approved.
- Final policy URLs and production routes remain implementation decisions.

# Stitch Prompt

Design a responsive desktop Web application, not a mobile app.

Create two connected TripMate Web screens: Traveler Registration and Verify Account. Produce responsive layouts at 1440 px, 1280 px, and 768 px with no horizontal scrolling. Use the same public authentication design system as TripMate sign-in: deep navy structure, teal interactive accents, limited coral emphasis, light-neutral form surfaces, generous whitespace, restrained geographic details, and professional Central Vietnam imagery. The tone is welcoming and credible, not playful.

Traveler Registration screen:

- Use a shared public authentication shell with a restrained travel-identity region and a focused registration panel.
- Include persistently labeled fields for Full Name, Email Address, optional Phone Number, Password, and Confirm Password.
- Include accessible password visibility controls and visible password requirements.
- Include one required checkbox statement accepting Terms and Privacy Policy, with both policies presented as links.
- Provide a primary Register button, a Continue with Google external-handoff button, and Back to Sign In.
- Google authorization is external; do not create the provider-owned authorization interface.

Verify Account screen:

- Reuse the same shell, typography, panel, and feedback system.
- Explain that the new account must be verified.
- Include one generic Verification Code input, a primary Verify button, Resend Code, and Back to Sign In.
- Do not imply a fixed digit count through separated boxes, labels, helper text, or placeholders.

Show useful registration states: initial, loaded, inline validation, existing account, loading with Register disabled, standard success that continues to Verify Account, Google external-handoff return, and temporary service failure. Show verification states: initial, loaded, empty code, invalid or expired code, resend confirmation, loading with Verify or Resend disabled, successful activation that returns to Public Sign In, and temporary service failure. No exact locked verification-success message exists, so use neutral placeholder copy if that visual state requires text. Use these exact approved messages only in their matching contexts:

- `This field is required.`
- `Invalid email format. Please enter a valid email address (e.g., user@example.com).`
- `An account with this email already exists. Please sign in or use another email.`
- `Invalid phone number. Phone number must be 10 digits starting with 0.`
- `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.`
- `Passwords do not match. Please re-enter.`
- `Account registered successfully! Please verify your email/OTP to activate your account.`
- `Invalid or expired verification code. Please request a new OTP.`
- `TripMate is temporarily unable to process your request. Please check your connection and try again.`

For unresolved Terms acceptance copy and Google-success copy, show a neutral placeholder such as “Message to be confirmed” rather than inventing policy. For resend confirmation, use a neutral visual placeholder and do not reproduce copy that fixes the code at six digits.

Represent the proposed navigation relationships without displaying technical paths as user-facing copy: registration `/register`, verification `/verify-account`, sign-in `/sign-in`, and a possible post-Google traveler destination `/traveler`.

At 1440 px use a centered authentication composition and constrained form. At 1280 px reduce outer spacing without compressing labels or consent text. At 768 px stack or simplify the identity region, use a single-column form, allow policy text and errors to wrap naturally, and keep primary actions obvious. Include accessible contrast, persistent labels, required/optional text indicators, associated errors, visible keyboard focus, logical tab order, and comfortable target sizes.
