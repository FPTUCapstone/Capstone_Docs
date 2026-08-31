# MVP Goal

Allow the Group Host to confirm and remove one eligible active member while immediately revoking that member's group-only access.

# Target User

Traveler acting as Group Host.

# Core User Problem

The Host needs a controlled, confirmed way to remove an active member from the shared trip.

# Core User Journey

Open members → select eligible active member → Remove Member → confirm MSG59 → verify permission/membership → mark Removed → revoke access → MSG60 → refresh list.

# Must Have

- Flutter Mobile only.
- Reuse Travel Group Details & Members.
- Use Remove Group Member Confirmation modal/dialog.
- Only Group Host may act; selected Traveler must be active.
- Require MSG59 before sending the irreversible request.
- On success set membership to Removed, record end timestamp, revoke member-only access, refresh list, and show MSG60.
- Keep prior membership unchanged on failure; use MSG126/MSG127 as applicable.
- Prevent double submission.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Host succession or leaving the group.
- Deleting the Traveler account.
- Public member management.

# MVP User Journey

Host selects an active member, confirms removal, and sees the updated list and locked success toast.

# Dependencies

- Existing group, authenticated Host, active target membership.
- Report 3 BR-45, BR-48, CR-01, CR-05 to CR-07, CR-10 to CR-13, CR-15, MSG59, MSG60, MSG125 to MSG127.

# Risks

- Removing an already inactive member or acting without Host permission.
- Failing to revoke member-only access after status changes.

# Open Questions

None affecting the finalized confirmation/removal behavior.

