# Product Context

TripMate UC-20 lets a Group Host remove an eligible active member from a private travel group after mandatory confirmation.

# Screen Objective

Design the member-management context and the **Remove Group Member Confirmation** modal, preserving the exact locked confirmation/success messages.

# Target User

Authenticated Traveler acting as Group Host.

# Screen Content

Travel Group Details & Members list, selected member action, confirmation modal, loading/disabled action, refreshed success/error outcomes.

# Required Components

Material 3 member list, overflow action, centered confirmation dialog over dimmed background, Cancel and destructive Remove Member actions, exact text, toast/error region.

# Required States

Loaded members, confirmation, cancel, removing/disabled, success/refreshed list, unauthorized/stale member, network failure.

# Navigation Context

Travel Group Details & Members → Remove Group Member Confirmation → updated members.

# UX Constraints

Flutter mobile only; CR-05 confirmation before request; CR-06 success toast; no optimistic removal; no removal without Host permission.

# Open Questions

None.

# Stitch Prompt

Design two connected Material Design 3 Flutter mobile views for TripMate UC-20.

## View 1 — Travel Group Details & Members

Use a 390×844 mobile viewport. Show **Hoi An Weekend Crew**, active-member total, Host chip, and privacy-limited member rows. For an eligible active member named **Minh Tran**, expose a low-emphasis overflow action “Remove Member”. Preserve a scanable travel-group style, 20-record list behavior, and status chips.

## View 2 — Remove Group Member Confirmation

Show a compact Material 3 modal/dialog over the dimmed member list. Exact screen/modal name: **“Remove Group Member Confirmation”**. Actor: Group Host. Purpose: confirm irreversible member removal.

Modal content:

- Short title “Remove group member”.
- Exact body text: **“Are you sure you want to remove Minh Tran from this group?”**
- Secondary action “Cancel”.
- Clearly destructive action “Remove Member”.

States:

- Confirmation: both actions enabled.
- Removing: progress on Remove Member; both actions disabled to block double submission.
- Success: modal closes, Minh disappears from active list, exact toast **“Minh Tran has been removed from the group.”**
- Unauthorized: exact message **“You do not have permission to access this function.”**
- Failure: member remains, exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Do not add account deletion, ban controls, Host succession, or admin tools. Keep the locked message wording exact.

