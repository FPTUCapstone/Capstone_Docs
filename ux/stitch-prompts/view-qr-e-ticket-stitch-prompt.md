# Product Context

TripMate UC-29 supports QR e-ticket viewing on Flutter Mobile and responsive Next.js Web. This Stitch artifact covers Mobile UI only. The screen belongs to the Traveler; Tour Operator scanning is UC-43.

# Screen Objective

Design the mobile **Booking Details & QR E-ticket** screen with secure ticket presentation, validity, payment, and check-in statuses but no scanner.

# Target User

Authenticated Traveler viewing an owned eligible Tour Booking.

# Screen Content

Booking code, Tour, departure, participants, payment/ticket status, QR e-ticket, validity, check-in status/timestamp.

# Required Components

Material 3 app bar, Booking/Tour card, white high-contrast QR card, status chips, validity block, participant details, payment/check-in timeline, skeleton/error variants.

# Required States

Loading, active eligible ticket, not-yet-active, consumed/checked-in, cancelled/ineligible with no active QR, unauthorized, retrieval failure.

# Navigation Context

My Tour Bookings → Booking Details & QR E-ticket → Back.

# UX Constraints

390×844 mobile-first, display-only, CR-07, privacy-minimal, no camera/scanner or check-in action.

# Open Questions

None.

# Stitch Prompt

Design a Material Design 3 Flutter-oriented mobile screen named **“Booking Details & QR E-ticket”** for TripMate. Actor: authenticated Traveler. Purpose: view and present an eligible owned Tour Booking’s secure QR ticket and current statuses.

Use a 390×844 viewport. Top to bottom:

1. Top app bar with Back and exact title.
2. Booking/Tour summary:
   - Booking code **TM-DB-260826-1042**
   - **Da Nang Heritage & Coast Day Tour**
   - **26/08/2026 · 08:00**
3. Payment, Booking, and Ticket status chips.
4. Large centered white QR e-ticket card with strong quiet-zone contrast; do not add a camera icon.
5. QR validity/issued information.
6. Participant-information section using privacy-minimal fields.
7. Check-in status and check-in timestamp where applicable.

Variants:

- Loading: neutral QR placeholder skeleton that cannot be mistaken for a valid code.
- Active: valid QR visible and exact toast **“Your encrypted QR ticket is ready. Present this code at the tour gate.”**
- Not yet active: no active-looking QR and exact text **“This ticket is valid only on departure date: 26/08/2026.”**
- Consumed: show “Checked in” status and timestamp; do not style as unused.
- Cancelled/ineligible: no active QR.
- Unauthorized: exact text **“You do not have permission to access this function.”**
- Failure: hide QR payload and show exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Do not add camera/scanner functionality, a “Scan QR” action, Tour Operator controls, check-in mutation, ticket transfer, or payment instrument details.

