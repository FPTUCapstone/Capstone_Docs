# Stitch Prompt: Update Traveler Profile

> Source documents:
> - Screen Specification: `ux/screen-specifications/update-traveler-profile-screen-spec.md`
> - User Flow: `ux/user-flows/update-traveler-profile-user-flow.md`
> - MVP Planning: `requirements/mvp/update-traveler-profile-mvp.md`
>
> Scope note: MVP covers full name and phone number only. Avatar upload and email change are
> Should/Could Have and not designed here.

# Product Context

TripMate is a travel companion product. This is a Traveler's profile settings screen — a
straightforward "edit and save" form for keeping personal info current.

# Screen Objective

Show the Traveler's current name and phone number, let them edit and save, with clear inline
error feedback and a confirmation on success.

# Target User

Authenticated Traveler, already inside the app. This is a settings screen, not a first-touch
experience.

# Screen Content

- Screen heading (e.g., "Profile")
- Form fields, pre-filled with current values:
  1. Full name
  2. Phone number
- Primary action: **Save** button

# Required Components

- Text input — Full name, pre-filled
- Text input — Phone number, pre-filled (numeric/phone keyboard type)
- Primary button — Save
- Inline field-level error message component
- Success/confirmation indicator (e.g., a toast or inline message)

# Required States

- **View/Input** — current values shown, editable
- **Error** — inline field-level error (invalid format, or phone already in use)
- **Loading** — Save button showing a subtle in-progress indicator
- **Success** — confirmation shown, values reflect the update

# Navigation Context

- Entry: from account/settings navigation
- Stays on this same screen through view, edit, save, and confirmation

# UX Constraints

- Mobile-first, single-column layout
- One clear primary action (Save)
- Inline, field-level validation errors
- Fields pre-filled with existing data, not empty (this is an edit screen, not a fresh form)
- Consistent spacing and accessible form controls

# Open Questions

- Whether avatar and email fields will later join this same screen (affects future layout, not
  this MVP version).

# Stitch Prompt

Design a mobile-first "Profile" settings screen for a travel app called TripMate, for a Traveler
to view and edit their basic info.

**Layout:** Single-column form, pre-filled with the Traveler's current data. Clear heading at the
top (e.g., "Profile").

**Fields (in this order), both pre-filled:**
1. Full name — text input
2. Phone number — text input

Each field has a clear label above it. Below any field with an error, show a small inline error
message in a distinct error color.

**Primary action:** a single full-width "Save" button below the form fields.

**States to show:**
- Default state — fields pre-filled with sample data, no errors
- Error state — an inline error under one field (e.g., "This phone number is already in use")
- Success state — a small confirmation indicator (e.g., a toast: "Profile updated")

**Style:** Clean, simple, consistent with the rest of TripMate's account/settings screens.
Generous spacing, accessible contrast and tap target sizes. No decorative content.
