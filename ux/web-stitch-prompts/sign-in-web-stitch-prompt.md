# Product Context

TripMate uses separate public and administrator sign-in contexts within one consistent Web product. Public sign-in supports traveler and tour-operator entry. Administrator sign-in is an operational access screen and must not resemble a tourism marketing page.

# Target Platform

Responsive desktop Web application. Design both public and administrator variants at 1440 px, 1280 px, and 768 px with no horizontal scrolling.

# Target User

- Registered travelers.
- Registered tour operators, including accounts whose application status restricts access.
- TripMate administrators.

# Screen Objective

Let each supported user authenticate in the correct context, understand actionable errors or account restrictions, recover a password, and reach the correct post-sign-in destination.

# Connected Screens

- TripMate Landing Page.
- Traveler Registration.
- Tour Operator Registration.
- Public Password Recovery.
- Admin Password Recovery.
- Operator Application Status for pending or rejected operator accounts.
- Role-appropriate post-sign-in destinations.
- External Google authorization handoff from public sign-in only.

# Layout Structure

## Public Sign In

1. Shared public header or lightweight brand return path.
2. A two-region authentication composition: restrained Central Vietnam identity imagery and a constrained sign-in form.
3. Form actions, registration links, and recovery link.
4. Inline and form-level feedback areas.

## Admin Login

1. Restrained TripMate administrator identity area.
2. Focused operational login panel without tourism-marketing cards.
3. Recovery link and feedback areas.
4. No public registration or Google entry actions.

# Required Content

## Public Sign In

- Email Address.
- Password.
- Remember me.
- Sign In.
- Continue with Google.
- Forgot Password.
- Register Traveler.
- Tour Operator registration handoff.
- Return to the landing page.

## Admin Login

- Email Address.
- Password.
- Remember me.
- Sign In.
- Forgot Password.

# Required Components

- Shared TripMate authentication shell.
- Labeled email and password fields.
- Password visibility control with an accessible label.
- Remember-me checkbox.
- Primary Sign In button.
- Public-only Continue with Google handoff button.
- Text links for recovery, registration, and return navigation.
- Inline field validation, form-level alert, account-status alert, loading indicator, and success feedback treatment.

# Required States

- **Initial:** Empty public sign-in form and empty administrator login form with no validation messages.
- **Loading:** Prevent repeated submission while preserving the entered Email Address.
- **Loaded:** Each form is ready for input in its correct shell.
- **Validation Error:** Empty required field uses `This field is required.` Invalid email uses `Invalid email format. Please enter a valid email address (e.g., user@example.com).`
- **Business Error:** Invalid credentials use `Incorrect email or password. Please try again.` An authenticated Pending Approval tour operator receives `Your Tour Operator account is pending verification. You will be notified once approved.` An administratively locked account receives `Your account has been locked due to policy violations. Please contact support@tripmate.com.`
- **System Error:** `TripMate is temporarily unable to process your request. Please check your connection and try again.` No session is presented as created.
- **Success:** `Welcome back to TripMate! Signed in successfully.` then role- and status-appropriate navigation.
- **Disabled Action:** Disable Sign In only while submission is processing; no other persistent disabled rule is specified.
- **Unauthorized:** A valid non-administrator must not gain administrator access. Exact wrong-role copy remains a neutral placeholder.
- **Responsive:** Both authentication panels remain readable and keyboard accessible at all target widths.
- Neutral placeholder treatment for the approved temporary lock after five consecutive failed attempts for 15 minutes because exact user-facing copy is unavailable.
- Neutral placeholder treatment for a blocked Pending Verification account because exact approved copy is unavailable.
- Neutral placeholder treatment for an unlinked Google identity because the credential-error message match is ambiguous in this context.
- Neutral placeholder treatment for an administrator wrong-role response when exact approved copy is unavailable.

# Navigation Context

- Public Sign In uses the proposed `/sign-in` destination.
- Admin Login uses the proposed `/admin/login` destination.
- Public Forgot Password leads to the proposed `/forgot-password` destination.
- Admin Forgot Password leads to the proposed `/admin/forgot-password` destination.
- Public registration links lead to proposed `/register` and `/partner/register` destinations.
- Successful traveler sign-in may lead to the proposed `/traveler` destination.
- Successful approved-operator sign-in may lead to the proposed `/operator` destination.
- Pending or rejected operator access leads to the proposed `/partner/application` destination.
- Successful administrator sign-in may lead to the proposed `/admin` destination.
- Routes are design context, not implementation requirements.

# Responsive Behavior

- At 1440 px, use a centered two-region public layout and a focused, narrower admin panel.
- At 1280 px, preserve the public identity/form balance while reducing outer whitespace.
- At 768 px, stack or simplify the public identity area so the form remains first-class; keep the admin form centered and uncluttered.
- Keep fields, alerts, and buttons within the viewport with no horizontal scrolling.
- Error text wraps naturally and never causes controls to shift off-screen.

# Visual Direction

- Shared deep navy, teal, coral, and light-neutral design tokens across both variants.
- Public sign-in may use restrained professional Central Vietnam imagery and a subtle geographic motif.
- Admin login uses the same typography, field, button, and feedback system but is more operational and restrained.
- Coral is reserved for important emphasis or error support, not decoration.
- Avoid playful illustration and excessive marketing content.

