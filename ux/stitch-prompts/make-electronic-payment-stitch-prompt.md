# Product Context

TripMate UC-28 supports electronic payment on Flutter Mobile and responsive Next.js Web. This Stitch artifact covers Mobile UI only. TripMate owns the summary and verified result screens; VNPay/PayOS owns the external payment UI.

# Screen Objective

Design TripMate’s mobile **Electronic Payment** screen across payment-ready, redirecting, return-processing, and verified result states without treating a deep link as success.

# Target User

Authenticated Traveler paying an eligible owned Booking.

# Screen Content

Booking summary, authoritative read-only amount, configured gateway, Pay Now, redirecting, processing/verifying, verified success, failed/cancelled, timeout, duplicate, amount-verification, generic failure.

# Required Components

Material 3 app bar, Booking card, trusted amount block, gateway card, Pay Now CTA, full-screen processing state, result status card, View Booking action, exact message components.

# Required States

Ready, initiating/disabled, redirecting, external-handoff context, return-processing, verified success, failure/cancelled, timeout, duplicate, amount mismatch, unverified/generic failure.

# Navigation Context

TripMate → External Payment Gateway → mobile deep link → TripMate → gateway verification → result → Booking Details.

# UX Constraints

390×844 mobile-first, CR-07/CR-08, no amount input, no external gateway website design, no deep-link success, no manual refund/wallet/crypto.

# Open Questions

Configured gateway may be VNPay or PayOS; use MSG85 only for VNPay.

# Stitch Prompt

Design a connected set of Material Design 3 Flutter mobile states for one TripMate screen named **“Electronic Payment”**. Actor: authenticated Traveler. Purpose: hand an eligible Booking to an external gateway and display only a TripMate-verified outcome.

Use a 390×844 viewport.

## Payment Ready

Top to bottom:

1. Top app bar with Back and title “Electronic Payment”.
2. Booking card for **Da Nang Heritage & Coast Day Tour**, Booking code, **26/08/2026 · 08:00**.
3. Large authoritative read-only amount **850,000 VND**; it must not look editable.
4. Configured Payment Gateway card, e.g. VNPay.
5. Full-width primary **“Pay Now”**.

## Redirecting

Disable Pay Now and show exact toast **“Redirecting to secure VNPay payment gateway...”**. Show only a handoff status; do not design the external VNPay/PayOS website.

## Return Processing

After mobile deep link return, show a dedicated processing state with progress indicator and clear “Verifying payment” status. Do not show success merely because the app returned.

## Verified Result variants

- Verified success only: green verified status, updated confirmed Booking summary, exact toast **“Payment successful! Your booking is confirmed and e-tickets have been issued.”**, primary “View Booking”.
- Failed/cancelled: exact alert **“Payment was cancelled or failed. Please try again or choose another payment method.”**
- Duplicate: exact inline text **“This booking has already been paid. Duplicate payment blocked.”**
- Timeout: exact alert **“Payment gateway is taking longer than usual. Please check your banking app before retrying.”**
- Amount mismatch: exact inline text **“Transaction rejected due to invalid payment amount verification.”**
- Generic/unverified failure: never show paid; exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Keep payment and security data privacy-minimal. Do not show full payment instrument data, transaction tokens, external gateway form fields, manual refund, in-app wallet, crypto, or editable amount.

