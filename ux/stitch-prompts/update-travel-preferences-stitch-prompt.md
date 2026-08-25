# Stitch Prompt: Update Travel Preferences

> Source documents:
> - Screen Specification: `ux/screen-specifications/update-travel-preferences-screen-spec.md`
> - User Flow: `ux/user-flows/update-travel-preferences-user-flow.md`
> - MVP Planning: `requirements/mvp/update-travel-preferences-mvp.md`
>
> **Caveat carried through from the screen spec:** the specific options listed for each
> preference category are placeholders (unconfirmed), included only so Stitch has concrete
> content to render. Treat the category structure (5 sections, select-style inputs) as the
> more durable part of this brief; the exact option lists should be revisited once confirmed.

# Product Context

TripMate is a travel companion product that generates personalized one-day itineraries. This
screen lets a Traveler set preferences that softly influence future itinerary generation — it's a
personalization/settings screen, not a form with pass/fail validation stakes.

# Screen Objective

Let the Traveler set values across five preference categories and save them, with a clear sense
that these are optional, soft preferences rather than mandatory requirements.

# Target User

Authenticated Traveler, likely setting this up before or around their first use of itinerary
generation (UC-10).

# Screen Content

- Screen heading (e.g., "Travel Preferences")
- Short explanatory line (e.g., "These help us personalize your itineraries — all optional")
- Five sections, each with its own label and selection control:
  1. **Transportation mode** (single-select): Walking, Public Transit, Car, Any
  2. **Travel pace** (single-select): Relaxed, Moderate, Packed
  3. **Food preferences** (multi-select): Vegetarian, Vegan, Halal, No restriction
  4. **Interests** (multi-select): Nature, History, Food & Drink, Shopping, Nightlife
  5. **Risk tolerance** (single-select): Low, Medium, High
- Primary action: **Save** button

# Required Components

- Section labels for each of the 5 categories
- Single-select control (e.g., segmented control or radio group) — transportation mode, pace,
  risk tolerance
- Multi-select control (e.g., chip/tag selection) — food preferences, interests
- Primary button — Save
- Inline field-level error message component (if a selection becomes invalid)

# Required States

- **Empty/Default** — first-time Traveler, nothing selected yet
- **View/Input** — values shown (or defaults), editable
- **Error** — inline error on a specific category
- **Loading** — Save button showing a subtle in-progress indicator
- **Success** — confirmation shown

# Navigation Context

- Entry: from account/settings navigation
- Stays on this same screen through view, edit, save, and confirmation

# UX Constraints

- Mobile-first, single-column layout, but with clear visual grouping/sectioning (5 distinct
  categories, not one flat list)
- Chip/tag style suits the two multi-select categories (food preferences, interests); a
  segmented control or radio group suits the three single-select categories
- Communicate optionality — no category should look mandatory or produce a blocking error for
  being empty
- Consistent spacing and accessible tap targets, especially for chip-style multi-select

# Open Questions

- Confirmed selectable options per category (the ones listed here are placeholders).
- Whether a Traveler can explicitly clear a category back to "no preference".
- Any limit on how many chips can be selected in the multi-select categories.

# Stitch Prompt

Design a mobile-first "Travel Preferences" settings screen for a travel app called TripMate,
where a Traveler configures personalization preferences.

**Layout:** Single-column, vertically stacked, mobile viewport. Heading at the top (e.g., "Travel
Preferences") with a short supporting line noting these are optional and help personalize future
itineraries. Organize the content into 5 clearly labeled sections, in this order:

1. **Transportation mode** — a segmented control or radio group with options: Walking, Public
   Transit, Car, Any
2. **Travel pace** — a segmented control or radio group with options: Relaxed, Moderate, Packed
3. **Food preferences** — a multi-select chip/tag group with options: Vegetarian, Vegan, Halal,
   No restriction
4. **Interests** — a multi-select chip/tag group with options: Nature, History, Food & Drink,
   Shopping, Nightlife
5. **Risk tolerance** — a segmented control or radio group with options: Low, Medium, High

**Primary action:** a single full-width "Save" button at the bottom of the screen.

**States to show:**
- Default state — nothing selected in any category (first-time Traveler)
- Filled state — one option selected per single-select category, a few chips selected per
  multi-select category
- Loading state — Save button showing a subtle in-progress indicator

**Style:** Clean, friendly, personalization-focused rather than form-validation-focused — selected
chips/options should feel satisfying to pick (clear selected-state styling), not clinical.
Generous spacing between sections. Accessible contrast and tap target sizes. No decorative
illustrations beyond simple section styling.
