# Stitch Prompt: Sign In

> Source documents:
> - Screen Specification: `ux/screen-specifications/sign-in-screen-spec.md`
> - User Flow: `ux/user-flows/sign-in-user-flow.md`
> - MVP Planning: `requirements/mvp/sign-in-mvp.md`
>
> Scope note: MVP covers email/password only. Phone/OTP and Google Social Login are Should/Could
> Have and not designed in this brief.

# Product Context

TripMate is a travel companion product used by Travelers, Tour Operators, and Administrators.
This is the single Sign In screen shared by all three roles — the most foundational screen in the
product, since almost every other feature requires being signed in first.

# Screen Objective

Let an existing account holder authenticate with email and password, with clear, non-revealing
error feedback on failure, and move into the app once authenticated.

# Target User

Any existing TripMate account holder — Traveler, Tour Operator, or Administrator. Broad audience;
design for clarity and speed rather than any one role's specific context.

# Screen Content

- Screen title / heading (e.g., "Sign in to TripMate")
- Form fields, in order:
  1. Email address
  2. Password (masked)
- Primary action: **Sign In** button
- Optional secondary link: "Forgot password?" (into a separate flow, not designed here) — include
  as a UX suggestion, not a confirmed requirement

# Required Components

- Text input — Email address (email keyboard type)
- Password input — masked, with a show/hide toggle
- Primary button — Sign In (full-width, single dominant action)
- Form-level or field-level error message component
- Optional: secondary text link, "Forgot password?"

# Required States

- **Initial** — empty fields, no errors
- **Input** — actively typing
- **Error** — invalid credentials or restricted-account error shown; stays on this screen
- **Loading** — Sign In button showing a subtle in-progress indicator (design suggestion)
- **Success** — transitions away (destination not designed here)

# Navigation Context

- Entry: standalone destination (specific entry points not confirmed in source)
- On success → leaves this screen (role-specific landing not designed here)
- On error → remains on this same screen

# UX Constraints

- Mobile-first, single-column layout
- One clear primary action (Sign In); no competing buttons
- Error message must not reveal whether the email or the password was the incorrect part
- Password field masked by default
- Minimize cognitive load: only the two required inputs, no extra fields
- Consistent spacing and accessible form controls

# Open Questions

- Whether a "Forgot password?" link belongs on this screen (not confirmed) — design it as an
  optional, secondary, low-emphasis element if included.
- No confirmed wording for the restricted-account error — design a generic status-message style
  without inventing specific copy.

# Stitch Prompt

Design a mobile-first Sign In screen for a travel app called TripMate.

**Layout:** Single-column, vertically stacked form, centered on a mobile viewport. Clear screen
heading at the top (e.g., "Sign in to TripMate").

**Fields (in this order):**
1. Email address — text input
2. Password — masked text input with a show/hide toggle

Each field has a clear label above it. Show a form-level or field-level error message in a
distinct error color when sign-in fails (generic wording, e.g., "Incorrect email or password").

**Primary action:** a single full-width "Sign In" button below the form fields.

**Optional:** a small secondary text link below the button or near the password field, "Forgot
password?"

**States to show:**
- Default/empty state — no errors
- Error state — a clear, non-alarming error message shown
- Loading state — Sign In button showing a subtle in-progress indicator

**Style:** Clean, simple, minimal decoration. Generous spacing. Accessible contrast and tap target
sizes. No marketing content, illustrations, or unrelated UI beyond the form and heading.
