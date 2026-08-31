# Product Context

TripMate is a mobile-first travel-planning app. UC-16 lets a Traveler store an accessible itinerary and essential map content on-device for dependable offline use.

# Screen Objective

Design the Flutter Mobile **Offline Map & Itinerary** screen, making download completeness, storage requirements, progress, and offline availability unmistakable.

# Target User

Authenticated Traveler preparing for weak or unavailable connectivity.

# Screen Content

Selected trip identity; offline package contents; 150 MB storage requirement; current/downloaded version; progress/status; Download or Retry action; exact Report 3 messages.

# Required Components

Material 3 top app bar, itinerary card, destination imagery/map thumbnail, package checklist, storage/progress indicator, status chip, primary button, timestamp/version metadata, alert/toast region.

# Required States

Initial, Loading/Downloading, Available Offline success, connectivity interruption, insufficient storage, generic failure, inaccessible itinerary, and already-current disabled state.

# Navigation Context

Accessible Itinerary → Offline Map & Itinerary → Offline Itinerary or Back. Session expiry preserves the intended destination through Sign In.

# UX Constraints

Flutter-oriented, single-column mobile viewport, outdoor-readable contrast, touch-friendly controls, English labels, CR-07 date/time, no server offline-package entity, no partial-success visual.

# Open Questions

None.

# Stitch Prompt

Design a Material Design 3 Flutter mobile screen named **“Offline Map & Itinerary”** for TripMate. Actor: authenticated Traveler. Purpose: download one accessible itinerary, its POIs, route, and map tiles for offline access.

Use a 390×844 mobile viewport. Top to bottom:

1. Top app bar with Back and title “Offline Map & Itinerary”.
2. A compact itinerary card for **Hoi An Weekend — 26/08/2026**, with destination image, route label “Da Nang → Hoi An”, and current version “Version 4”.
3. An “Offline package” section listing itinerary, POIs, route, and map tiles with compact icons.
4. Storage panel showing “150 MB required” and available-device-storage status.
5. Download status area with version/timestamp metadata in dd/MM/yyyy and HH:mm.
6. One full-width primary action: “Download for Offline Use” or “Retry Download”.

Show these variants:

- Initial: package ready, primary action enabled.
- Downloading: determinate/indeterminate progress, action disabled, exact message **“Downloading offline trip package (itinerary, POIs, map tiles)...”**
- Success: green “Available Offline” chip, downloaded version/time, exact toast **“Trip package downloaded! Itinerary is now available offline without internet.”**
- Interrupted: no completed chip, exact alert **“Download interrupted due to connection loss. Partial download discarded. Please retry.”**
- Insufficient storage: exact alert **“Insufficient storage space. At least 150MB free space required for offline map data.”**
- Generic failure: exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**
- Already current: completed metadata visible and primary download action disabled.

Never visually treat a partial download as available offline. Keep the design functional, map/travel-oriented, scanable outdoors, and minimally decorated.

