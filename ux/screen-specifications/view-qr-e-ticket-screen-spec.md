# Screen Inventory

1. **Booking Details & QR E-ticket** — Traveler-owned ticket display for UC-29.

# Screen: Booking Details & QR E-ticket

## Related UC

UC-29 — View QR E-ticket

## Actor

Traveler

## Platform

Flutter Mobile and responsive Next.js Web. Current Stitch output covers Flutter Mobile UI only.

## Purpose

Display the secure QR e-ticket and current Booking, payment, ticket validity, and check-in information without scanning or changing state.

## Entry Conditions

Authenticated Traveler opens an owned eligible Tour Booking.

## Entry Points

My Tour Bookings → selected Booking → **View QR E-ticket**.

## Exit / Navigation

Back → My Tour Bookings. No scanner navigation is included.

## Required Data

Booking ID/code; Tour; departure; participant information where applicable; Booking/payment status; QR data/status/issued timestamp/validity; check-in status/timestamp.

## Main Layout Regions

1. Top app bar.
2. Booking/Tour summary.
3. Secure high-contrast QR ticket card.
4. Ticket validity/status.
5. Departure and participant information.
6. Payment/check-in status timeline.
7. Message region.

## Components

### User Inputs

None.

### Read-only Information

All required Booking, Tour, departure, participant, payment, ticket, QR validity, and check-in data.

### Primary Actions

None; display/presentation only.

### Secondary Actions

Back.

## Validation

Booking exists, belongs to Traveler, eligible for active QR. QR corresponds to Booking and satisfies security requirements. Cancelled/consumed/invalid QR never appears unused/valid.

## Business Rules

BR-82, BR-83, BR-85. Apply CR-07, CR-10 to CR-12, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG93 | Your encrypted QR ticket is ready. Present this code at the tour gate. |
| MSG98 | This ticket is valid only on departure date: {Departure_Date}. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG126 | You do not have permission to access this function. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Booking identity shell.

### Loading State

Ticket/QR skeleton; no active-looking placeholder QR.

### Loaded State

Current owned ticket data.

### Empty State

Not applicable; ineligible/no ticket is Business Error.

### Validation Error State

Not applicable to input.

### Business Error State

Wrong owner uses MSG126; cancelled/ineligible has no active QR; not-yet-active uses MSG98; consumed shows checked-in status and not unused-valid styling.

### System Error State

MSG127; no QR payload shown.

### Success State

Eligible active QR shown with MSG93 and current statuses.

### Confirmation State

Not applicable.

### Disabled Action State

No check-in/scan action exists.

### Offline State

If securely cached display is not explicitly supported, do not assume it; retrieval failure uses MSG127.

## Navigation Map

My Tour Bookings → Booking Details & QR E-ticket → Back.

## Open Questions

None.

## UX Suggestions

Use a white QR card with large quiet zone and strong status chips; avoid camera icon/actions that imply scanning.

