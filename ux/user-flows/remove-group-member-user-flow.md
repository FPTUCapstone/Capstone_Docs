# User Flow Overview

UC-20 removes an eligible active group member only after Host confirmation.

# Actor

Traveler acting as Group Host

# Entry Point

Travel Group Details & Members → active member action → Remove Member.

# Primary User Goal

Remove an active member and revoke group-only access.

# Main Flow

| Step | User Action | System Action | Next State |
|---|---|---|---|
| 1 | Selects active member and Remove Member | Displays MSG59 confirmation | Confirmation |
| 2 | Confirms | Disables action; verifies Host permission and active membership | Removing |
| 3 | — | Marks membership Removed, records end timestamp, revokes access | Removed |
| 4 | — | Shows MSG60 and refreshes member list | Updated Members |

# Alternative Flows

Cancel confirmation → close modal with no change.

# Validation Flow

Group exists; current Traveler is Host; target membership is active.

# Error Flow

- Not Host: MSG126.
- Target no longer active: no update; refresh current data.
- Write/network failure: previous membership remains; MSG127.
- Expired session: MSG125.

# Success State

Member is inactive, access is revoked, list reflects the new state, and MSG60 is shown.

# Navigation Map

Members list → Remove confirmation → updated Members list.

# Flow Diagram

```mermaid
flowchart TD
    A[Select active member] --> B[Remove Member]
    B --> C[MSG59 confirmation]
    C -->|Cancel| A
    C -->|Confirm| D{Host and member still valid?}
    D -->|No permission| E[MSG126]
    D -->|No active member| F[No update; refresh]
    D -->|Yes| G[Mark Removed and revoke access]
    G --> H[MSG60 and updated list]
    G -->|Failure| I[MSG127; prior state]
```

# Open Questions

None.

