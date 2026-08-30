# MVP Goal

Allow a Guest or Traveler to browse and submit searches for approved, publicly available tours.

# Target User

Guest or authenticated Traveler on Flutter Mobile or responsive Next.js Web.

# Core User Problem

The user needs to find relevant public tours by destination, date, price, interests, and supported criteria before viewing details.

# Core User Journey

Open Tours → see public tours → enter optional criteria → submit → validate → show matching paginated results or exact empty/error state → select Tour Details.

# Must Have

- Canonical Flutter Mobile and responsive Next.js Web support; current Stitch artifact is Mobile UI only.
- Display only approved/publicly available tours.
- Search on submit, never every keystroke.
- Support destination, travel date/date range, min/max price where provided, interest tags, other supported filters, and sort.
- Validate date ordering and min/max amounts inline.
- CR-01: 20 per page, total count, preserve page/filters/sort after detail return.
- Tour cards include enough identity, price, and current availability summary to select a result.
- Use MSG64 for no matches, MSG65 for unavailable tour, MSG127 for failure.
- Selecting a result enters UC-26; no Booking is created.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Booking creation.
- Recommendations.
- Editing tours.
- Desktop-specific Stitch output.

# MVP User Journey

Browse or submit filters; see a mobile-first result list with traceable Web support noted; open Tour Details.

# Dependencies

- Approved/public tour catalogue and current availability summary.
- Report 3 BR-56, BR-60, CR-01 to CR-04, CR-07 to CR-12, CR-15, MSG01, MSG64, MSG65, MSG127.

# Risks

- Stale or non-public tours appearing.
- Search-on-type behavior would violate CR-02.
- Losing list state after returning from details would violate CR-01.

# Open Questions

“Other Supported Filters” are not enumerated; no extra filters are invented.

