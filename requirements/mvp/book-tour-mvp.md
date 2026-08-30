# MVP Goal

Allow an authenticated Traveler to create one eligible Tour Booking with validated capacity, participant data, optional voucher, and TripMate-calculated amounts.

# Target User

Authenticated Traveler on Flutter Mobile or responsive Next.js Web.

# Core User Problem

The Traveler needs to reserve the correct departure and seats with valid participant details and a transparent, authoritative total before payment.

# Core User Journey

Open eligible Tour → select departure/quantity → enter participants → optionally apply voucher → revalidate → review read-only price/discount/total/expiry → confirm Booking → reserve capacity → MSG79 → optionally continue to UC-28.

# Must Have

- Canonical Flutter Mobile and responsive Next.js Web support; current Stitch artifact is Mobile UI only.
- Show selected Tour and departure information.
- Requested slot/passenger quantity and participant full name plus valid ID/Passport information.
- Optional voucher input with apply/remove outcomes.
- Revalidate eligibility and capacity at confirmation.
- TripMate calculates base/unit price, discount, total, and payment expiration; all monetary values are read-only and never trusted editable inputs.
- Require explicit Booking confirmation.
- Create Booking separately from payment, reserve capacity, and show MSG79.
- Show MSG77, MSG78, MSG80, MSG100 to MSG103, and MSG127 as applicable.
- Prevent double submission; money/date/time formatting follows CR-07/CR-08.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Electronic gateway payment UI (UC-28).
- Editing authoritative calculated amounts.
- Tour discovery/details.
- Desktop-specific Stitch output.

# MVP User Journey

Traveler enters only trusted booking inputs, reviews calculated values, confirms, and receives a pending-payment Booking with 15-minute expiration where required.

# Dependencies

- Eligible Tour/departure, current capacity, Traveler session, participant validation, voucher validation.
- Report 3 BR-61 to BR-64, BR-68, BR-97 to BR-100 where applied, CR-03 to CR-13, CR-15, MSG01, MSG77 to MSG80, MSG100 to MSG103, MSG125 to MSG127.

# Risks

- Capacity race between review and confirmation.
- Editable or client-trusted monetary values.
- Booking and payment being incorrectly treated as one operation.

# Open Questions

No extra participant fields beyond finalized required information are invented.

