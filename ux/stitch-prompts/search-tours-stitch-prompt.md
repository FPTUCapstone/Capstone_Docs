# Product Context

TripMate UC-24 supports public Tour search on Flutter Mobile and responsive Next.js Web. This Stitch artifact intentionally covers Mobile UI only because the current repository workflow is mobile-first.

# Screen Objective

Design the mobile **Search Tours** screen with submit-based search, filter state, paginated public results, and exact empty/error behavior.

# Target User

Guest or Traveler.

# Screen Content

Destination/date/price/interests criteria, Search CTA, filters/sort, total count, Tour cards with price and availability, mobile pagination, messages.

# Required Components

Material 3 app bar, search field, filter chips/bottom sheet, date/range and VND inputs, submit button, sort control, result count, image cards, status chips, pagination/load-page control, skeleton, empty/error view.

# Required States

Initial public list, criteria ready without auto-search, loading/disabled, results, inline validation, MSG64 empty, unavailable Tour, system failure.

# Navigation Context

Home/Tours → Search Tours → Tour Details → preserved Search Tours.

# UX Constraints

390×844 mobile-first, CR-01 20/page/total/preserved state, CR-02 search only on submit, CR-07/CR-08, no booking creation, no desktop composition in this prompt.

# Open Questions

No additional filters beyond Report 3 supported criteria.

# Stitch Prompt

Design a Material Design 3 Flutter-oriented mobile screen named **“Search Tours”** for TripMate. Actor: Guest or Traveler. Purpose: browse and submit searches for approved public Tours, then open Tour Details.

Use a 390×844 viewport. Top to bottom:

1. Top app bar titled “Tours”.
2. Destination search field with sample **Da Nang**, but do not search while typing.
3. Horizontal filter chips for Date, Price, Interests, and Sort. Opening filters may use a mobile bottom sheet with date range, minimum/maximum VND, and supported interest tags.
4. A clear full-width or prominent “Search” submit action.
5. Result header such as “24 tours” and active-filter chips.
6. Vertical Tour cards using Central Vietnam content:
   - **Da Nang Heritage & Coast Day Tour**
   - destination image
   - Tour Operator summary
   - **26/08/2026 · 08:00**
   - **850,000 VND**
   - **3 seats left** status chip
7. Mobile-appropriate pagination preserving 20 records per page, current page, filters, and sort.

Show variants:

- Initial: first 20 public Tours.
- Criteria-ready: typed filters visible but results unchanged until Search is submitted.
- Loading: card skeletons and Search disabled.
- Inline invalid date/price state.
- Empty: exact text **“No tour packages found matching your destination and dates.”**
- Tour unavailable: exact text **“This tour package is currently unavailable for booking.”**
- Failure: exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

Selecting a card navigates to **Tour Details** and returning restores list state. Do not add booking creation, auto-search-on-keystroke, wishlist, social controls, or generic dashboard styling.

