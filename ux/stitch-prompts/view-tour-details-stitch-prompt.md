# Product Context

TripMate UC-26 supports Tour Details on Flutter Mobile and responsive Next.js Web. This Stitch artifact covers Mobile UI only. Guests may view; booking requires Traveler authentication.

# Screen Objective

Design a complete mobile **Tour Details** screen exposing Tour identity, operator, description, POIs, itinerary/timeline, meeting point, price, departure, remaining capacity, availability, and booking CTA.

# Target User

Guest or Traveler evaluating a public Tour.

# Screen Content

Image hero; Tour/operator identity; description; included POIs; itinerary/timeline; meeting point; departure; price; remaining slots; availability; sticky Book Tour CTA.

# Required Components

Material 3 app bar, image carousel/hero, status chips, operator card, expandable description, POI cards, vertical timeline, meeting-point card, departure selector/summary, VND price/capacity panel, sticky CTA, skeleton/error components.

# Required States

Loading content/availability, loaded eligible, Guest booking handoff, sold-out/unavailable, unknown availability with disabled CTA, system failure.

# Navigation Context

Search/Recommendations → Tour Details → Sign In if Guest → UC-27; Back restores source list.

# UX Constraints

390×844 mobile-first, current availability before CTA, CR-07/CR-08, read-only viewing, no booking form or desktop design.

# Open Questions

No extra “other Tour information” is invented.

# Stitch Prompt

Design a Material Design 3 Flutter-oriented mobile screen named **“Tour Details”** for TripMate. Actor: Guest or Traveler. Purpose: inspect complete current public Tour information and proceed to booking only when eligible.

Use a 390×844 viewport. Top to bottom:

1. Edge-to-edge destination image hero with Back and restrained share-neutral navigation only; no wishlist.
2. Tour title **“Da Nang Heritage & Coast Day Tour”**, public/available status chip, duration.
3. Tour Operator card with permitted public identity.
4. Description.
5. “Included places” horizontal/compact cards: My Khe Beach, Marble Mountains, Son Tra Peninsula, Dragon Bridge.
6. “Itinerary” vertical timeline with times such as **08:00**, **10:30**, **14:00**, **17:30**.
7. Meeting point card with address/map thumbnail.
8. Departure information for **26/08/2026 · 08:00**.
9. Availability block: **3 seats left**.
10. Sticky bottom bar with read-only **850,000 VND** and primary **“Book Tour”** CTA.

Variants:

- Loading: content skeleton and availability loader; Book Tour disabled.
- Eligible Traveler: CTA enabled.
- Eligible Guest: tapping Book Tour routes to Sign In, preserving Tour destination.
- Sold-out/unavailable: exact inline message **“This tour package is currently unavailable for booking.”** and disabled CTA; other public details may remain visible.
- Availability unknown/system failure: disabled CTA and exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Do not add participant inputs, payment controls, editable price, wishlist, social comments, or admin/operator controls.

