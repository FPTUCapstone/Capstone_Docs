# Product Context

TripMate UC-27 supports Tour Booking on Flutter Mobile and responsive Next.js Web. This Stitch artifact covers Mobile UI only. Booking is created independently before payment.

# Screen Objective

Design the mobile **Tour Booking Confirmation** screen with trusted participant inputs, optional voucher, read-only TripMate-calculated money, 15-minute payment expiry, explicit confirmation, and pending-payment success.

# Target User

Authenticated Traveler booking an eligible Tour.

# Screen Content

Selected Tour/departure, passenger quantity, participant forms, voucher, read-only base/discount/total, expiration, Confirm Booking, pending-payment result and Pay Now handoff.

# Required Components

Material 3 app bar, Tour summary, departure control, quantity stepper, repeated participant cards, voucher field/apply/remove, read-only price breakdown, expiration info, sticky CTA, inline errors, toast/alert states.

# Required States

Initial/loaded, recalculating, validation errors, voucher outcomes, capacity error, confirmation review, creating/disabled, success/pending payment, expired, system failure.

# Navigation Context

Tour Details → Tour Booking Confirmation → Pending Payment → UC-28 or Booking Details.

# UX Constraints

390×844 mobile-first, English UI, CR-07/CR-08, monetary values never editable, no gateway UI, block double submission.

# Open Questions

No additional participant fields beyond finalized requirements.

# Stitch Prompt

Design a Material Design 3 Flutter-oriented mobile screen named **“Tour Booking Confirmation”** for TripMate. Actor: authenticated Traveler. Purpose: create a Booking for an eligible Tour using valid capacity and participant inputs, with TripMate-calculated read-only amounts.

Use a 390×844 viewport. Top to bottom:

1. Top app bar with Back and exact title.
2. Selected Tour card: **Da Nang Heritage & Coast Day Tour**, operator, image.
3. Departure selector showing **26/08/2026 · 08:00**.
4. Passenger quantity stepper with availability text **3 seats left**.
5. Participant card(s), one per selected slot, with required “Full name” and “ID / Passport number” fields.
6. Optional voucher field with “Apply” and applied/removable state.
7. Read-only price breakdown:
   - Base price: **850,000 VND**
   - Quantity
   - Discount
   - Total amount
   These must look non-editable.
8. Payment expiration information: complete payment within 15 minutes.
9. Sticky full-width **“Confirm Booking”** CTA.

Show variants:

- Initial/loaded.
- Inline participant error with exact text **“Passenger full name and valid ID/Passport number are required.”**
- Capacity error with exact text **“Only 3 seats left for this tour. Please reduce passenger count.”**
- Voucher success exact toast **“Voucher "DANANG10" applied! You saved 85,000 VND.”**
- Invalid voucher exact text **“Invalid or expired voucher code. Please check code or minimum order condition.”**
- Minimum spend exact text **“Minimum order spend of 1,000,000 VND required to use this voucher.”**
- Voucher removed exact toast **“Voucher removed from current order.”**
- Creating: Confirm disabled with progress.
- Success/pending payment: Booking code and read-only summary, exact toast **“Booking created! Please complete payment within 15 minutes to secure your seats.”**, plus primary “Pay Now” leading to UC-28.
- Expired: exact alert **“Your booking reservation has expired due to non-payment within 15 minutes. Seats released.”**
- Failure: exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Do not design price/discount/total as editable fields. Do not include external payment-gateway UI or mark payment completed here.

