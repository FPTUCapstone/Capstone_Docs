# MVP Goal

Allow a Traveler to pay an eligible Booking through the configured external gateway while TripMate displays and trusts only its authoritative verified result.

# Target User

Authenticated Traveler on Flutter Mobile or responsive Next.js Web.

# Core User Problem

The Traveler needs a safe handoff to payment and an unambiguous verified result without duplicate payment or client-controlled amount.

# Core User Journey

Open eligible unpaid Booking → review authoritative amount/gateway → Pay Now → create Pending transaction → external gateway → deep link/redirect back → TripMate verifies gateway result/IPN → show verified result state.

# Must Have

- Canonical Flutter Mobile and responsive Next.js Web support; current Stitch artifact is Mobile UI only.
- TripMate Electronic Payment screen shows Booking summary, authoritative amount/currency, configured gateway, and Pay Now.
- Disable duplicate submission and reject already-paid Booking with MSG88.
- Amount is read-only and controlled by TripMate; mismatch uses MSG92.
- Show redirecting state with MSG85 where VNPay is used.
- Do not design the VNPay/PayOS external website as a TripMate screen.
- Mobile path: TripMate → external gateway → deep link → TripMate → gateway result verification → result state.
- Deep link/redirect alone is never authoritative success.
- Include processing, verified success (MSG86), failure/cancelled (MSG87), timeout (MSG89), duplicate-payment, verification failure, and generic failure states.
- Only verified success updates Booking paid/confirmed state.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- External gateway UI.
- Manual refund.
- In-app wallet or crypto.
- Trusting client amount or deep-link success.
- Desktop-specific Stitch output.

# MVP User Journey

Traveler reviews the TripMate-controlled payment summary, leaves for the gateway, returns, waits for verification, and sees the exact verified outcome.

# Dependencies

- Eligible owned Booking, configured gateway, authoritative amount, verified gateway result/IPN.
- Report 3 BR-69 to BR-75, CR-07 to CR-13, CR-15, MSG85 to MSG89, MSG92, MSG125 to MSG127.

# Risks

- Treating a deep link as success.
- Duplicate payment or amount tampering.
- Persistence failure after verified gateway response requiring reconciliation.

# Open Questions

The configured gateway may be VNPay or PayOS; MSG85 is specifically used where VNPay is used.

