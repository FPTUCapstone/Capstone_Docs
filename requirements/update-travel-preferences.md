# Overview

Allows a Traveler to configure preferences that may be used as soft constraints during
personalized itinerary generation. Supported preferences may include preferred transportation
mode, travel pace, food preferences, interests, and risk tolerance. Successfully saved preferences
become available to future scheduling requests and CSP (Constraint Satisfaction Problem)
processing.

# Actors

- **Traveler** (primary) — an authenticated user with the Traveler role.
- **System (TripMate Platform)** — validates and saves preferences; makes them available to the
  scheduling/CSP engine.

# Preconditions

- The Traveler is authenticated (UC-04 Sign In). *(Explicit)*

# Main Flow

1. Traveler navigates to Travel Preferences settings. *(Explicit)*
2. System displays current preference values (if any were previously set). *(Explicit)*
3. Traveler configures preferences: transportation mode, travel pace, food preferences, interests,
   risk tolerance. *(Explicit)*
4. Traveler saves. *(Explicit)*
5. System validates the submitted preferences. *(Explicit)*
6. System saves the preferences to the Traveler's profile. *(Explicit)*
7. Saved preferences become available for future scheduling requests (UC-10) and CSP processing.
   *(Explicit)*

# Alternative Flows

- **AF1 — Traveler has no preferences set yet (first time):** System shows default/empty state
  rather than pre-filled values. *(Assumption)*
- **AF2 — Traveler updates only a subset of preferences:** Explicit — the source names multiple
  independent preference categories, implying partial updates are allowed.

# Exception Flows

- **EF1 — Invalid preference value:** System rejects the specific field (e.g., an
  out-of-range value) and shows an error. *(Assumption — exact valid ranges/options per
  preference are not specified)*
- **EF2 — System/save failure:** System informs the Traveler and does not partially save the
  update. *(Assumption)*

# Postconditions

**Success:**
- The Traveler's saved preferences reflect the updated values.
- These preferences are available to future scheduling requests and CSP processing.

**Failure:**
- Preferences remain unchanged from their last saved state.
- The Traveler is informed of the specific validation failure.

# Functional Requirements

- FR1: The system shall display the Traveler's current preferences for editing.
- FR2: The system shall allow configuring preferred transportation mode, travel pace, food
  preferences, interests, and risk tolerance.
- FR3: The system shall validate submitted preference values before saving.
- FR4: The system shall persist saved preferences and make them available to scheduling requests
  and CSP processing.
- FR5: The system shall confirm a successful update.

# Business Rules

- BR1: Preferences are described as **soft constraints** — they influence but do not strictly
  determine CSP-generated itineraries. *(Explicit)*
- BR2 *(assumption)*: Preferences are optional; a Traveler with no preferences set can still use
  UC-10 Create Scheduling Request, just without preference-based influence.
- BR3 *(assumption)*: The exact value domain for each preference (e.g., what options exist for
  "travel pace" or "risk tolerance") is not specified — do not invent a fixed enum without
  confirmation.

# Edge Cases

- Traveler sets preferences that are internally contradictory (e.g., very low risk tolerance +
  very fast pace) — no conflict detection defined; likely acceptable since these are soft
  constraints, not hard rules.
- Traveler saves an empty/cleared preference set after previously having values — does this reset
  to "no preference" (excluded from CSP) or fail validation? Undefined.
- Very large "interests" list (if free-form/multi-select) — no limit defined.

# Missing Information

- What are the exact selectable options/value domain for each preference (transportation mode,
  travel pace, food preferences, interests, risk tolerance)? Enum values, free text, or scale
  (e.g., 1–5)?
- Can a Traveler clear a previously set preference back to "no preference"?
- Is there a limit on how many "interests" or "food preferences" can be selected?
- How exactly do these soft constraints weight against hard constraints in CSP processing (an
  implementation detail of UC-10, but affects whether preferences need per-item priority/weight
  input here)?

# MVP Scope

**Suggestion** *(not explicit in source — for discussion)*:
- A small, fixed set of preference categories with simple selectable options (e.g., single-select
  transportation mode, single-select pace, multi-select food preferences/interests, single-select
  risk tolerance) — exact options still need product/business confirmation before UI can be
  finalized.
