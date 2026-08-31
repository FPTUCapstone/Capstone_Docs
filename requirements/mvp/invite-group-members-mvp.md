# MVP Goal

Allow the authenticated Group Host to generate and share a usable invitation code and QR invitation for an existing travel group.

# Target User

Traveler acting as Group Host.

# Core User Problem

The Host needs a secure, supported invitation artifact that eligible Travelers can use to join the correct group.

# Core User Journey

Open group → Invite Members → verify Host → generate invitation → display code and QR → copy or share.

# Must Have

- Flutter Mobile only.
- Verify selected group and current Traveler's Host permission.
- Generate a unique usable invitation associated with the group.
- Display both invitation code and QR invitation.
- Allow copy/share; MSG55 is shown when the code is copied.
- Do not create membership when generating or sharing.
- Use MSG126 for permission denial and MSG127 for generation failure.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Joining the group (UC-23).
- Invitation usage quotas or public discovery.
- Member management.

# MVP User Journey

Host opens group, selects Invite Members, receives code and QR, then copies or shares them.

# Dependencies

- Existing group and authenticated Group Host.
- Report 3 BR-43, BR-44, BR-46, CR-10 to CR-13, MSG55, MSG125 to MSG127.

# Risks

- A generated invitation associated with the wrong group or one that creates membership immediately would violate the finalized rules.

# Open Questions

Expiration is represented only where applicable; no invitation usage quota is defined.

