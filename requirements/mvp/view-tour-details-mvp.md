# MVP Goal

Allow a Guest or Traveler to inspect complete current Tour details and availability before deciding to book.

# Target User

Guest or authenticated Traveler on Flutter Mobile or responsive Next.js Web.

# Core User Problem

The user needs trustworthy Tour content, operator identity, itinerary, price, departure, capacity, and availability before booking.

# Core User Journey

Select public Tour → load current content and availability → show complete details → Traveler may book; Guest is sent through authentication before UC-27.

# Must Have

- Canonical Flutter Mobile and responsive Next.js Web support; current Stitch artifact is Mobile UI only.
- Show Tour identity/title, Tour Operator, description, included POIs, itinerary, timeline, meeting point where applicable, price, departure information, remaining capacity, inclusions/other supported information, and availability.
- Retrieve current availability before presenting booking as available.
- Never present unavailable/sold-out Tour as bookable; show MSG65.
- Guest may view; booking requires Traveler authentication with intended destination preserved.
- Use MSG127 for retrieval failure.
- Viewing is read-only and never creates a Booking.
- Dates/times/money follow CR-07/CR-08.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Booking form (UC-27).
- Editing Tour content.
- Desktop-specific Stitch output.

# MVP User Journey

User views complete mobile-first details; the Book Tour CTA is enabled only when currently eligible and authenticated, or routes a Guest through Sign In first.

# Dependencies

- Approved/public Tour, Tour Operator data, current departure and capacity data.
- Report 3 BR-59, BR-60, CR-07 to CR-12, CR-15, MSG65, MSG125 to MSG127.

# Risks

- Stale capacity or an enabled booking CTA for an unavailable Tour.
- Accidental personal data exposure beyond public operator information.

# Open Questions

Exact “other supported Tour information” is not enumerated and is not expanded.

