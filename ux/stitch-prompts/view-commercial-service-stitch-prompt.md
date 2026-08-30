# Product Context

TripMate UC-30 represents commercial services only as POIs in the centralized POI catalogue. Supported categories are exactly Hotel, Vehicle Rental, and Restaurant. It is Flutter Mobile only.

# Screen Objective

Design two connected mobile screens: **Commercial Services Search & List** and **Commercial Service Details**, with a clean handoff to UC-31 but no Booking flow.

# Target User

Authenticated Traveler finding a commercial-service POI.

# Screen Content

Submit-based search/filter, exact-category chips, POI cards and pagination; then POI detail, location, hours, price/availability where trustworthy, and optional Book Service navigation to UC-31.

# Required Components

Material 3 app bars, search field, category chips, filter sheet, result count, image-led POI cards, pagination, detail hero, address/map card, hours/attributes, price/availability, CTA, empty/error states.

# Required States

List initial/loading/results/inline validation/empty/failure; detail loading/loaded/inactive/unverified availability/failure/disabled CTA.

# Navigation Context

Commercial Services → Search & List → Details → optional UC-31; Back restores list state.

# UX Constraints

390×844 Flutter mobile only, CR-01/CR-02, exact three categories, POI terminology, no Service/Provider entity, no booking implementation inside UC-30.

# Open Questions

Category-specific attributes and other supported filters are not exhaustively defined.

# Stitch Prompt

Design two connected Material Design 3 Flutter mobile screens for TripMate UC-30. Actor: authenticated Traveler. Use 390×844 viewports.

## Screen 1 — Commercial Services Search & List

Top to bottom:

1. Top app bar titled **“Commercial Services”**.
2. Location search field with sample **Da Nang**; search executes only when submitted.
3. Exactly three category chips: **Hotel**, **Vehicle Rental**, **Restaurant**. Add no others.
4. Filter chips/sheet for date/time and minimum/maximum price where applicable.
5. Prominent “Search” submit action.
6. Result header with total count.
7. Image-led POI cards, for example:
   - **My Khe Beachfront Hotel** — Hotel — Da Nang — **1,250,000 VND**
   - **Da Nang City Car Rental** — Vehicle Rental — **850,000 VND**
   - **Hoi An Riverside Restaurant** — Restaurant
8. Mobile pagination preserving 20 records per page, filters, sort, and page.

States:

- Initial first 20 active commercial POIs.
- Loading skeletons and disabled Search.
- Inline missing input exact text **“This field is required.”** where applicable.
- Empty exact text **“No records found matching your criteria.”**
- Failure exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**

## Screen 2 — Commercial Service Details

Top to bottom:

1. Image hero with Back and exact title **“Commercial Service Details”**.
2. POI name and one of the exact three category chips.
3. Description.
4. Address/location card and small map thumbnail.
5. Opening hours where applicable.
6. Stored category-specific commercial attributes.
7. Price or price range where available.
8. Availability status only when TripMate can reliably determine it.
9. Optional full-width “Book Service” CTA that navigates to separate UC-31.

Detail states:

- Loading with CTA disabled.
- Loaded trustworthy details.
- Inactive/unavailable POI with disabled CTA.
- Availability unknown: withhold confirmed-availability styling and disable the CTA where required; do not invent a new system message.
- Failure with the exact MSG127 text above.

Use POI language and IDs conceptually; do not introduce Service, Service ID, Provider, Provider ID, provider marketplace, user-created providers, new categories, or any booking form/payment controls. UC-30 ends when the Traveler views details or navigates to UC-31.
