# Overview

Allows a Traveler to request generation of a personalized one-day itinerary by providing planning
constraints such as available travel time, starting point, destination, mandatory locations,
budget, and relevant preferences. TripMate validates the request and internally provides the
constraints to the CSP Engine, which attempts to generate an optimized itinerary satisfying the
available conditions.

# Actors

- **Traveler** (primary) — an authenticated user with the Traveler role.
- **System (TripMate Platform)** — validates the request and forwards constraints to the CSP
  Engine.
- **CSP Engine** (secondary, internal) — attempts to generate an optimized itinerary; its internal
  algorithm is an implementation detail, not part of this use case's UI/UX scope.

# Preconditions

- The Traveler is authenticated (UC-04 Sign In). *(Explicit)*
- The Traveler has planning constraints to provide (available time, starting point, destination,
  etc.). *(Explicit)*
- Travel preferences (UC-09), if previously set, exist and can be used as soft constraints.
  *(Explicit — "relevant preferences" reused from UC-09, not redefined here)*

# Main Flow

1. Traveler navigates to Create Scheduling Request. *(Explicit)*
2. Traveler provides planning constraints: available travel time, starting point, destination,
   mandatory locations, budget. *(Explicit)*
3. Traveler optionally reviews/adjusts relevant preferences for this request (reused from UC-09).
   *(Explicit — "relevant preferences" named as an input)*
4. Traveler submits the request. *(Explicit)*
5. System validates the request. *(Explicit)*
6. System provides the constraints to the CSP Engine. *(Explicit)*
7. CSP Engine attempts to generate an optimized one-day itinerary satisfying the available
   conditions. *(Explicit)*
8. System presents the generated itinerary to the Traveler, if successful. *(Assumption — the
   presentation of results is implied by "generate a personalized itinerary" but not detailed)*

# Alternative Flows

- **AF1 — No mandatory locations provided:** mandatory locations are named as a constraint but not
  stated as required — treating them as optional. *(Assumption)*
- **AF2 — Traveler has no preferences set (UC-09 skipped):** request proceeds using only hard
  constraints, per BR2 in update-travel-preferences.md. *(Explicit, by reference)*

# Exception Flows

- **EF1 — Missing required constraint(s):** System blocks submission and shows validation errors
  (exact required vs. optional constraints undefined — see Missing Information).
- **EF2 — Invalid constraint value:** e.g., negative budget, malformed time range, invalid
  location. System rejects with a field-level error. *(Assumption on specifics)*
- **EF3 — CSP Engine cannot find a feasible itinerary:** the request satisfies basic validation
  but the constraints are infeasible together (e.g., too many mandatory locations for the
  available time/budget). Behavior — error message, partial result, or suggestion to relax
  constraints — is undefined. *(See Missing Information — this is the most significant gap for
  UX, since it's a very plausible outcome, not an edge case)*
- **EF4 — System/CSP service failure:** System informs the Traveler that itinerary generation
  could not be completed. *(Assumption)*

# Postconditions

**Success:**
- A generated one-day itinerary is available to the Traveler, satisfying the provided
  constraints. *(Assumption on presentation — see Missing Information)*

**Failure (validation):**
- No request is sent to the CSP Engine; the Traveler is informed of the specific invalid
  field(s).

**Failure (infeasible / CSP failure):**
- No itinerary is generated; the Traveler is informed the request could not be satisfied (exact
  messaging undefined).

# Functional Requirements

- FR1: The system shall allow a Traveler to submit available travel time, starting point,
  destination, mandatory locations, and budget as planning constraints.
- FR2: The system shall incorporate the Traveler's saved travel preferences (UC-09) as soft
  constraints, when available.
- FR3: The system shall validate the request before forwarding it to the CSP Engine.
- FR4: The system shall forward validated constraints to the CSP Engine for itinerary generation.
- FR5: The system shall present the generated itinerary to the Traveler on success.
- FR6: The system shall inform the Traveler when no feasible itinerary can be generated (EF3).

# Business Rules

- BR1: Hard constraints (time, starting point, destination, mandatory locations, budget) are
  required inputs to the CSP Engine; preferences (UC-09) are soft constraints that influence but
  don't strictly bind the result. *(Explicit — consistent with BR1 in
  update-travel-preferences.md)*
- BR2 *(assumption)*: Only one active scheduling request/itinerary is generated per submission —
  there's no indication of saving multiple draft requests or a request history in source.
- BR3 *(assumption)*: A Traveler without saved preferences can still submit a request (per BR2 in
  update-travel-preferences.md); this use case does not require UC-09 to have been completed
  first.

# Edge Cases

- Constraints are individually valid but mutually infeasible (see EF3) — the single most
  important undefined behavior for this use case's UX, since a "no feasible itinerary" result is
  a normal, expected outcome of CSP-based generation, not a rare failure.
- Traveler provides a starting point that is far from the destination relative to the available
  time — is this caught by validation (EF2) or only discovered by CSP infeasibility (EF3)?
- Traveler submits mandatory locations that are outside reasonable travel range of the starting
  point/destination — same ambiguity as above.
- CSP Engine takes a long time to respond — no timeout/loading-experience behavior defined.
- Traveler wants to regenerate/retry with slightly different constraints after seeing a result —
  no "adjust and regenerate" flow defined, only a fresh submission.

# Missing Information

- What is the exact required vs. optional status of each constraint (time, starting point,
  destination, mandatory locations, budget)? Are all required, or only some?
- What does the system do when the CSP Engine cannot find a feasible itinerary (EF3) — show an
  error, suggest relaxing specific constraints, or return a partial/best-effort result?
- How is the generated itinerary presented (a list of stops with times? a map? both)? Presentation
  format is not described in source at all.
- Is there a maximum/minimum for "available travel time" or "budget"?
- Can a Traveler save, revisit, or compare multiple generated itineraries, or is each request a
  one-off, discarded after viewing?
- Roughly how long does CSP generation take (affects whether a loading/waiting UX needs special
  design, e.g., progress messaging vs. a simple spinner)?
- Can the Traveler adjust constraints and regenerate without re-entering everything from scratch?

# MVP Scope

**Suggestion** *(not explicit in source — for discussion)*:
- A single-request flow: enter constraints → submit → wait → see result or a
  "couldn't generate an itinerary" message. No saved history, no regenerate-in-place, no map
  visualization at MVP (a simple list of stops/times may be enough to prove the core value).
