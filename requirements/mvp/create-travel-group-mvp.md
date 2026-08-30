# MVP Goal

Allow an authenticated Traveler to create one travel group linked to an eligible TripMate itinerary and become its Group Host.

# Target User

Authenticated Traveler with access to an eligible itinerary.

# Core User Problem

The Traveler needs a shared group container tied to an existing trip before inviting companions.

# Core User Journey

Open eligible itinerary → create group → enter Group Name → validate → create and link group → assign creator as exclusive Host → show success and open group.

# Must Have

- Flutter Mobile only.
- Existing accessible itinerary shown as the required association.
- Required valid Group Name input.
- Create exactly one group record linked to the itinerary.
- Automatically and exclusively assign the creator as Group Host.
- Prevent double submission.
- Show MSG01 for missing required input, MSG54 on success, and MSG127 on failure.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Inviting members (UC-18).
- Joining a group (UC-23).
- Public group discovery.
- Host self-selection or alternate Host assignment at creation.

# MVP User Journey

1. Traveler opens an eligible itinerary and selects Create Travel Group.
2. TripMate shows the selected itinerary and Group Name input.
3. Traveler submits.
4. TripMate validates, creates the linked group, assigns the Traveler as Host, shows MSG54, and opens the group.

# Dependencies

- Authenticated Traveler and existing accessible itinerary.
- Report 3 BR-41, BR-42, CR-03, CR-04, CR-06, CR-10 to CR-13, MSG01, MSG54, MSG125 to MSG127.

# Risks

- Creating a group without a valid itinerary or without one exclusive Host would violate the finalized model.

# Open Questions

None that affect the defined input set.

