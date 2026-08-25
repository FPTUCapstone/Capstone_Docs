# Screen Inventory

1. Create Scheduling Request Form — enter constraints, submit, and see the "infeasible" outcome
   inline (so the Traveler can adjust and resubmit without losing context).
2. Itinerary Result Screen — shown only on a successful, feasible result.

2 screens are needed for the Must Have MVP journey.

---

# Screen: Create Scheduling Request Form

## Purpose

Collect the Traveler's hard constraints for a one-day itinerary, submit them (with saved
preferences, if any) to the CSP Engine, and handle both the "generating" wait and an infeasible
outcome without losing the Traveler's input.

## User

Authenticated Traveler.

## Entry Conditions

Traveler is authenticated and navigates to Create Scheduling Request.

## Required Components

### Inputs

- Available travel time.
- Starting point.
- Destination.
- Mandatory locations (assumed optional/multi — see Open Questions).
- Budget.

### Information Display

- Indication that saved preferences (UC-09), if any, will be included automatically — not stated
  as required copy in source, but reduces confusion about where preferences factor in.
- Infeasible-result message (E2), shown inline after a failed generation attempt, without
  clearing the Traveler's entered constraints.

### Primary Actions

- Generate Itinerary (submit).

### Secondary Actions

None defined.

## Validation

- Required-field check on hard constraints (exact required/optional split still open — see Open
  Questions).
- Format/range checks (e.g., budget as a positive number, internally consistent time range).

## Error Handling

- **E1 — Missing/invalid constraint value:** inline field-level error; nothing sent to the CSP
  Engine.
- **E2 — Infeasible constraints:** a clear, non-alarming message that no itinerary could be
  generated with the current constraints, inviting adjustment — **treated as an expected outcome,
  not an error state**. Entered values remain on screen for editing.
- **E3 — System/CSP service failure:** generic error; Traveler can retry.

## States

- **Input** — empty or partially filled form.
- **Validation Error** — E1.
- **Loading / Generating** — request sent, waiting on the CSP Engine; duration unknown (see Open
  Questions in the MVP doc) — Should Have a loading message, not just a bare spinner.
- **Infeasible** — E2, inline on this same screen.
- **System Error** — E3.
- **Success** — navigates to the Itinerary Result Screen.

## Navigation

- On success → Itinerary Result Screen.
- On infeasible (E2) or any error → stays on this screen, values retained.

## Business Rules

BR1, BR2, BR3 (see [requirements/create-scheduling-request.md](../../requirements/create-scheduling-request.md)).

## Open Questions

- Exact required vs. optional status of each constraint.
- Typical CSP generation time (affects how the Loading/Generating state should be designed —
  simple spinner vs. progress messaging).
- Can mandatory locations be more than one? Is there a limit?

## UX Suggestions

- Show which saved preferences (if any) will be applied, as a small summary/chip list, so the
  Traveler isn't guessing what "relevant preferences" means for this request — not a confirmed
  requirement.

---

# Screen: Itinerary Result Screen

## Purpose

Present the CSP-generated itinerary to the Traveler as an ordered list of stops with times.

## User

Traveler who just received a successful, feasible itinerary.

## Entry Conditions

Reached only after a successful generation from the Create Scheduling Request Form.

## Required Components

### Inputs

None — display-only screen (MVP scope; no map or editing at MVP).

### Information Display

- Ordered list of stops, each with an associated time.
- Not confirmed whether per-stop details (address, cost, duration) are shown — see Open
  Questions; do not invent beyond "stop + time" without confirmation.

### Primary Actions

Not confirmed in source — e.g., "Start a new request" or "Save this itinerary" are plausible but
unconfirmed. See Open Questions.

### Secondary Actions

None confirmed.

## Validation

Not applicable — display-only screen.

## Error Handling

Not applicable — this screen is only reached on success.

## States

- **Success** — the only confirmed state; shows the generated list.

## Navigation

- Reached from: Create Scheduling Request Form, on success.
- Leads to: unresolved (see Open Questions) — e.g., could return to the Request Form for a new
  request.

## Business Rules

FR5 (see [requirements/create-scheduling-request.md](../../requirements/create-scheduling-request.md)).

## Open Questions

- What per-stop details are shown beyond a time (address, cost, duration, why it was chosen)?
- Is there an action to save or share this itinerary, or start a new request?
- Should a map view accompany the list (Should Have, not Must Have per the MVP scope)?

## UX Suggestions

- A clear "Create another request" action, so the Traveler isn't left on a dead-end screen —
  mirrors the same open concern flagged for other post-success screens in this batch (e.g.,
  Pending Approval Confirmation for Tour Operator registration).