# Accessibility

- Persistent visible labels for every input.
- Error messages are associated with their fields and are not indicated by color alone.
- Strong contrast, visible keyboard focus, logical tab order, and comfortable target sizes.
- Password visibility control has an accessible name and state.
- Loading and form-level feedback are perceivable without removing the user's entered context.

# UX Constraints

- Google authorization is an external handoff; design only the public Continue with Google trigger, never the provider-owned screen.
- Do not show Google sign-in in the administrator context.
- Preserve entered email when displaying recoverable errors where appropriate.
- Prevent repeat submissions during loading.
- Represent the approved temporary lock after five consecutive failed attempts for 15 minutes, but use neutral placeholder copy because no matching locked message exists.
- Use exact approved messages only in matching contexts; use neutral placeholder copy where the approved message is ambiguous or missing.
- Do not claim authentication, authorization, or session behavior is implemented.

# Things NOT to Design

- No Google provider authorization interface.
- No Google sign-in action on Admin Login.
- No administrator registration.
- No public registration actions inside the administrator form.
- No session-expiry screen.
- No dashboards or post-login application screens.
- No tourism marketing cards in the administrator shell.
- No mobile application screen.

# Open Questions

- Exact copy for temporary five-attempt lock behavior is not approved.
- Exact guidance for a blocked Pending Verification account is unresolved; the locked MSG10 text is reserved for the distinct Pending Approval tour-operator status.
- Exact copy for an unlinked Google identity is unresolved; the locked email/password credential message is not treated as mandatory Google-return copy.
- Exact administrator wrong-role copy is not approved.
- Whether Remember me changes any visible expiry information is unresolved.
- Whether Tour Operator registration appears directly in the public form or only in public navigation remains a placement decision; the entry must remain reachable.
- Google authentication for administrators is not approved and therefore remains excluded.
- Final post-sign-in routes and production authentication behavior remain implementation decisions.

# Stitch Prompt

Design a responsive desktop Web application, not a mobile app.

Create two connected TripMate authentication screens within one coherent Web design system: Public Sign In and Admin Login. Produce responsive layouts at 1440 px, 1280 px, and 768 px with no horizontal scrolling.

Use deep navy for structural areas, teal for trustworthy interactive accents, limited coral for important emphasis and error support, and light-neutral form surfaces. Typography, inputs, buttons, alerts, spacing, and focus states must be shared across both contexts. The public variant may include restrained professional Central Vietnam photography or a subtle geographic motif. The administrator variant must feel operational, calm, and focused, with no tourism marketing cards.

Public Sign In:

- Create a balanced authentication shell with a restrained travel-identity region and a constrained form panel.
- Include labeled Email Address and Password fields, an accessible password-visibility control, Remember me, a primary Sign In button, and a public-only Continue with Google handoff button.
- Include Forgot Password, Register Traveler, Tour Operator registration, and return-to-landing links.
- Continue with Google launches an external handoff; do not design the Google provider screen.

Admin Login:

- Create a focused administrator login panel using the same form components and visual tokens.
- Include labeled Email Address and Password fields, password visibility, Remember me, Sign In, and Forgot Password.
- Do not include Google sign-in, registration, public promotional cards, or traveler imagery that distracts from operational access.

Show coherent examples of these states across the two screens: default, inline field validation, invalid credentials, locked account, an authenticated Pending Approval tour-operator status notice on the public screen, a separately blocked Pending Verification placeholder state, an unlinked-Google return placeholder state, form-level service failure, loading with repeat submission prevented, and successful sign-in. Use these exact approved messages only in their matching state:

- `This field is required.`
- `Invalid email format. Please enter a valid email address (e.g., user@example.com).`
- `Incorrect email or password. Please try again.`
- `Your Tour Operator account is pending verification. You will be notified once approved.`
- `Your account has been locked due to policy violations. Please contact support@tripmate.com.`
- `Welcome back to TripMate! Signed in successfully.`
- `TripMate is temporarily unable to process your request. Please check your connection and try again.`

Where exact copy is not approved—the 15-minute temporary lock after five consecutive failed attempts, the separately blocked Pending Verification account, an unlinked Google identity, or administrator wrong-role response—use a visibly neutral placeholder such as “Message to be confirmed” instead of inventing message text. Do not reuse the Pending Approval tour-operator message for the blocked Pending Verification state, and do not make the email/password credential error mandatory for the Google-return state.

Represent the proposed navigation relationships without exposing technical paths as page copy: public sign-in `/sign-in`, admin login `/admin/login`, public recovery `/forgot-password`, admin recovery `/admin/forgot-password`, traveler registration `/register`, operator registration `/partner/register`, operator application status `/partner/application`, traveler destination `/traveler`, operator destination `/operator`, and admin destination `/admin`.

At 1440 px use a centered two-region public composition and a focused narrower admin panel. At 1280 px reduce outer spacing while preserving hierarchy. At 768 px stack or simplify the public identity region and keep both forms fully readable. Ensure persistent labels, associated error text, visible keyboard focus, strong contrast, logical tab order, comfortable targets, naturally wrapping alerts, and no clipped fields or horizontal scrolling.
