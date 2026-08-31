# Product Context

UC-23 lets a TripMate Traveler join a private group using a valid code or QR invitation.

# Screen Objective

Design the Flutter Mobile **Join Shared Group Trip** screen with equally clear manual-code and QR-invitation paths.

# Target User

Authenticated Traveler holding an invitation.

# Screen Content

Invitation-code input, Scan QR action, Join CTA, exact validation/business/error messages, joined-group success.

# Required Components

Material 3 app bar, outlined code field, primary Join button, separator, scan-QR card/action, inline messages, loading/disabled state, success toast.

# Required States

Initial, code validation, scanning entry, validating/joining, invalid invitation, already-member, system failure, success/navigation.

# Navigation Context

Travel Groups → Join Shared Group Trip → Travel Group Details & Members.

# UX Constraints

Flutter mobile only, English UI, no public group discovery, no invitation quota, no membership before successful validation.

# Open Questions

None.

# Stitch Prompt

Design a Material Design 3 Flutter mobile screen named **“Join Shared Group Trip”** for TripMate. Actor: authenticated Traveler. Purpose: join a private travel group using either an invitation code or QR invitation.

Use a 390×844 mobile viewport. Top to bottom:

1. Top app bar with Back and exact title.
2. Compact travel-group illustration/icon, not decorative-heavy.
3. Required outlined text field labeled “Invitation code”, sample **HOIAN-8K4P**.
4. Full-width primary button “Join Group”.
5. Centered “or” separator.
6. Large touch-friendly secondary card/button “Scan QR Invitation” with QR icon.
7. Inline message region.

States:

- Initial: empty code, Join disabled, Scan QR available.
- Missing code: exact inline error **“This field is required.”**
- Validating/joining: Join and Scan QR disabled, progress indicator.
- Invalid/expired/unavailable: exact inline text **“This invitation is invalid, expired, or no longer available. Please check the invitation and try again.”**
- Already member: exact inline text **“You are already a member of this travel group.”**
- Failure: exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**
- Success: exact toast **“You have joined "Hoi An Weekend Crew"!”**, then navigate to Travel Group Details & Members.

Do not add invitation usage counts, public groups, friend discovery, or location-sharing opt-in on this screen.

