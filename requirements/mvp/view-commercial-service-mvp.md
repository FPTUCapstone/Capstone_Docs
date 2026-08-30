# MVP Goal

Allow a Traveler to search, browse, and inspect commercial-service POIs in the three finalized categories without creating a service Booking.

# Target User

Authenticated Traveler on Flutter Mobile.

# Core User Problem

The Traveler needs to find and understand relevant Hotel, Vehicle Rental, or Restaurant POIs and optionally continue to the separate booking UC.

# Core User Journey

Open Commercial Services → submit optional category/location/date/price filters → see paginated matching POIs → select POI → view stored details and qualified availability → optionally continue to UC-31.

# Must Have

- Flutter Mobile only.
- Two connected screens: Commercial Services Search & List; Commercial Service Details.
- Treat services only as active POIs in the centralized POI catalogue.
- Supported categories are exactly Hotel, Vehicle Rental, Restaurant.
- Search on submit; validate date/time and min/max price inline.
- CR-01 list behavior: 20 per page, total count, preserved page/filters/sort.
- List/detail data may include POI name, category, description, location/coordinates, address, hours, photos, commercial attributes, price/range, requested date/time, and availability where reliably known.
- Never present unverified availability as confirmed.
- Use MSG128 for no matches, MSG127 for retrieval failure, MSG01 for missing required input where applicable.
- Details may offer a CTA to UC-31; UC-30 creates no Booking.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Separate Service/Provider entity, ID, catalogue, or marketplace.
- Categories beyond Hotel, Vehicle Rental, Restaurant.
- Commercial-service Booking flow (UC-31).
- User-created providers.

# MVP User Journey

Traveler searches the three POI categories, opens POI details, and may navigate to UC-31 without booking inside UC-30.

# Dependencies

- Active centralized POI catalogue and available commercial attributes.
- Report 3 BR-87; BR-88/BR-89 only for the handoff to UC-31; CR-01 to CR-04, CR-07 to CR-13, CR-15, MSG01, MSG125 to MSG129.

# Risks

- Introducing a separate service/provider model.
- Showing unreliable availability as confirmed.
- Accidentally embedding UC-31 booking controls.

# Open Questions

“Other supported filters” and category-specific attributes are not enumerated; no additional categories or entities are invented.

