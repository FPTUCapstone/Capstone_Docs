# MVP Goal

Show an authenticated Traveler ranked, eligible Tour recommendations only when similarity score is strictly greater than 80%.

# Target User

Authenticated Traveler with sufficient search/travel criteria and/or saved preferences.

# Core User Problem

The Traveler needs a focused list of tours matched to their travel style without seeing unavailable or under-threshold results.

# Core User Journey

Open recommendations → obtain criteria/preferences → score eligible public tours → keep score > 80% → rank → display result cards and exact score message → open Tour Details.

# Must Have

- Canonical Flutter Mobile and responsive Next.js Web support; current Stitch artifact is Mobile UI only.
- Require authenticated Traveler and sufficient matching criteria.
- Evaluate only approved/publicly available Tours.
- Qualification is **Similarity Score > 80%**, never >= 80%.
- Exclude unavailable tours regardless of score.
- Rank qualified results and show each score using MSG68.
- Apply CR-01 list behavior.
- Use MSG128 for no eligible/qualified results and MSG127 for processing failure.
- Selecting a recommendation enters UC-26; no Booking is created.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Explaining or editing the algorithm.
- Showing tours at exactly 80% or below.
- Booking creation.
- Desktop-specific Stitch output.

# MVP User Journey

Traveler sees a ranked mobile list of >80% eligible tours and selects one for details.

# Dependencies

- Sufficient Traveler criteria/preferences, eligible tour catalogue, Tour Matching Algorithm.
- Report 3 BR-57, BR-58, BR-60, CR-01, CR-07 to CR-12, CR-15, MSG68, MSG125 to MSG128.

# Risks

- Implementing >=80% instead of >80%.
- Recommending an unavailable Tour.

# Open Questions

The finalized source does not define the exact sufficiency threshold for input criteria; the screen only represents available/insufficient result states.

