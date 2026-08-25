# Stitch Prompt: Sign Out

> Source documents:
> - Screen Specification: `ux/screen-specifications/sign-out-screen-spec.md`
> - User Flow: `ux/user-flows/sign-out-user-flow.md`
> - MVP Planning: `requirements/mvp/sign-out-mvp.md`
>
> Scope note: no dedicated screen is required for the Must Have MVP scope — Sign Out is a menu
> action that immediately clears the session and redirects to Sign In. This brief covers only the
> **optional** confirmation dialog, which is a Could Have, not a confirmed requirement. If it's
> not built, there is nothing to generate in Stitch for this use case.

# Product Context

TripMate is a travel companion product. Sign Out is a low-risk, easily-reversible action
available to any authenticated user (Traveler, Tour Operator, Administrator) from the app's
account/menu area.

# Screen Objective

If built: give the user a brief chance to confirm or cancel before ending their session.

# Target User

Any authenticated user, any role.

# Screen Content

- Short confirmation prompt (e.g., "Sign out of TripMate?")
- Two actions: Confirm (Sign Out) and Cancel

# Required Components

- Modal/dialog container
- Prompt text
- Primary button — Sign Out (confirm)
- Secondary button — Cancel

# Required States

- **Prompt** — the only state for this optional dialog.

# Navigation Context

- Confirm → Sign In screen (session cleared)
- Cancel → dismiss, user remains on their current screen, still authenticated

# UX Constraints

- Should be lightweight — a small modal/dialog, not a full screen
- Two clearly distinguishable actions (avoid making "Sign Out" look identical in weight to
  "Cancel"; Cancel should be the visually lower-emphasis option since it's the safer default)

# Open Questions

- Whether this dialog is built at all for MVP is unresolved — the MVP scope suggests skipping it
  in favor of an immediate sign-out.

# Stitch Prompt

Design a small mobile-first confirmation dialog for a travel app called TripMate, shown when a
user taps "Sign Out".

**Layout:** A compact modal/dialog centered on the screen, over a dimmed background.

**Content:** A short prompt (e.g., "Sign out of TripMate?"), no icon or illustration needed.

**Actions:** Two buttons side by side or stacked — a lower-emphasis "Cancel" button and a clearly
distinguishable "Sign Out" button as the confirming action.

**Style:** Minimal, calm, no decorative elements. Keep it visually consistent with the rest of the
TripMate app's form/dialog components.
