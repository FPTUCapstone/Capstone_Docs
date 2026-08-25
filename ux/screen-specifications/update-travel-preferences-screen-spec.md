# Screen Inventory

1. Travel Preferences Screen — configure the five preference categories.

Only 1 screen is needed for the Must Have MVP journey.

> **Important caveat:** the specific input controls below (single-select vs. multi-select) and
> their option lists are built on the **placeholder value domains** from
> `requirements/mvp/update-travel-preferences-mvp.md`, which are explicitly unconfirmed. Treat
> the control types as reasonable placeholders, not settled requirements.

---

# Screen: Travel Preferences Screen

## Purpose

Let a Traveler configure preferences used as soft constraints for personalized itinerary
generation.

## User

Authenticated Traveler.

## Entry Conditions

Traveler is authenticated and navigates to Travel Preferences.

## Required Components

### Inputs

- Transportation mode — single-select. *(placeholder options — see caveat above)*
- Travel pace — single-select. *(placeholder options)*
- Food preferences — multi-select. *(placeholder options)*
- Interests — multi-select. *(placeholder options)*
- Risk tolerance — single-select. *(placeholder options)*

### Information Display

- Current saved values (or an empty/default state for a first-time Traveler).
- Brief explanatory copy that these are soft preferences, not hard requirements, for future
  itinerary generation — not stated in source as required copy, but reduces ambiguity about what
  this screen does.

### Primary Actions

- Save.

### Secondary Actions

None defined.

## Validation

Each category's selected value(s) checked against its value domain (placeholder — unconfirmed).

## Error Handling

- **E1 — Invalid preference value:** inline field-level error.
- **E2 — System/save failure:** generic error; preferences unchanged.

## States

- **Empty / Default** — first-time Traveler, nothing set yet.
- **View / Input** — current values shown (or defaults), editable.
- **Validation Error** — E1.
- **Loading** — saving.
- **Success** — confirmation shown.

## Navigation

- Stays on this screen throughout (view, edit, save, confirmation all happen here).

## Business Rules

BR1, BR2, BR3 (see [requirements/update-travel-preferences.md](../../requirements/update-travel-preferences.md)).

## Open Questions

- Confirmed selectable options for each preference category — **blocks finalizing this screen's
  exact controls**.
- Can a Traveler clear a category back to "no preference"?
- Is there a limit on multi-select categories?

## UX Suggestions

- Group the five categories under clear section labels rather than one long flat list, since
  they cover distinct dimensions (transport, pace, food, interests, risk) — not a confirmed
  requirement, purely a scannability suggestion.
- Use chip/tag-style multi-select for food preferences and interests (common pattern for this
  kind of input) — suggestion only.
