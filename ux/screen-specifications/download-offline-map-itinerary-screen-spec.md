# Screen Inventory

1. **Offline Map & Itinerary** — Flutter Mobile download/status screen for UC-16.

# Screen: Offline Map & Itinerary

## Related UC

UC-16 — Download Offline Map & Itinerary

## Actor

Traveler

## Platform

Flutter Mobile only

## Purpose

Let an authenticated Traveler download the selected accessible itinerary, POIs, route, and map data and verify that the complete current version is available offline.

## Entry Conditions

- Traveler is authenticated.
- Selected itinerary exists and is accessible.
- Screen is opened before or during a trip from an itinerary the Traveler can access.

## Entry Points

Accessible Itinerary → **Download for Offline Use**.

## Exit / Navigation

- Back → selected Itinerary.
- Completed download → remain on this screen in Available Offline state; supported offline itinerary can be opened.
- Expired session → Sign In, preserving the intended destination.

## Required Data

Traveler ID; Itinerary ID/title/version/items; related POIs; route/map package summary; package-size/storage requirement; local storage availability; download status/progress; downloaded version; download timestamp.

## Main Layout Regions

1. Material 3 top app bar with Back and exact screen name.
2. Compact selected-itinerary card using sample trip content.
3. Offline package contents and 150 MB storage requirement.
4. Download/status panel with progress or completed metadata.
5. Primary action area and message region.

## Components

### User Inputs

None.

### Read-only Information

- Itinerary identity and dates.
- Included offline data: itinerary, POIs, routes, map tiles.
- Current server itinerary version.
- Downloaded version and timestamp when available.
- Storage requirement/status and progress.

### Primary Actions

- **Download for Offline Use**.
- **Retry Download** after interruption/failure.

### Secondary Actions

- Back.
- Open Offline Itinerary when complete.

## Validation

- Authenticated Traveler; existing accessible itinerary (BR-36).
- Sufficient storage; package must not exceed the configured 150 MB limit.
- All required content must be stored before “available offline” is shown (BR-37).
- Disable the submit control while download request is starting (CR-13).

## Business Rules

BR-36, BR-37, BR-38, BR-39. Apply CR-07, CR-10, CR-11, CR-12, CR-13, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG104 | Downloading offline trip package (itinerary, POIs, map tiles)... |
| MSG105 | Trip package downloaded! Itinerary is now available offline without internet. |
| MSG106 | Download interrupted due to connection loss. Partial download discarded. Please retry. |
| MSG107 | Insufficient storage space. At least 150MB free space required for offline map data. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG126 | You do not have permission to access this function. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Selected itinerary and package requirements loaded; Download action available if not already stored.

### Loading State

Progress indicator and MSG104; repeat action disabled.

### Loaded State

Current package/download metadata shown.

### Empty State

Not applicable: entry requires a selected itinerary. If it disappears or becomes inaccessible, show Business Error rather than an empty catalogue.

### Validation Error State

Insufficient storage uses MSG107; no partial availability.

### Business Error State

Inaccessible itinerary uses MSG126; unavailable/changed source prevents download.

### System Error State

Connectivity interruption uses MSG106. Other failures use MSG127. Previous completed offline version, if any, is not falsely replaced.

### Success State

Show MSG105, Available Offline status chip, downloaded version, and timestamp formatted per CR-07.

### Confirmation State

Not required; download is reversible/retryable.

### Disabled Action State

Download/Retry is disabled while a request is starting or active, and when the current version is already complete offline.

### Offline State

If connectivity is lost before completion, show MSG106 and do not mark complete. A previously completed package remains visibly available.

## Navigation Map

Itinerary → Offline Map & Itinerary → Available Offline / Retry → Itinerary or Offline Itinerary.

## Open Questions

None affecting the approved screen behavior.

## UX Suggestions

Use a compact storage/progress bar and an “Available Offline” status chip. These are presentation suggestions, not new requirements.

