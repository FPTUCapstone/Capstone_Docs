# Screen Inventory

1. **Electronic Payment** — TripMate-owned pre-payment, return-processing, and verified-result states for UC-28.
2. External VNPay/PayOS UI is explicitly not a TripMate screen and is not specified.

# Screen: Electronic Payment

## Related UC

UC-28 — Make Electronic Payment

## Actor

Traveler (TripMate screen); External Payment Gateway (external handoff only)

## Platform

Flutter Mobile and responsive Next.js Web. Current Stitch output covers Flutter Mobile UI only.

## Purpose

Show the eligible Booking and authoritative payable amount, hand off to the configured gateway, then show only a TripMate-verified result.

## Entry Conditions

Authenticated Traveler owns an eligible unpaid Booking.

## Entry Points

Pending Booking/Booking Details → **Pay Now**.

## Exit / Navigation

TripMate → external gateway → mobile deep link/web redirect → TripMate processing → verified result → Booking Details.

## Required Data

Booking ID/code/summary; Traveler ownership; authoritative amount/currency; gateway; Transaction ID/type/status; gateway reference/response; transaction timestamp; Booking payment status.

## Main Layout Regions

1. Top app bar.
2. Booking summary.
3. Authoritative read-only payment amount.
4. Configured gateway card.
5. Pay Now CTA.
6. Redirecting/processing panel.
7. Verified result panel and next navigation.

## Components

### User Inputs

None. The configured gateway and authoritative amount are read-only; there is no amount input.

### Read-only Information

Booking summary; total amount; currency; gateway; transaction/reference/status after verification.

### Primary Actions

- **Pay Now**.
- **View Booking** after result.
- Retry only when the verified/current state permits.

### Secondary Actions

Back before request.

## Validation

Owned eligible Booking; not already paid; amount equals authoritative Booking amount; client/deep link never determines success; verified gateway result/IPN required. Apply CR-13.

## Business Rules

BR-69 to BR-75. Apply CR-07 to CR-13 and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG85 | Redirecting to secure VNPay payment gateway... |
| MSG86 | Payment successful! Your booking is confirmed and e-tickets have been issued. |
| MSG87 | Payment was cancelled or failed. Please try again or choose another payment method. |
| MSG88 | This booking has already been paid. Duplicate payment blocked. |
| MSG89 | Payment gateway is taking longer than usual. Please check your banking app before retrying. |
| MSG92 | Transaction rejected due to invalid payment amount verification. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |

## States

### Initial State

Eligible Booking summary and authoritative amount loaded; Pay Now enabled.

### Loading State

Eligibility/request creation. After deep link/redirect, show Processing/Verifying without success.

### Loaded State

Payment-ready state.

### Empty State

Not applicable; entry requires a Booking.

### Validation Error State

Amount verification failure uses MSG92.

### Business Error State

Already paid uses MSG88; ineligible Booking blocks Pay Now.

### System Error State

Gateway timeout uses MSG89; failed/cancelled uses MSG87; other failures use MSG127. Never show success.

### Success State

Only after verified gateway confirmation: MSG86 and confirmed Booking status.

### Confirmation State

TripMate payment summary serves as pre-handoff review; no extra irreversible confirmation is defined.

### Disabled Action State

Pay Now disabled while request is active, for already-paid/ineligible Booking, or while verifying return.

### Offline State

No payment initiation/verification completion; MSG127 or MSG89 as applicable.

## Navigation Map

Booking → Electronic Payment → External Gateway → deep link/redirect → Processing → Verified Result → Booking Details.

## Open Questions

Configured gateway may be VNPay or PayOS. MSG85 applies where VNPay is used.

## UX Suggestions

Use a single trusted amount block and a full-screen processing state after return; do not imitate gateway branding beyond a gateway name/logo card.
