# Screen Inventory

1. **Tour Booking Confirmation** — booking-input, calculated-summary, confirmation, and pending-payment screen for UC-27.

# Screen: Tour Booking Confirmation

## Related UC

UC-27 — Book Tour

## Actor

Traveler

## Platform

Flutter Mobile and responsive Next.js Web. Current Stitch output covers Flutter Mobile UI only.

## Purpose

Collect trusted booking inputs, display TripMate-calculated price/discount/total/expiration as read-only, confirm the Booking, and reserve capacity independently of payment.

## Entry Conditions

Authenticated Traveler selected Book Tour for an eligible public Tour.

## Entry Points

Tour Details → **Book Tour**.

## Exit / Navigation

Back → Tour Details before creation. Success → pending-payment state, Booking Details, or UC-28 Pay Now.

## Required Data

Selected Tour/operator; available departure date/time; remaining slots; requested quantity; participant full name and ID/Passport; optional voucher code; unit/base price; discount; total; Booking code/status; payment status; booked/expiration timestamps.

## Main Layout Regions

1. Top app bar and selected Tour summary.
2. Departure selector.
3. Passenger/slot quantity stepper.
4. Repeated participant-information sections.
5. Voucher input/apply/remove.
6. Read-only price breakdown.
7. Payment-expiration information.
8. Confirmation CTA and message region.

## Components

### User Inputs

Departure where applicable; passenger/slot quantity; required participant full name and ID/Passport; optional voucher code.

### Read-only Information

Selected Tour/operator; remaining capacity; base/unit price; discount; total amount; payment expiration; Booking/payment status and code after success.

### Primary Actions

- **Confirm Booking**.
- **Pay Now** after successful creation where electronic payment is required.

### Secondary Actions

Apply Voucher; Remove Voucher; Back.

## Validation

Authenticated; Tour eligible; quantity <= current capacity; participant data complete/valid; voucher valid/applicable. TripMate calculates all money; client values are not trusted. Confirm revalidates capacity. Apply CR-03/CR-04/CR-13.

## Business Rules

BR-61 to BR-64, BR-68, BR-97 to BR-100 where a voucher is applied. Apply CR-03 to CR-13 and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG01 | This field is required. |
| MSG77 | Only {Remaining_Slots} seats left for this tour. Please reduce passenger count. |
| MSG78 | Passenger full name and valid ID/Passport number are required. |
| MSG79 | Booking created! Please complete payment within 15 minutes to secure your seats. |
| MSG80 | Your booking reservation has expired due to non-payment within 15 minutes. Seats released. |
| MSG100 | Voucher "{Voucher_Code}" applied! You saved {Discount_Amount} VND. |
| MSG101 | Invalid or expired voucher code. Please check code or minimum order condition. |
| MSG102 | Minimum order spend of {Min_Amount} VND required to use this voucher. |
| MSG103 | Voucher removed from current order. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Selected Tour and current departure/capacity/price loaded; one participant section shown for quantity 1.

### Loading State

Availability/price recalculation or Booking creation; relevant actions disabled.

### Loaded State

Inputs and authoritative read-only summary ready.

### Empty State

Not applicable; entry requires selected Tour.

### Validation Error State

MSG01/MSG78 inline; MSG77 for quantity; MSG101/MSG102 for voucher.

### Business Error State

Tour no longer bookable/capacity changed; no Booking created. Expired pending Booking uses MSG80.

### System Error State

MSG127; no false Booking or capacity reservation.

### Success State

Booking code/status and expiry shown; MSG79 toast; optional Pay Now enters UC-28.

### Confirmation State

Traveler reviews the authoritative summary and explicitly confirms Booking.

### Disabled Action State

Confirm disabled with invalid inputs or while submitting; read-only price fields never editable.

### Offline State

Creation requires network; MSG127 and form state retained.

## Navigation Map

Tour Details → Tour Booking Confirmation → Pending Payment → UC-28 or Booking Details.

## Open Questions

No extra participant fields are introduced.

## UX Suggestions

Use collapsible participant cards for multiple passengers and a sticky read-only total/Confirm bar.

