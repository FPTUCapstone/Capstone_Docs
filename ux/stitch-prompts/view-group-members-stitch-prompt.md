# Product Context

TripMate UC-19 lets active members view privacy-permitted membership for a private travel group.

# Screen Objective

Design the Flutter Mobile **Travel Group Details & Members** screen with a clear Host, active member list, total count, pagination state, and sharing-status metadata.

# Target User

Authenticated Traveler who is an active group member.

# Screen Content

Group summary, Host/member cards, Joined Timestamp, location-sharing status, total count/list navigation, read-only error/empty states.

# Required Components

Material 3 app bar, destination/group header, Host chip, member list cards/rows, status chips, total count, mobile-appropriate pagination control, skeleton, empty/error component.

# Required States

Initial/loading, loaded, CR-01 empty, permission denied, retrieval failure, role-dependent disabled actions.

# Navigation Context

Travel Groups → Travel Group Details & Members → separate invite/remove/leave/location-sharing UCs.

# UX Constraints

Mobile-only, privacy-minimal data, 20 records per page, preserve page/filter/sort, CR-07 timestamps, no social feed/friend actions.

# Open Questions

Exact permitted profile fields depend on group privacy configuration.

# Stitch Prompt

Design a Material Design 3 Flutter mobile screen named **“Travel Group Details & Members”** for TripMate, representing UC-19. Actor: authenticated active group member. Purpose: view the current Host and active members with only permitted information.

Use a 390×844 mobile viewport. Top to bottom:

1. Top app bar with Back, title “Hoi An Weekend Crew”, and restrained overflow menu.
2. Compact group header with Hoi An image, trip date **26/08/2026**, and total count “6 active members”.
3. A visually emphasized Host row for **Linh Nguyen**, with “Group Host” chip, joined time **08:00 20/08/2026**, and location-sharing status chip.
4. Active member rows for realistic names, avatar initials, Joined Timestamp, and permitted sharing status only.
5. Footer/mobile pagination showing 20 records per page behavior and current/total state where needed.
6. Role-permitted secondary actions as low-emphasis navigation, not social features.

Variants:

- Loading member-card skeletons.
- Loaded list.
- Empty state with exact text **“No records found matching your criteria.”**
- Unauthorized state with exact text **“You do not have permission to access this function.”**
- Failure state with exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Use clear Host and location status chips, outdoor-readable contrast, and compact metadata. Do not expose email, phone, payment, or other unnecessary personal data. Do not add chat, likes, comments, friend actions, or public group discovery.

