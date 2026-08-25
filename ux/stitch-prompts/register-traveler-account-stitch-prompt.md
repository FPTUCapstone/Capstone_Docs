# Stitch Prompt: Register Traveler Account

> Source documents:
> - Screen Specification: `ux/screen-specifications/register-traveler-account-screen-spec.md`
> - User Flow: `ux/user-flows/register-traveler-account-user-flow.md`
> - MVP Planning: `requirements/mvp/register-traveler-account.md`
>
> Scope note: This brief covers the **Registration Screen** only. The Login screen is referenced solely as a navigation destination (see Navigation Context) — it is not designed here, because the approved screen spec explicitly marks Login as a minimal, unconfirmed stub (no independent Login use case has been analyzed).

---

# Product Context

TripMate is a travel companion product. This screen is the self-service entry point that lets an unauthenticated Guest create a TripMate account and become a Traveler. It is the first screen most new users will ever see.

# Screen Objective

Let the Guest submit full name, email, phone number, and password in one form, get clear inline feedback on any error, and on success be handed off to the Login screen to sign in manually. No auto-login, no confirmation screen — success is a redirect.

# Target User

Guest — an unauthenticated, first-time visitor with no prior context on the product. Assume mobile usage, average patience for forms, and no technical background.

# Screen Content

- Screen title / heading (e.g., "Create your account")
- Form fields, in order:
  1. Full name
  2. Email address
  3. Phone number
  4. Password (masked)
- Terms of Service / Privacy Policy text below the Register button (e.g., "By registering, you agree to the Terms & Privacy Policy"), with "Terms & Privacy Policy" as a tappable link — **no checkbox**
- Primary action: **Register** button
- Optional secondary link to Login screen for existing users (e.g., "Already have an account? Log in") — include as a UX suggestion, not a confirmed requirement

# Required Components

- Text input — Full name
- Text input — Email address (email keyboard type)
- Text input — Phone number (numeric/phone keyboard type)
- Password input — masked, with a show/hide toggle
- Primary button — Register (full-width, clearly the single dominant action)
- Inline field-level error message component (appears directly beneath its field)
- Static text + inline link — Terms/Privacy annotation, positioned below the Register button
- Optional: secondary text link to Login

# Required States

- **Initial** — all fields empty, no errors, Register button enabled
- **Input** — Guest actively typing/filling fields
- **Validation error** — one or more fields show an inline error (e.g., "Password is not 8 characters long"); Guest stays on this screen
- **Duplicate account error** — inline error on the conflicting field (e.g., "Email already exists"); Guest stays on this screen
- **Loading** — Register button shows a loading/submitting indicator while the request is in flight (design suggestion, not a confirmed requirement)
- **Success** — form submission succeeds; screen transitions away (redirect to Login) — no confirmation state to design, show only the transition-out moment if illustrating success at all

# Navigation Context

- Entry: Guest arrives at this screen unauthenticated (exact entry path — e.g. a "Sign Up" link — is not confirmed in source; design the screen as a standalone destination)
- On successful submission → redirects to the Login screen (not part of this brief)
- On validation or duplicate error → remains on this same screen with inline errors
- Optional secondary link → Login screen (not part of this brief)

# UX Constraints

- Mobile-first single-column layout
- One clear primary action (Register); no competing buttons
- Inline, field-level validation errors — not a single generic error banner
- No consent checkbox — Terms/Privacy is text + link only
- Password field must be masked by default
- Minimize cognitive load: no unnecessary decorative elements, no extra fields beyond the four required inputs
- Consistent spacing and accessible form controls (adequate tap targets, labeled inputs, visible focus/error states)

# Open Questions

- Whether field values are retained after a validation/duplicate error is unresolved in source — design should reasonably assume retention of non-sensitive fields (name, email, phone), but this is not a confirmed requirement.
- No confirmed phone number format/mask — design a plain text input without inventing a specific format rule.
- No confirmed entry path into this screen (e.g., a "Sign Up" link) — design it as a self-contained screen.

---

# Stitch Prompt

Design a mobile-first account registration screen for a travel app called TripMate.

**Layout:** Single-column, vertically stacked form, centered on a mobile viewport. Clear screen heading at the top (e.g., "Create your account").

**Fields (in this order):**
1. Full name — text input
2. Email address — text input
3. Phone number — text input
4. Password — masked text input with a show/hide toggle

Each field has a clear label above it. Below any field with an error, show a small inline error message in a distinct error color (e.g., "Password is not 8 characters long" or "Email already exists").

**Primary action:** A single full-width "Register" button below the form fields, styled as the clear dominant call to action on the screen.

**Below the Register button:** small, non-bold text: "By registering, you agree to the Terms & Privacy Policy" — with "Terms & Privacy Policy" shown as a tappable link/inline text link, not a button.

**Optional:** a small secondary text link below that, "Already have an account? Log in."

**States to show:**
- Default/empty state — all fields empty, no errors
- Error state — at least one field showing an inline validation error beneath it, and/or a duplicate-account error beneath the email field
- Loading state — Register button showing a subtle in-progress indicator (e.g., spinner replacing button label)

**Style:** Clean, simple, minimal decoration. Generous spacing between fields. Accessible contrast and tap target sizes. No consent checkbox. No extra marketing content, illustrations, or unrelated UI beyond the form, heading, and Terms/Privacy text.
