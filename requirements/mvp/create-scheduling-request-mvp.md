# MVP Goal

Let a Traveler describe a one-day trip (time, start, destination, must-see stops, budget) and get
back a generated itinerary — or a clear, honest "couldn't generate one" outcome — in a single
request/response cycle.

# Target User

Authenticated Traveler wanting a personalized one-day plan, optionally with saved preferences
(UC-09) already in place.

# Core User Problem

Manually planning a day trip that fits a time/budget window and includes must-see stops is
tedious; the Traveler wants the system to do the constraint-solving for them.

# Core User Journey

Enter hard constraints (+ optional preferences) → submit → CSP generates → see the itinerary, or
a clear message that it couldn't be generated.

# Must Have

- Input for the named hard constraints: available travel time, starting point, destination,
  mandatory locations, budget. *(FR1)*
- Reuse of saved preferences (UC-09) as soft constraints when present, with graceful degradation
  when absent. *(FR2, BR3)*
- Request validation before forwarding to the CSP Engine. *(FR3)*
- A defined, honest response for **infeasible constraints** (EF3) — even if it's just a plain
  "we couldn't build an itinerary that fits these constraints, try adjusting them" message. This
  is Must Have, not Could Have, because it's a normal, expected outcome, not a rare failure.
- Presentation of a successful result as, at minimum, an ordered list of stops with times — the
  simplest format that proves the core value. *(FR5, scoped down from the undefined "presentation
  format" in Missing Information)*

# Should Have

- Loading/waiting state with reasonable messaging, since CSP generation time is unknown and may
  not be instant.
- Map visualization of the itinerary, in addition to the list — richer, but the list alone can
  carry MVP.

# Could Have

- Save/revisit/compare multiple generated itineraries (no history at MVP — each request is a
  one-off).
- "Adjust and regenerate" without re-entering all constraints from scratch.

# Out of Scope

- Any CSP algorithm behavior itself (internal to the engine, not a UI/UX concern for this repo).
- Partial/best-effort itinerary results when constraints are infeasible — MVP only needs a clear
  failure message, not a "closest possible" fallback (that's a real feature, not a given).

# MVP User Journey

1. Traveler opens Create Scheduling Request.
2. Traveler enters available time, starting point, destination, mandatory locations, budget.
3. (Optional) Traveler's saved preferences (UC-09) are included automatically as soft constraints.
4. Traveler submits.
5. System validates; forwards to CSP Engine.
6. Traveler sees a loading state.
7. On success: itinerary shown as an ordered list of stops with times.
8. On infeasibility: clear message that no itinerary could be generated with these constraints.
9. On system failure: generic error, Traveler can retry.

# Dependencies

- Depends on UC-04 Sign In.
- Optionally depends on UC-09 Update Travel Preferences (degrades gracefully if skipped).
- Depends entirely on the CSP Engine existing and being callable — this is the one use case in
  this batch with a hard backend/algorithm dependency outside the UI/UX layer.

# Risks

- EF3 (infeasible constraints) being left undefined is the single biggest risk in this whole
  batch of use cases: if it's not designed for, the MVP demo will look broken the first time a
  Traveler enters a reasonable-looking but infeasible combination (e.g., 3 mandatory locations in
  a 2-hour window).
- Result presentation format is completely unspecified in source; the "ordered list" MVP default
  here is a judgment call, not a confirmed requirement — flagged clearly rather than silently
  assumed.
- CSP generation latency is unknown; if it's slow, the loading state (Should Have, not Must Have)
  becomes load-bearing UX rather than a nicety.

# Open Questions

- Exact required vs. optional status of each hard constraint.
- Defined behavior for infeasible constraints (error message wording, suggestions to relax
  specific constraints, or something else).
- Confirmed presentation format for the generated itinerary (list, map, or both).
- Typical CSP generation time (affects loading-state design).
- Can a Traveler save/revisit past itineraries?
