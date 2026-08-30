# MVP Goal

Allow a Traveler to view the secure QR e-ticket and current ticket/check-in information for an eligible owned Tour Booking.

# Target User

Authenticated Traveler on Flutter Mobile or responsive Next.js Web.

# Core User Problem

The Traveler needs a trustworthy ticket to present at the meeting point and needs to know its validity and check-in state.

# Core User Journey

Open My Tour Bookings → select eligible Booking → View QR E-ticket → verify ownership/eligibility → display booking/tour/departure/participants/status/QR/validity/check-in details.

# Must Have

- Canonical Flutter Mobile and responsive Next.js Web support; current Stitch artifact is Mobile UI only.
- Show Booking code, Tour, departure, participant information, Booking/payment/ticket status, QR e-ticket, QR validity, check-in status, and check-in timestamp where applicable.
- Only expose active QR for an eligible owned Booking.
- Represent not-yet-active state with MSG98.
- Show MSG93 when an eligible QR is available and MSG127 on retrieval failure.
- Consumed/invalid/cancelled QR must not appear as an unused valid ticket.
- Viewing is read-only and does not modify check-in.
- No camera/scanner functionality; scanning is UC-43.
- Respect CR-07 and CR-15.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- QR scanning or Tour Operator check-in (UC-43).
- Editing Booking/check-in state.
- Desktop-specific Stitch output.

# MVP User Journey

Traveler opens an eligible Booking and views its secure QR and current statuses for presentation.

# Dependencies

- Eligible owned Booking and available QR e-ticket information.
- Report 3 BR-82, BR-83, BR-85, CR-07, CR-10 to CR-12, CR-15, MSG93, MSG98, MSG125 to MSG127.

# Risks

- Exposing a valid-looking QR for cancelled/consumed/ineligible Booking.
- Leaking participant information beyond the Traveler's own Booking.

# Open Questions

None affecting the display-only screen.

