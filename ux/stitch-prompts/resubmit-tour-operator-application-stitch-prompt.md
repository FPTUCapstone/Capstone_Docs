# Stitch Prompt: Resubmit Tour Operator Application

> Source documents:
> - Screen Specification: `ux/screen-specifications/resubmit-tour-operator-application-screen-spec.md`
> - User Flow: `ux/user-flows/resubmit-tour-operator-application-user-flow.md`
> - MVP Planning: `requirements/mvp/resubmit-tour-operator-application-mvp.md`

# Product Context

TripMate is a travel companion product. This screen is where a Tour Operator whose business
registration was rejected comes back, sees why, fixes it, and gets back into the review queue —
reusing the same identity and application, not starting over.

# Screen Objective

Show the Administrator's rejection reason clearly, present the previously submitted application
pre-filled and editable, and let the Tour Operator resubmit with one clear action.

# Target User

An authenticated Tour Operator whose latest application was rejected. Already familiar with the
form (they filled it once before); the goal now is quick correction, not re-learning the form.

# Screen Content

- Screen heading (e.g., "Your application needs changes")
- Rejection reason, shown prominently near the top (e.g., in a highlighted banner/callout)
- The application form, pre-filled with previously submitted values:
  1. Company Name
  2. Tax Code
  3. Business contact details
  4. Travel Business License — file upload (shows the previously uploaded file, replaceable)
- Primary action: **Resubmit** button

# Required Components

- Callout/banner component for the rejection reason — visually distinct from the form (e.g.,
  warning color), not just plain body text
- Pre-filled text inputs — Company Name, Tax Code, business contact details
- Pre-filled file upload control — Travel Business License, with an option to replace the file
- Inline field-level error message component
- Primary button — Resubmit (full-width, single dominant action)

# Required States

- **Rejection Review** — rejection reason shown, form pre-filled, no edits yet
- **Editing** — one or more fields/file changed from the pre-filled values
- **Validation error** — inline field-level error(s), Tour Operator stays on screen
- **Loading** — Resubmit button showing a subtle in-progress indicator (design suggestion)
- **Success** — resubmission succeeds; screen transitions away (destination not confirmed — see
  Open Questions)

# Navigation Context

- Entry: only after Sign In, when the account has a Rejected application (not designed here)
- On success → transitions away (destination undecided)
- On error → stays on this same screen

# UX Constraints

- Mobile-first, single-column layout
- The rejection reason must be the most visually prominent element on the screen — it's the
  reason the Tour Operator is here
- One clear primary action (Resubmit); no competing buttons
- Inline, field-level validation errors — not a single generic error banner
- Reuse the same visual style/components as the original Tour Operator Registration Form for
  consistency (same product, same form, just pre-filled)

# Open Questions

- Whether the rejection reason references specific fields (and should visually tie to them) or is
  just a general text explanation — design a general callout without inventing per-field tagging.
- Where the Tour Operator lands after a successful resubmission is unresolved — design only the
  transition-out moment, not a confirmed destination screen.

# Stitch Prompt

Design a mobile-first "rejected application resubmission" screen for a travel app called
TripMate, for a Tour Operator correcting a previously rejected business registration.

**Layout:** Single-column, vertically stacked, mobile viewport. Clear heading at the top (e.g.,
"Your application needs changes").

**Rejection reason:** A prominent callout/banner directly below the heading, visually distinct
(e.g., warning-colored background or left border), containing the Administrator's rejection
reason text.

**Form fields below the callout, pre-filled with existing values:**
1. Company Name — text input
2. Tax Code — text input
3. Business contact details — text input
4. Travel Business License — file upload control showing the currently attached file, with a
   clear "Replace file" affordance

Each field has a clear label above it. Below any field with an error, show a small inline error
message in a distinct error color.

**Primary action:** a single full-width "Resubmit" button below the form fields.

**States to show:**
- Default state — rejection callout + all fields pre-filled, no errors
- Error state — at least one field showing an inline validation error
- Loading state — Resubmit button showing a subtle in-progress indicator

**Style:** Clean and consistent with a standard business registration form — same visual language
as a typical TripMate form screen. The rejection callout should stand out without feeling
alarming or punitive. Generous spacing, accessible contrast and tap targets. No decorative or
marketing content.
