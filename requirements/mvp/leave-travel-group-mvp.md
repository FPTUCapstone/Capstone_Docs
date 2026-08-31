# MVP Goal

Allow an active Traveler to leave a travel group while preserving exactly one valid Host whenever members remain and closing an empty group.

# Target User

Authenticated Traveler who is an active group member, including the current Group Host.

# Core User Problem

A member needs to leave voluntarily without creating an invalid Host-less group.

# Core User Journey

Select Leave Group → determine Host state → show the locked confirmation → confirm → transfer Host first when required or close final-member group → mark membership Left → revoke access → redirect and report success.

# Must Have

- Flutter Mobile only.
- Verify active membership and determine whether the Traveler is Host.
- Non-Host: confirm with MSG130, then leave.
- Host with other active members: select the active remaining member with earliest Joined Timestamp, identify that successor using MSG61, transfer Host before marking the current Host Left.
- Host as final active member: use MSG130, close the group, then mark membership Left.
- Record membership end timestamp, revoke member-only access, redirect away.
- Use MSG129 when no action-specific success message exists.
- Never remove the Host if successor transfer fails; prevent double submission.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Manual Host selection.
- Keeping an active group with no Host.
- Deleting the group record.
- Leaving other groups.

# MVP User Journey

The correct confirmation is shown for the member's current Host situation; the operation completes atomically enough to preserve valid Host state.

# Dependencies

- Existing group and active membership data with Joined Timestamp.
- Report 3 BR-48, BR-49, CR-05 to CR-07, CR-10 to CR-13, MSG61, MSG125 to MSG130.

# Risks

- Incorrect successor ordering or changing membership before Host transfer can leave active members without a Host.
- Membership may change between confirmation and submit.

# Open Questions

None; successor selection is locked by BR-49.

