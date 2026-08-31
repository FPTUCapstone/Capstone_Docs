# Product Context

TripMate UC-21 lets an active Traveler leave a private travel group while BR-49 preserves a valid Host or closes an empty group.

# Screen Objective

Design the group-context entry and the **Leave Travel Group Confirmation** modal with three locked behavioral variants and no manual Host selection.

# Target User

Authenticated active group member, including the Group Host.

# Screen Content

Group context, Leave action, non-Host confirmation, deterministic Host-succession confirmation, final-Host closure confirmation, disabled/loading and success/error outcomes.

# Required Components

Material 3 group view, Leave action, centered modal, successor read-only row, Cancel/Leave buttons, exact locked messages, progress and toast/alert states.

# Required States

Member confirmation, Host-with-members succession, final-Host closure, cancel, loading/disabled, transfer failure, generic failure, success/redirect.

# Navigation Context

Travel Group Details & Members → Leave Travel Group Confirmation → Travel Groups/safe destination.

# UX Constraints

Flutter mobile only; successor is earliest Joined Timestamp; no Host picker; transfer precedes leave; no optimistic local leave.

# Open Questions

None.

# Stitch Prompt

Design connected Material Design 3 Flutter mobile views for TripMate UC-21.

Base view: **Travel Group Details & Members** for **Hoi An Weekend Crew**, with current member/Host status and a low-emphasis destructive “Leave Group” action.

Modal name: **“Leave Travel Group Confirmation”**. Use a 390×844 mobile viewport with a compact dialog over the dimmed group view. Actor: active Traveler. Purpose: confirm leaving while preserving BR-49 Host state.

Create three confirmation variants:

1. Non-Host member: show exact message **“Are you sure you want to continue? This action may not be reversible.”**
2. Group Host with remaining members: show a read-only successor row for **Minh Tran**, selected by earliest Joined Timestamp, and exact message **“You are the Group Host. Leaving will transfer Host privileges to Minh Tran. Confirm leave?”**
3. Group Host as final active member: explain the group will close and show exact message **“Are you sure you want to continue? This action may not be reversible.”**

Each dialog has secondary “Cancel” and destructive “Leave Group”. Do not add any Host selection control.

States:

- Confirmation ready.
- Leaving: progress on Leave Group and both actions disabled.
- Success: redirect outside the group and exact toast **“Operation completed successfully.”**
- Failure: keep current membership/Host state and exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**
- Successor-transfer failure: keep current Host in the group; do not show success.

Use clear status chips and restrained travel visuals. No manual successor picker, group deletion control, admin action, or social features.

