# MVP Goal

Allow an active travel-group member to view the current active membership and permitted member information.

# Target User

Authenticated Traveler who is an active member of the selected group.

# Core User Problem

Group members need to know who has joined, who is Host, and the permitted sharing status without exposing unnecessary personal data.

# Core User Journey

Open group → Members → verify active membership → load active members and Host → display permitted information.

# Must Have

- Flutter Mobile only.
- Verify selected group and active membership.
- Display current Host, active members, permitted member information, Joined Timestamp, membership status, and location-sharing status.
- Apply privacy limits from BR-51/CR-15.
- Apply CR-01 list behavior: 20 records per page, total count, preserved page/filter/sort.
- Show MSG126 for unauthorized access, MSG127 for retrieval failure, and MSG128 for applicable empty state.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Editing membership.
- Removing or inviting members.
- Displaying personal data beyond the UC requirement.

# MVP User Journey

Active member opens Members; TripMate verifies membership, loads active memberships and Host, then displays permitted data.

# Dependencies

- Existing travel group and active membership.
- Report 3 BR-48, BR-51, CR-01, CR-07, CR-10 to CR-12, CR-15, MSG125 to MSG128.

# Risks

- Privacy leakage if non-permitted information is displayed.
- Stale membership data could show removed/left members as active.

# Open Questions

The exact permitted member profile fields are governed by group privacy rules and are not expanded here.

