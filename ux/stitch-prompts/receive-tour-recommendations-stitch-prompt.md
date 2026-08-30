# Product Context

TripMate UC-25 supports Tour recommendations on Flutter Mobile and responsive Next.js Web. This Stitch artifact covers Mobile UI only. Only approved, available Tours with similarity score strictly greater than 80% qualify.

# Screen Objective

Design a ranked mobile **Tour Recommendations** list with visible score qualification and exact empty/error states.

# Target User

Authenticated Traveler with sufficient criteria/preferences.

# Screen Content

Personalization context, ranked Tour cards, >80% match score, price/availability, total/pagination, exact messages.

# Required Components

Material 3 app bar, small context header, ranked image cards, score badges, price/capacity chips, total count, pagination, skeleton and empty/error components.

# Required States

Loading, qualified results, no results/insufficient criteria, unavailable exclusion, failure, disabled list while loading.

# Navigation Context

Traveler Home/Tours → Tour Recommendations → Tour Details → preserved recommendations.

# UX Constraints

Mobile-first 390×844, >80% not >=80%, CR-01 list behavior, English UI, VND formatting, no algorithm-edit controls or booking creation.

# Open Questions

Exact criteria-sufficiency rule is not defined.

# Stitch Prompt

Design a Material Design 3 Flutter-oriented mobile screen named **“Tour Recommendations”** for TripMate. Actor: authenticated Traveler. Purpose: show ranked approved and available Tour recommendations where similarity score is strictly greater than 80%.

Use a 390×844 viewport. Top to bottom:

1. Top app bar with Back and title “Tour Recommendations”.
2. Compact personalization header indicating results are based on travel style/preferences, without exposing algorithm details.
3. Result count and preserved page state.
4. Ranked vertical Tour cards with destination imagery and compact metadata. First card:
   - rank #1
   - **Da Nang Heritage & Coast Day Tour**
   - **92% match** badge
   - exact supporting message **“Matching score: 92% based on your travel style and preferences.”**
   - **850,000 VND**
   - **26/08/2026 · 08:00**
   - **3 seats left**
5. Additional cards must all show scores above 80%; do not show 80%.
6. Mobile pagination with 20 records per page behavior.

Variants:

- Loading card skeletons.
- Qualified ranked results.
- Empty/no qualified result with exact text **“No records found matching your criteria.”**
- Failure with exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Selecting a card opens **Tour Details** and returning restores list state. Do not include unavailable Tours, a tour at exactly 80%, algorithm controls, wishlist, social features, or booking creation.

