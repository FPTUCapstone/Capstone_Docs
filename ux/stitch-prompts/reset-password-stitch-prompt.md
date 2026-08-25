# Stitch Prompt: Reset Password

> Source documents:
> - Screen Specification: `ux/screen-specifications/reset-password-screen-spec.md`
> - User Flow: `ux/user-flows/reset-password-user-flow.md`
> - MVP Planning: `requirements/mvp/reset-password-mvp.md`
>
> Covers both screens: Request Reset and Set New Password.

# Product Context

TripMate is a travel companion product. This is the self-service recovery path for a Guest who
forgot their password — a short, low-decoration flow whose whole purpose is to get the person
back into their account with minimal friction.

# Target User

Guest who cannot sign in due to a forgotten password. Likely mildly frustrated; design for speed
and clarity over anything decorative.

---

## Screen 1: Request Reset

### Screen Objective

Let the Guest submit their registered email and receive a generic confirmation, without revealing
whether that email actually matches an account.

### Screen Content

- Heading (e.g., "Reset your password")
- Email address input
- Primary action: **Send Reset Link**
- After submission: a generic confirmation message (e.g., "If an account exists for this email,
  we've sent a reset link")

### Required Components

- Text input — Email address
- Primary button — Send Reset Link
- Confirmation message component (post-submission)

### Required States

- Default/empty — no errors
- Validation error — invalid email format
- Loading — submitting
- Confirmation — generic "check your email" message shown

### Navigation Context

- Entry: from Sign In ("Forgot password?")
- On submit → generic confirmation, same screen
- Exit: external (Guest checks their email)

---

## Screen 2: Set New Password

### Screen Objective

Let the Guest, having followed a valid reset link, set a new password that meets the account
password policy.

### Screen Content

- Heading (e.g., "Set a new password")
- New password input (masked)
- Primary action: **Reset Password**

### Required Components

- Password input — masked, with show/hide toggle
- Primary button — Reset Password
- Inline field-level error message component
- Link-error state message (invalid/expired link)

### Required States

- Input — empty field
- Validation error — password doesn't meet policy
- Link error — invalid/expired reset link
- Loading — submitting
- Success — confirmation, then hands off to Sign In (not designed here)

### Navigation Context

- Entry: only via a valid emailed reset link
- On success → transitions toward Sign In (not designed in this brief)
- On link error → cannot proceed on this screen

---

# UX Constraints (both screens)

- Mobile-first, single-column layout
- One clear primary action per screen; no competing buttons
- Inline, field-level validation errors — not a generic banner
- Minimal, calm tone — no marketing/decorative content; this is a recovery flow
- Password field masked by default, consistent styling with the Sign In and Registration screens

# Open Questions

- Whether a "Confirm new password" second field is included (UX suggestion, not confirmed).
- Exact reset link expiry time (affects link-error messaging, not the visual design itself).

# Stitch Prompt

Design two connected mobile-first screens for a travel app called TripMate: a password-reset
request screen and a set-new-password screen.

## Screen 1 — Request Reset

**Layout:** Single-column, centered form on a mobile viewport. Heading at the top (e.g., "Reset
your password"), with a short supporting line (e.g., "Enter your email and we'll send you a
reset link").

**Field:** Email address — text input with a clear label.

**Primary action:** a single full-width "Send Reset Link" button.

**States to show:**
- Default state — empty field
- Error state — invalid email format, inline error below the field
- Confirmation state — the form replaced by (or overlaid with) a generic message: "If an account
  exists for this email, we've sent a reset link. Please check your inbox."

**Style:** Minimal, calm, no decorative elements.

## Screen 2 — Set New Password

**Layout:** Single-column, centered form on a mobile viewport. Heading at the top (e.g., "Set a
new password").

**Field:** New password — masked text input with a show/hide toggle and a clear label.

**Primary action:** a single full-width "Reset Password" button.

**States to show:**
- Default state — empty field
- Error state — inline error below the field (e.g., password policy not met)
- Link-error state — a message replacing the form (e.g., "This reset link has expired or is
  invalid")

**Style:** Visually consistent with Screen 1 and with the Sign In screen — same form language,
spacing, and typography. No marketing content or illustrations.
