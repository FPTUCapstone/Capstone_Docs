# Product Context

UC-22 gives a TripMate group member explicit control over sharing their GPS location with permitted group members.

# Screen Objective

Design the Flutter Mobile **Group Location Sharing Settings** screen with one truthful opt-in switch and device-permission handling.

# Target User

Authenticated active travel-group member.

# Screen Content

Group identity, privacy explanation, saved status, device-permission state, switch, exact enabled/disabled/permission/failure messages.

# Required Components

Material 3 app bar, group card, privacy/status card, location switch, optional Open Settings action, toast/inline alert, loading/disabled state.

# Required States

Loading, sharing disabled, sharing enabled, permission denied, saving/disabled, unauthorized, system failure.

# Navigation Context

Travel Group Details & Members → Group Location Sharing Settings → optional device Settings → return.

# UX Constraints

Flutter mobile only; active visual requires both explicit opt-in and device permission; no live group map or non-member sharing.

# Open Questions

None.

# Stitch Prompt

Design a Material Design 3 Flutter mobile screen named **“Group Location Sharing Settings”** for TripMate. Actor: authenticated active group member. Purpose: explicitly enable or disable sharing the Traveler’s GPS location with permitted members of **Hoi An Weekend Crew**.

Use a 390×844 mobile viewport. Top to bottom:

1. Top app bar with Back and exact screen title.
2. Group identity card for Hoi An Weekend Crew.
3. Privacy-focused card explaining the setting applies only to permitted group members.
4. Status row with location icon, “Live location sharing”, and one Material switch.
5. Small device-permission status row.
6. Message/toast area.

Variants:

- Loaded/disabled: switch off, neutral “Not sharing” status.
- Saving: switch disabled with progress.
- Enabled success: switch on, active status chip, exact toast **“Live location sharing is now active with group members.”**
- Disabled success: switch off, exact toast **“Live location sharing disabled.”**
- Permission denied: keep switch off and show exact inline message **“Location permission is required for real-time navigation and trip tracking. Please enable it in Settings.”** Add a low-emphasis “Open Settings” action.
- Unauthorized: exact text **“You do not have permission to access this function.”**
- Failure: revert to prior saved state and exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Do not show a member map, background tracking dashboard, sharing to non-members, or an enabled status when device permission is absent.

