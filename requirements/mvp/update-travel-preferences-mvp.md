# MVP Goal

Let a Traveler set a small set of travel preferences that feed into future itinerary generation
as soft constraints, without needing to define every possible dimension of personalization on
day one.

# Target User

Authenticated Traveler, especially one about to use UC-10 Create Scheduling Request for the first
time.

# Core User Problem

Personalized itinerary generation (UC-10) is meaningfully better with some signal about what the
Traveler likes; without a preferences screen, CSP has nothing to work with beyond hard
constraints.

# Core User Journey

Open Travel Preferences → select values across a small set of categories → save → preferences are
available the next time a scheduling request runs.

# Must Have

- Editable preference categories: transportation mode, travel pace, food preferences, interests,
  risk tolerance — all five named in source. *(FR2)*
- Validation before saving. *(FR3)*
- Persisted and available to UC-10 / CSP processing. *(FR4)*
- Confirmation on save. *(FR5)*
- Treated as optional / soft constraints — a Traveler with nothing set can still use UC-10.
  *(BR1, BR2)*

**Placeholder value domains** *(assumption — needs product confirmation before build; included
only so downstream screens/Stitch have something concrete to render)*:
- Transportation mode — single-select (e.g., Walking, Public Transit, Car, Any)
- Travel pace — single-select (e.g., Relaxed, Moderate, Packed)
- Food preferences — multi-select (e.g., Vegetarian, Vegan, Halal, No restriction)
- Interests — multi-select (e.g., Nature, History, Food & Drink, Shopping, Nightlife)
- Risk tolerance — single-select (e.g., Low, Medium, High)

# Should Have

- A visible "no preference" / clear option per category, so a Traveler can explicitly opt out of
  a category rather than leaving it ambiguous.

# Could Have

- Per-category priority/weighting (e.g., "food preferences matter more to me than pace") — not
  named in source at all; would only make sense once CSP weighting behavior is defined.

# Out of Scope

- Any preference dimension not named in source (transportation mode, pace, food, interests, risk
  tolerance are the only five given).
- Conflict detection between contradictory preferences (e.g., low risk tolerance + packed pace) —
  these are soft constraints, not hard rules, so no validation conflict is expected.

# MVP User Journey

1. Traveler opens Travel Preferences.
2. System shows current values (or empty/default state if none set yet).
3. Traveler selects values across the five categories.
4. Traveler saves.
5. System validates and persists.
6. Confirmation shown; preferences now available to future scheduling requests.

# Dependencies

- Depends on UC-04 Sign In.
- UC-10 Create Scheduling Request depends on this use case for its soft-constraint inputs (but
  UC-10 must degrade gracefully when no preferences are set, per BR2).

# Risks

- The value domain for every preference category is currently a placeholder assumption, not a
  confirmed requirement. If the real options differ substantially, the screen spec and Stitch
  prompt built from this MVP doc will need rework — flagged loudly here rather than silently
  building on an invented enum as if it were confirmed.

# Open Questions

- Confirmed selectable options for each of the five preference categories.
- Can a Traveler clear a previously set preference back to "no preference"?
- Is there a limit on multi-select categories (food preferences, interests)?
- How do these soft constraints weight against hard constraints in CSP (affects whether a
  priority/weight UI is ever needed)?
