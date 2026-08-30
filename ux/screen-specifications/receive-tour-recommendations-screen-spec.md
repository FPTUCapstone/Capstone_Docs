# Screen Inventory

1. **Tour Recommendations** — ranked recommendation list for UC-25.

# Screen: Tour Recommendations

## Related UC

UC-25 — Receive Tour Recommendations

## Actor

Traveler

## Platform

Flutter Mobile and responsive Next.js Web. Current Stitch output covers Flutter Mobile UI only.

## Purpose

Display ranked, eligible public Tours whose similarity score is strictly greater than 80%.

## Entry Conditions

Authenticated Traveler has sufficient criteria and/or saved preferences.

## Entry Points

Traveler Home or Tours → Recommendations.

## Exit / Navigation

Select recommendation → Tour Details. Back restores list state.

## Required Data

Traveler criteria/preferences availability; Tour ID/title/image/operator; relevant price/availability summary; similarity score; rank; total count; current page.

## Main Layout Regions

Top app bar; compact personalization context; ranked Tour card list; score badges/message; total/pagination; empty/error area.

## Components

### User Inputs

Page/list navigation; Tour selection.

### Read-only Information

Tour identity, price/availability summary, rank, and exact score message.

### Primary Actions

Select a recommended Tour.

### Secondary Actions

Change page; back.

## Validation

Authenticated Traveler; sufficient criteria; approved/public and available Tours only; score must be **> 80%**, not >=80%. Apply CR-01 list behavior and CR-07/CR-08.

## Business Rules

BR-57, BR-58, BR-60. Apply CR-01, CR-07 to CR-12, and CR-15.

## Application Messages

| Code | Locked content |
|---|---|
| MSG68 | Matching score: {Score}% based on your travel style and preferences. |
| MSG125 | Your session has expired. Please sign in again to continue. |
| MSG127 | TripMate is temporarily unable to process your request. Please check your connection and try again. |
| MSG128 | No records found matching your criteria. |

## States

### Initial State

Recommendation context and loading shell.

### Loading State

Ranked card skeletons; navigation disabled.

### Loaded State

Qualified ranked results with MSG68 score content.

### Empty State

MSG128 for insufficient/no eligible/no >80% results where applicable.

### Validation Error State

Not applicable to editable inputs on this screen.

### Business Error State

Unavailable Tour excluded; if it becomes unavailable after display, it cannot continue as bookable.

### System Error State

MSG127; previous state unchanged.

### Success State

Loaded ranked recommendations; no Booking modified.

### Confirmation State

Not required.

### Disabled Action State

Unavailable result selection disabled; list actions disabled while loading.

### Offline State

MSG127.

## Navigation Map

Traveler Home → Tour Recommendations → Tour Details → preserved Recommendations.

## Open Questions

Exact criteria sufficiency rules are not defined.

## UX Suggestions

Use a clear score badge such as “92% match”, but retain exact MSG68 text in detail/supporting copy.

