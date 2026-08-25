# Stitch Prompt: Change Password

> Source documents:
> - Screen Specification: `ux/screen-specifications/change-password-screen-spec.md`
> - User Flow: `ux/user-flows/change-password-user-flow.md`
> - MVP Planning: `requirements/mvp/change-password-mvp.md`

# Product Context

TripMate is a travel companion product. This is an account-settings screen used by any
authenticated user (Traveler, Tour Operator, or Administrator) to proactively update their
password.

# Screen Objective

Let the user prove identity with their current password and set a new one that meets the account
password policy, with clear inline error feedback.

# Target User

Any authenticated TripMate user, any role. Assume they're already inside the app, familiar with
its UI — this is a settings sub-screen, not a first-touch experience.

# Screen Content

- Screen heading (e.g., "Change Password")
- Form fields, in order:
  1. Current password (masked)
  2. New password (masked)
- Primary action: **Change Password** button

# Required Components

- Password input — Current password, masked, with show/hide toggle
- Password input — New password, masked, with show/hide toggle
- Primary button — Change Password (full-width or settings-appropriate width)
- Inline field-level error message component

# Required States

- **Initial** — empty fields, no errors
- **Error** — current password incorrect, or new password fails policy (inline, per field)
- **Loading** — button showing a subtle in-progress indicator (design suggestion)
- **Success** — confirmation message shown

# Navigation Context

- Entry: from Account Settings
- On success → confirmation, then back toward Account Settings (assumption)
- On error → stays on this same screen

# UX Constraints

- Consistent with the visual style of Sign In / Registration / Reset Password screens (same
  password field treatment, same policy-hint styling)
- One clear primary action; no competing buttons
- Inline, field-level validation errors
- This is a settings sub-screen — it can be laid out within a settings page context rather than
  as a full standalone screen, if that fits the product's settings pattern better (design
  latitude, not a stated requirement)

# Open Questions

- Whether re-authentication is required after a successful change (doesn't affect this screen's
  visual design, only what happens after).

# Stitch Prompt

Design a mobile-first "Change Password" screen for a travel app called TripMate, as part of the
account settings area.

**Layout:** Single-column form, either a standalone screen or a settings sub-page, with a clear
heading (e.g., "Change Password").

**Fields (in this order):**
1. Current password — masked text input with a show/hide toggle
2. New password — masked text input with a show/hide toggle

Each field has a clear label above it. Below any field with an error, show a small inline error
message in a distinct error color (e.g., "Current password is incorrect" or a password-policy
error under the new password field).

**Primary action:** a single full-width "Change Password" button below the form fields.

**States to show:**
- Default/empty state — no errors
- Error state — an inline error under the relevant field
- Loading state — button showing a subtle in-progress indicator

**Style:** Clean, minimal, consistent with the rest of TripMate's account/settings and
authentication screens. Generous spacing, accessible contrast and tap targets. No marketing or
decorative content.
