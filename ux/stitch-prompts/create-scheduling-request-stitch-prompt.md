# Stitch Prompt: Create Scheduling Request

> Source documents:
> - Screen Specification: `ux/screen-specifications/create-scheduling-request-screen-spec.md`
> - User Flow: `ux/user-flows/create-scheduling-request-user-flow.md`
> - MVP Planning: `requirements/mvp/create-scheduling-request-mvp.md`
>
> Covers both screens: the Request Form (including its inline "infeasible" state) and the
> Itinerary Result screen. Result presentation is a simple ordered list at MVP — no map, per the
> MVP scope's judgment call (source doesn't specify a presentation format at all).

# Product Context

TripMate is a travel companion product. This is the core value-delivery flow: a Traveler
describes a one-day trip and TripMate's CSP engine generates a personalized itinerary. It's the
most "product-defining" screen pair in this whole batch of use cases.

# Target User

Authenticated Traveler planning a one-day trip, possibly with saved preferences (UC-09) already
set.

---

## Screen 1: Create Scheduling Request Form

### Screen Objective

Collect the Traveler's constraints, submit them, show a clear waiting state, and handle an
infeasible result gracefully — without losing what the Traveler already entered.

### Screen Content

- Heading (e.g., "Plan your day")
- Form fields, in order:
  1. Available travel time
  2. Starting point
  3. Destination
  4. Mandatory locations (assume a repeatable/multi-entry input)
  5. Budget
- Note that saved preferences will be applied automatically, if any exist
- Primary action: **Generate Itinerary**

### Required Components

- Text/time input — Available travel time
- Text input — Starting point
- Text input — Destination
- Repeatable text input (add/remove entries) — Mandatory locations
- Numeric input — Budget
- Primary button — Generate Itinerary
- Inline field-level error message component
- Loading/generating indicator (distinct from a simple button spinner — this may take a moment)
- Inline "infeasible" message component (non-alarming tone, distinct from a hard error)

### Required States

- Input — empty or partially filled
- Validation error — inline field errors
- Loading/Generating — waiting on the CSP engine
- Infeasible — a calm, explanatory inline message, form values retained
- System error — generic error message

### Navigation Context

- Entry: from Traveler's main navigation
- On success → Itinerary Result screen (Screen 2)
- On infeasible or any error → stays on this same screen, values retained

---

## Screen 2: Itinerary Result Screen

### Screen Objective

Present the generated itinerary clearly as an ordered sequence of stops with times.

### Screen Content

- Heading (e.g., "Your itinerary")
- An ordered list of stops, each showing at least a time and a stop name
- Primary action: not confirmed — design a simple "Plan another trip" action as a reasonable
  default (see Open Questions)

### Required Components

- Ordered list/timeline component — one row per stop, each with a time and name
- Primary button — "Plan another trip" (UX suggestion, not confirmed)

### Required States

- Success — the only state for this screen (only reached after a feasible result)

### Navigation Context

- Entry: only from a successful generation on Screen 1
- Exit: back toward Screen 1 for a new request (suggestion, not confirmed)

---

# UX Constraints (both screens)

- Mobile-first, single-column layout
- Screen 1: one clear primary action; the "infeasible" state must read as normal/expected, not as
  an error (different visual treatment than a validation or system error)
- Screen 2: a simple, scannable timeline/list — this is the payoff moment of the whole product,
  keep it clean and readable, not cluttered
- Consistent spacing and accessible form controls throughout

# Open Questions

- Whether per-stop details beyond time + name (address, cost, duration) are shown on Screen 2.
- Confirmed presentation format — this brief assumes list-only for MVP; a map view is Should Have.
- Primary action(s) available on Screen 2 (not confirmed in source).
- Typical CSP generation time, affecting how much "weight" the loading state should carry visually.

# Stitch Prompt

Design two connected mobile-first screens for a travel app called TripMate: a one-day trip
planning request form, and its generated itinerary result.

## Screen 1 — Plan Your Day (Request Form)

**Layout:** Single-column, vertically stacked form on a mobile viewport. Heading at the top (e.g.,
"Plan your day"), with a short line noting that saved preferences will be applied automatically.

**Fields, in this order:**
1. Available travel time — text or time-range input
2. Starting point — text input
3. Destination — text input
4. Mandatory locations — a repeatable input allowing the Traveler to add multiple stops (e.g.,
   tag-style entries with an "add" affordance)
5. Budget — numeric input

Each field has a clear label above it. Below any field with an error, show a small inline error
message in a distinct error color.

**Primary action:** a single full-width "Generate Itinerary" button.

**States to show:**
- Default state — empty form
- Filled state — form with example values entered, including 2 mandatory-location tags
- Loading state — a generating/progress indicator replacing or overlaying the button area (e.g.,
  "Finding the best plan for you…")
- Infeasible state — a calm, non-error-styled inline message below the form (e.g., "We couldn't
  fit these into one day — try adjusting your time or locations"), with the form values still
  visible and editable

**Style:** Clean, friendly, trip-planning tone. Generous spacing. Accessible contrast and tap
targets. No unrelated marketing content.

## Screen 2 — Your Itinerary (Result)

**Layout:** Single-column, vertically stacked timeline/list on a mobile viewport. Heading at the
top (e.g., "Your itinerary").

**Content:** An ordered, timeline-style list of stops, each row showing a time (e.g., "9:00 AM")
and a stop name, connected visually as a sequence (e.g., a vertical line connecting rows).

**Primary action:** a "Plan another trip" button at the bottom, returning to Screen 1.

**Style:** Clean, scannable, satisfying — this is the payoff screen of the whole flow. Consistent
typography and spacing with Screen 1. No decorative content that distracts from the itinerary
itself.
