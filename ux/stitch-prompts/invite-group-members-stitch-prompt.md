# Product Context

UC-18 lets a TripMate Group Host generate a code and QR invitation for a private travel group. Invitation generation does not create membership.

# Screen Objective

Design the Flutter Mobile **Invite Group Members** screen with a highly scannable QR invitation, copyable code, and share action.

# Target User

Authenticated Traveler acting as Group Host.

# Screen Content

Group identity, QR invitation, invite code, optional expiry/status, Copy and Share actions, exact permission/copy/failure states.

# Required Components

Material 3 app bar, group card, high-contrast QR card, read-only code field, copy icon button, share button, loading skeleton, message region.

# Required States

Loading, invitation ready, code-copied success, permission denied, network/generation failure, actions disabled until ready.

# Navigation Context

Travel Group Details & Members → Invite Group Members → Back. Recipient uses UC-23 separately.

# UX Constraints

Mobile-only, English UI, no member creation, no invitation quota, no public group discovery.

# Open Questions

Show expiration only if the source provides it.

# Stitch Prompt

Design a Material Design 3 Flutter mobile screen named **“Invite Group Members”** for TripMate. Actor: authenticated Traveler acting as Group Host. Purpose: display a usable invitation code and QR invitation for the selected group; sharing must not create membership.

Use a 390×844 mobile viewport. Top to bottom:

1. Top app bar with Back and title “Invite Group Members”.
2. Group identity card for **Hoi An Weekend Crew**, with Hoi An thumbnail and Host status chip.
3. Centered white QR invitation card with strong quiet-zone contrast.
4. Read-only invite code field using realistic code **HOIAN-8K4P**, plus touch-friendly copy icon.
5. Full-width “Share Invitation” button.
6. Optional small expiry/status line only if data exists.

States:

- Loading: QR/code skeletons; Copy and Share disabled.
- Ready: code and QR visible.
- Copy success: exact toast **“Invite code "HOIAN-8K4P" copied to clipboard.”**
- Permission denied: exact inline message **“You do not have permission to access this function.”**
- Failure: exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Do not add invitation usage counts, member quotas, public links, friend-system controls, or any claim that sharing has joined a member.

