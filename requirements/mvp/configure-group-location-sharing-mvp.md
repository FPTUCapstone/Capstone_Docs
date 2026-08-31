# MVP Goal

Allow an active group member to explicitly enable or disable sharing their GPS location with permitted members.

# Target User

Authenticated Traveler who is an active member of the selected group.

# Core User Problem

The Traveler needs direct control over whether their location is shared, subject to device permission and group privacy.

# Core User Journey

Open settings → toggle sharing → verify active membership and device permission when enabling → save → show exact enabled/disabled message.

# Must Have

- Flutter Mobile only.
- Show current sharing status.
- Explicit opt-in toggle/action.
- Verify active membership.
- When enabling, require device location permission; without it, keep sharing inactive and show MSG46.
- Save enabled/disabled state and show MSG62 or MSG63.
- Restrict shared location visibility to permitted group members.
- Preserve prior value and show MSG127 on save failure; prevent double submission.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Background location map or member tracking UI.
- Sharing with non-members.
- Automatically enabling without explicit opt-in and device permission.

# MVP User Journey

Traveler opens settings, chooses enabled/disabled, and receives the locked result message after a successful save.

# Dependencies

- Active membership, device location-permission state, group privacy rules.
- Report 3 BR-50, BR-51, CR-06, CR-10 to CR-13, CR-15, MSG46, MSG62, MSG63, MSG125 to MSG127.

# Risks

- UI claiming sharing is active when device permission is missing.
- Privacy leakage to non-members.

# Open Questions

None affecting the settings screen.

