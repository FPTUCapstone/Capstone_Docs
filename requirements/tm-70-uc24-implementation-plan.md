# TM-70 — UC-24 Search Tours V2 Implementation Plan

## Document Control

| Field | Value |
|---|---|
| Status | **APPROVED BASELINE — IMPLEMENTATION NOT STARTED** |
| Plan date | 2026-10-07 |
| Contract | `requirements/tm-70-uc24-search-tours-v2-addendum.md` |
| Delivery method | Dependency-ordered TDD |

## 1. Goal

Deliver the complete Report 3 V2 UC-24 contract on Backend, responsive Web, and
Flutter Mobile without regressing the approved TM-70/TM-208/TM-209 behavior.

Every implementation task follows:

1. write a focused failing test;
2. confirm the expected RED reason;
3. make the smallest production change;
4. run focused tests;
5. run the relevant regression suite;
6. record evidence before the task is complete.

No phase may claim completion when a required test is skipped because its
environment is unavailable.

## 2. Existing Functionality to Preserve

The implementation extends rather than unnecessarily rewrites:

- Approved/Public Tour filtering;
- anonymous search for Guest and Traveler;
- Destination Unicode NFC normalization;
- existing price validation;
- nullable `thumbnailUrl` supplied by TM-208;
- TM-209 real-thumbnail and neutral-fallback behavior;
- Remaining Slots;
- `availabilityStatus` behavior;
- public response `no-store` behavior;
- page metadata and default page size 20;
- stale-result preservation;
- requested-versus-displayed criteria separation;
- retry behavior;
- accessibility improvements;
- field-linked Web validation errors;
- English feature resource files.

Regression tests must protect each item throughout the phases below.

## 3. Delivery Constraints

- The approved addendum is the UC-24 implementation contract.
- No client owns or hardcodes the category taxonomy.
- No POI image or runtime mock is used as a Tour thumbnail.
- `Title ASC, TourId ASC` is retained only as a deterministic tie-break.
- Mobile infinite-scroll-only behavior must not remain the sole pagination UI.
- Rating functionality may reuse TM-211 infrastructure but cannot be omitted
  from UC-24 acceptance.
- Product/BA approval of production category seeds remains a deployment gate,
  not a reason to hardcode temporary categories.

## 4. Phase 0 — Docs and API Contract Freeze

### Task 0.1 — Freeze the canonical contract

**Tests/checks first**

- Review every addendum section against Report 3 V2 and approved D1–D9.
- Search the addendum for obsolete assumptions: single `departureDate`, rating
  excluded, category excluded, keyword excluded, fixed title-only ordering, or
  Mobile infinite-scroll-only acceptance.

**Production/documentation change second**

- Approve this addendum as the only TM-70 clarification layer over Report 3 V2.
- Record request, response, messages, rules, dependencies, and ACs.

**Regression checks**

- Confirm unrelated V2 requirements were not rewritten.
- Confirm TM-208/TM-209 contracts remain intact.

**Completion gate**

- D1–D9 are traceable and no known old-baseline assumption remains.

### Task 0.2 — Create language-neutral API contract examples in Docs

**Checks first**

- Define canonical valid request examples for all criteria and interactions.
- Define canonical invalid request examples for validation and key handling.
- Define canonical populated, empty, nullable-field, unknown-availability, and
  unrated response examples.
- Define canonical ordering examples for every sort and tie-break.

**Documentation change second**

- Record language-neutral request/response and ordering examples in Docs only.
- Do not change Backend, Web, or Mobile repositories during Phase 0.

**Regression checks**

- Existing TM-70 payloads containing all required nullable fields remain valid.
- Missing required nullable properties remain invalid.

**Completion gate**

- The frozen examples define parameter names, enum values, defaults, ordering,
  and nullable property presence before repository-specific tests and feature
  code are extended.

## 5. Phase 1 — Database and Domain Additions

### Task 1.1 — TourCategory schema contract

**Tests first**

- SQL migration tests for fresh schema, upgrade schema, idempotency, wrong-shape
  rollback, FK enforcement, stable unique code, English name, and active state.
- Domain tests proving one Tour references at most one category during the
  transitional migration and exactly one after the production backfill gate.

**Production change second**

- Add `TourCategory` table/entity/configuration.
- Add the Tour `category_id` FK.
- Use an additive transitional nullable FK for existing data until an approved
  production taxonomy can be seeded and existing Tours mapped.
- Do not create a join table.

**Regression tests**

- Existing Tour search, Tour creation, approval, media, and publication schema
  tests remain green.
- No existing Tour data is deleted or assigned a fabricated category.

**Completion gate**

- Fresh and upgrade SQL paths converge on the expected transitional schema.
- No production category names have been invented.

### Task 1.2 — Production seed and non-null enforcement gate

**DEFERRED PRODUCTION ENFORCEMENT GATE**

After Task 1.1, Task 1.3 and Phase 2 may proceed. Product/BA category seed
approval and legacy backfill are not prerequisites for schema, taxonomy API,
or search-query TDD. Tests may use clearly labelled test-only categories; they
must not be presented as the production taxonomy.

**Tests first**

- Seed idempotency and stable-code tests.
- Backfill mapping coverage for every existing production Tour category.
- Migration test proving `category_id` becomes non-null only after backfill.

**Production change second**

- Add the Product/BA-approved seed list.
- Backfill existing Tours using an explicitly approved mapping.
- Enforce the final non-null FK.

**Regression tests**

- Re-running deployment scripts does not duplicate or rename categories.
- Inactive categories remain stored but are not selectable.

**Completion gate**

- Product/BA seed approval exists and every persisted Tour has exactly one
  valid category.

**Current external gate:** `PENDING PRODUCT/BA SEED APPROVAL`.

This gate must close before final non-null enforcement, production rollout,
and final TM-70 acceptance.

### Task 1.3 — Rating feasibility and index review

**Tests first**

- SQL fixtures for no reviews, one review, multiple reviews, non-Tour reviews,
  hidden/deleted reviews, and decimal means.
- Query-plan baseline for page-bounded search combined with rating filter/sort.

**Production change second**

- Add only the indexes or query projections required to aggregate eligible Tour
  reviews efficiently.
- Do not denormalize an aggregate unless measurements prove it necessary and a
  separate design decision approves consistency handling.

**Regression tests**

- Existing review create/edit/delete behavior and aggregate outcomes remain
  correct.

**Completion gate**

- Eligibility predicate is shared and the measured query plan is acceptable.

## 6. Phase 2 — Backend TDD

### Task 2.1 — Request parser and query model

**Tests first**

- Convert the frozen Docs request examples into Backend typed fixtures.
- RED tests for every new key, duplicate/wrong-case/unknown keys, default sort,
  empty keyword, independent date endpoints, invalid numeric values, and
  invalid page/pageSize.

**Production change second**

- Extend the request parser and application query model with the canonical
  parameters.
- Preserve anonymous access and `no-store`.

**Regression tests**

- Existing destination/price/page requests behave unchanged.

**Completion gate**

- Parser tests prove exact allowlist, defaults, and rejection behavior.

### Task 2.2 — Validator

**Tests first**

- RED tests for MSG157, MSG158, acceptance of either one-sided date, invalid
  two-ended date order, duration minimum, rating scale, active category code,
  sort enum, and page bounds.
- RED price-boundary tests prove non-negative whole-VND values, rejection of
  values outside the selected Backend monetary representation, and the same
  technical limit in validation and OpenAPI.

**Production change second**

- Add field-linked validation and ProblemDetails mapping.

**Regression tests**

- Existing price validation remains green.
- No valid past date is rejected merely for being in the past.

**Completion gate**

- Invalid requests never dispatch the search query.

### Task 2.3 — Keyword normalization and matching

**Tests first**

- Provider-backed SQL tests for Title, Destination, and Operator matches;
  all-token behavior; substring behavior; case differences; accented versus
  unaccented text; and empty keyword.
- Destination-filter tests for trim/NFC, case-insensitive and
  accent-insensitive substring matching, empty omission, and conjunction with
  keyword.
- Ordering tests that choose each token's best class, require every token,
  classify the Tour by its worst token class, and cover a mixed exact-Title +
  Operator example.

**Production change second**

- Implement server-side normalization and SQL-translatable matching.
- Implement the dedicated Destination predicate independently from keyword.
- Order exact Title before Title prefix, Title token/substring, Destination,
  and Operator Name without arbitrary numeric weights; within a class use
  rating-null-last, Title, and TourId tie-breaks.

**Regression tests**

- Existing Destination filtering remains correct when the keyword criterion is
  omitted.
- Query remains page-bounded and executes without N+1 reads.

**Completion gate**

- Real SQL Server tests prove matching and deterministic relevance ordering.

### Task 2.4 — Departure Date Range

**Tests first**

- Authoritative SQL boundary tests on real SQL Server:
  - **Test A (Equal timestamp tie):** Two schedules with identical `departureAtUtc`, different `scheduleId` -> lower stable `scheduleId` selected deterministically by `departureAtUtc ASC, scheduleId ASC`.
  - **Test B (`asOfUtc` exact boundary):** `departureAtUtc == asOfUtc` -> considered upcoming.
  - **Test C (Just before boundary):** `departureAtUtc < asOfUtc` -> not upcoming without explicit past date filter.
  - **Test D (Same request snapshot):** Representative selection, upcoming filtering, real-time availability evaluation, and response envelope `asOfUtc` use the identical captured instant.
  - **Test E (Explicit past search):** Past/completed schedule selected as representative under explicit past filter; Tour visible, detail navigation allowed, booking disabled, capacity status not converted to `soldOut`.
  - **Test F (Equal local date/time ordering):** Final deterministic `scheduleId ASC` tie-break proven by real SQL Server provider tests.
- RED SQL tests for omitted endpoints, start only, end only, inclusive same-day
  range, two-ended range, VN timezone boundaries, past dates, invalid range,
  sold-out schedules, Cancelled/disabled exclusions, and Tours with no matching
  search-discoverable schedule.

**Production change second**

- Capture single request-level `asOfUtc` snapshot once at request start.
- Implement date predicates against search-discoverable schedules using
  `Asia/Ho_Chi_Minh` local calendar boundaries.
- Implement deterministic representative schedule selection ordered by
  `departureAtUtc ASC, scheduleId ASC`.
- Treat past/completed representative schedules as non-bookable (`bookable = false`)
  while preserving underlying capacity status without forced `soldOut`.

**Regression tests**

- Existing representative departure and availability selection remains
  deterministic and conforms to Section 6 of the addendum.

**Completion gate**

- All four endpoint combinations and the six authoritative boundary tests are
  proven with SQL integration tests.

### Task 2.5 — Duration and Category filtering

**Tests first**

- Exact duration match/non-match tests.
- Active category match, inactive category rejection, unknown category, and
  legacy null-category transitional behavior tests.

**Production change second**

- Add duration equality and category-code predicates.

**Regression tests**

- Approved/Public visibility remains the outer eligibility boundary.

**Completion gate**

- Filters combine correctly with keyword, price, and date predicates.

### Task 2.6 — Rating aggregation and filter

**Tests first**

- No-review null, exact mean, unrounded threshold boundary, hidden/deleted
  exclusion, non-Tour review exclusion, one-decimal output, and unrated-filter
  exclusion tests.

**Production change second**

- Add a SQL-translatable eligible-review aggregate and minimum predicate.

**Regression tests**

- Pagination count and item rating use the same eligibility predicate.

**Completion gate**

- Unit and real SQL tests agree on filter and output semantics.

### Task 2.7 — Sorting

**Tests first**

- RED ordering tests for Relevance, Price ASC, Price DESC, Rating, Departure
  Date, empty-keyword fallback, unrated last, missing schedule last, and every
  deterministic tie-break.
- Relevance fixtures prove best-per-token matching, worst-token Tour
  classification, and the approved class precedence.
- Departure fixtures prove earliest matching schedule under a date criterion,
  nearest upcoming schedule without one (using the request-level `asOfUtc`
  snapshot), deterministic schedule tie-break by `departureAtUtc ASC, scheduleId ASC`,
  Cancelled/disabled exclusion, and missing representative schedule last.
- Real SQL Server provider tests proving deterministic departure date ordering
  when multiple Tours have representative schedules departing at the identical
  instant or when schedules within a Tour share identical departure timestamps.

**Production change second**

- Implement selected primary sort and common tie-breaks (`Title ASC, TourId ASC`),
  ensuring `departureDate` sort uses representative schedule departure instant ASC
  with the `scheduleId ASC` tie-break.

**Regression tests**

- Repeated requests over unchanged data return stable pages.

**Completion gate**

- Each sort is proven with deliberately tied fixtures.

### Task 2.8 — DTO and availability projection

**Tests first**

- Convert the frozen Docs response examples into Backend typed fixtures.
- RED serialization tests for mandatory nullable fields, Aggregate Rating,
  unknown availability, null thumbnail, and page envelope metadata including
  `asOfUtc` matching the request snapshot.
- RED availability tests distinguishing capacity status from booking eligibility:
  - Upcoming and available -> `available`, remaining count (> 0), bookable;
  - Upcoming and sold out -> `soldOut`, 0 slots, not bookable;
  - Provider failure -> `unknown`, null slots, retains representative schedule
    ID/time, detail navigation allowed, booking disabled, not marked sold out;
  - Past or completed representative schedule -> capacity status preserved
    (`available` or `soldOut`), remaining slots preserved where known, booking
    disabled (`bookable = false`);
  - No upcoming schedule -> `noUpcomingSchedule`, null schedule ID/time/slots,
    not bookable.

**Production change second**

- Extend the DTO/projection while preserving page-bounded primary-media lookup,
  returning the request-level `asOfUtc` snapshot, and projecting representative
  schedule availability status without confounding capacity with booking
  eligibility.

**Regression tests**

- TM-208 thumbnail rules, Remaining Slots, no POI fallback, and no-store remain
  green.

**Completion gate**

- OpenAPI and runtime JSON expose the same required/nullability contract.

### Task 2.9 — Category taxonomy endpoint

**Tests first**

- Anonymous access, active-only ordering, stable code, English name, and empty
  taxonomy tests.
- Prove exact ordering by English `name ASC`, then `code ASC`.

**Production change second**

- Implement `GET /api/v1/tour-categories` from server-owned taxonomy data.

**Regression tests**

- Inactive categories cannot be selected through search validation.

**Completion gate**

- Web/Mobile can obtain their category controls without local lists.

### Phase 2 completion gate

- Focused unit/API/SQL suites green.
- Full Backend solution build and tests green.
- No SQL test is skipped when it is required for provider semantics.
- Query count and plan show no N+1 or unbounded client evaluation.

## 7. Phase 3 — Web TDD

### Task 3.1 — Typed contract and URL state

**Tests first**

- Convert the frozen Docs request/response examples into Web typed fixtures.
- RED parser/serializer tests for every query parameter, dynamic default sort,
  independent dates, required nullable response fields, browser back/forward,
  and invalid raw parameters.
- Prove Destination trim/NFC serialization, empty omission, and simultaneous
  Keyword + Destination preservation.

**Production change second**

- Extend typed API models, BFF allowlist, URL state, client validation, and API
  parsing.

**Regression tests**

- Existing destination, price, page, thumbnail, and stale-result tests remain
  green.

**Completion gate**

- URL round-trips all criteria without loss or silent correction.

### Task 3.2 — Search controls

**Tests first**

- Component tests for keyword, one-sided/two-sided dates, duration, taxonomy-fed
  category, minimum rating, sort, inline errors, and submit-only behavior.
- Destination control tests cover accent-bearing input, empty normalization,
  and combined Keyword + Destination submission without client-side
  substitution of Backend matching semantics.

**Production change second**

- Add accessible controls using English feature resources.
- Fetch category options from Backend.

**Regression tests**

- Existing focus handling, touch targets, field-linked errors, and accessibility
  semantics remain green.

**Completion gate**

- No hardcoded category or new hardcoded user-facing copy exists.

### Task 3.3 — Results, pagination, and Clear Filters

**Tests first**

- Card tests for rating/null rating, real/null/broken thumbnail, availability
  unknown, booking-disabled semantics, and past/completed representative schedules
  (detail navigation allowed, booking disabled, capacity not presented as sold out).
- Pagination and Clear Filters tests proving keyword retention and default sort.

**Production change second**

- Render Aggregate Rating and preserve current card data.
- Extend numbered pagination and reset behavior.

**Regression tests**

- Stale results, requested/displayed criteria, retry, total count, and Detail
  links remain green.

**Completion gate**

- Web satisfies every Web-applicable AC in the addendum.

### Task 3.4 — Context restoration

**Tests first**

- Integration tests for Search → Detail URL → Back with keyword, every filter,
  sort, and a page greater than 1.

**Production change second**

- Include canonical search context in the detail handoff and restore it on
  return.

**Regression tests**

- Direct Tour Detail links remain valid without search context.

**Completion gate**

- Back returns to the same logical result page without resetting criteria.

### Phase 3 completion gate

- Focused tests, full Web tests, typecheck, lint, and production build green.
- Runtime request matches the frozen Backend contract.

## 8. Phase 4 — Mobile TDD

### Task 4.1 — Typed query/model and repository contract

**Tests first**

- Convert the frozen Docs request/response examples into Mobile typed fixtures.
- RED tests for query serialization, independent dates, dynamic sort default,
  all filters, mandatory nullable response fields, rating parsing, and taxonomy
  parsing.
- Prove Destination trim/NFC serialization, empty omission, and simultaneous
  Keyword + Destination preservation.

**Production change second**

- Extend domain value objects, data models, repository, and use case.

**Regression tests**

- Existing public `skipAuth`, error mapping, thumbnail, and availability parsing
  remain green.

**Completion gate**

- JSON → data model → domain entity preserves the complete contract.

### Task 4.2 — Controls and validation

**Tests first**

- Widget/Cubit tests for keyword, dates, duration, Backend category options,
  minimum rating, sort, inline errors, submit, and Clear Filters.
- Destination widget/Cubit tests cover accent-bearing input, empty
  normalization, and combined Keyword + Destination submission.

**Production change second**

- Extend the search page/filter UI using English resources and accessible
  semantics.

**Regression tests**

- Guest/Traveler rendering, retry, stale warning, and pull-to-refresh remain
  green.

**Completion gate**

- Mobile exposes all V2 criteria without a hardcoded category list.

### Task 4.3 — Explicit pagination

**Tests first**

- RED tests for page selection, previous/next boundaries, current page,
  page-size 20, filter/sort retention, loading, failure, and non-accumulated
  page replacement.

**Production change second**

- Replace the current accumulated infinite-scroll presentation with explicit
  page navigation.
- Optional prefetch must not change visible page semantics.

**Regression tests**

- Pull-to-refresh refreshes the current logical search without duplicate items.

**Completion gate**

- Mobile pagination behavior is contract-equivalent to Web.

### Task 4.4 — Result card and context restoration

**Tests first**

- Card tests for rating/null rating, thumbnail states, unknown availability,
  booking-disabled semantics, past/completed representative schedules (detail
  navigation allowed, booking disabled), and detail accessibility.
- Navigation tests for Search → Detail → Back with full context.

**Production change second**

- Render Aggregate Rating and connect detail navigation while retaining query
  state.

**Regression tests**

- TM-209 thumbnail/fallback and existing neutral placeholder semantics remain
  green.

**Completion gate**

- Mobile satisfies every Mobile-applicable AC in the addendum.

### Phase 4 completion gate

- `dart format` verification, `flutter analyze`, focused tests, and full Flutter
  tests green.

## 9. Phase 5 — Docs Final Reconciliation

**Checks first**

- Search all TM-70 Docs for old source references, single `departureDate`,
  deferred keyword/rating/category/duration/sort, fixed primary ordering, and
  Mobile infinite-scroll-only statements.

**Documentation change second**

- Reconcile MVP scope, Web/Mobile screen specifications, user flows, API
  contracts, coverage matrices, and enrichment notes with the implemented
  contract.
- Record the approved production category seed list after Product/BA approval.

**Regression checks**

- Preserve approved decisions and do not expand TM-212 scope.

**Completion gate**

- No active document contradicts the addendum or runtime behavior.

## 10. Phase 6 — Independent Codex Review

Run at least two independent reviews after implementation:

1. contract/spec and cross-platform parity review;
2. SQL correctness, query performance, security, and regression review.

Each review must use exact repository base/HEAD commits and report findings by
severity. Implementation authors resolve proven findings and rerun affected
quality gates before approval.

Completion gate:

- No unresolved BLOCKER/HIGH finding.
- Any accepted lower-severity deviation is recorded explicitly.

## 11. Phase 7 — Integration, E2E, UAT, and Benchmark

### SQL integration

- Run fresh-schema and upgrade migrations on an identified isolated SQL Server.
- Execute the six authoritative representative-schedule boundary tests on real SQL Server:
  - **Test A (Equal timestamp tie):** Two schedules with identical `departureAtUtc`, different `scheduleId` -> lower stable `scheduleId` selected deterministically by `departureAtUtc ASC, scheduleId ASC`.
  - **Test B (`asOfUtc` exact boundary):** `departureAtUtc == asOfUtc` -> considered upcoming.
  - **Test C (Just before boundary):** `departureAtUtc < asOfUtc` -> not upcoming without explicit past date filter.
  - **Test D (Same request snapshot):** Representative selection, upcoming filtering, real-time availability evaluation, and response envelope `asOfUtc` use the identical captured instant.
  - **Test E (Explicit past search):** Past/completed schedule selected as representative under explicit past filter; Tour visible, detail navigation allowed, booking disabled, capacity status not converted to `soldOut`.
  - **Test F (Equal local date/time ordering):** Final deterministic `scheduleId ASC` tie-break proven by real SQL Server provider tests.
- Exercise accent-insensitive keyword matching, date timezone boundaries,
  accent-insensitive Destination substring matching and Keyword conjunction,
  search-discoverable schedule rules, representative schedule selection,
  category FK/filter, rating aggregate, sort null placement, page count, and
  deterministic ordering.
- Confirm temporary databases are cleaned.

### Web/Mobile E2E

- Execute every AC on both platforms against the same Backend dataset.
- Compare request parameters and visible result order for parity.
- Verify network failure retains previous results.
- Verify Search → Detail → Back context restoration.

### UAT

- Guest and Traveler journeys.
- One-sided and two-sided date filters.
- Unaccented Vietnamese keyword.
- Category taxonomy supplied by Backend.
- Unrated and rated Tours.
- Unknown availability and disabled booking.
- Page navigation and Clear Filters keyword preservation.

### Benchmark

- Record dataset size and indexes.
- Measure combined keyword/date/category/rating/sort queries.
- Confirm the query is server-evaluated, page-bounded, and has no N+1 media,
  schedule, category, or review reads.
- Define and obtain approval for the performance threshold before using the
  benchmark as a release gate.

### Final completion gate

- Backend, Web, and Mobile full regression suites green.
- SQL integration, E2E, UAT, and benchmark have recorded final results.
- Production category seeds approved and deployed.
- Docs reflect exact runtime behavior.
- Independent review approved.

## 12. Related Jira Boundaries

- **TM-208:** Preserve the Backend thumbnail contract.
- **TM-209:** Preserve Web/Mobile thumbnail rendering and neutral fallback.
- **TM-210:** Remains the Tour Detail task; TM-70 only owns context handoff and
  return restoration.
- **TM-211:** May deliver rating infrastructure, but TM-70 cannot pass final ACs
  without Aggregate Rating, Minimum Rating, and Rating sorting.
- **TM-212:** Favorites, recommendations, persona matching, and booking
  implementation remain out of TM-70 scope.

No Jira status or issue content is changed by this plan.

## 13. Phase Evidence Record

For every task, record:

- exact pre-task commit;
- test added first and expected RED reason;
- production files changed;
- focused command and pass/fail/skip count;
- regression command and pass/fail/skip count;
- SQL database identity without credentials, when applicable;
- `git diff --check` result;
- final `git status --short --branch`;
- any deviation or external gate.

Do not mark a phase complete based only on an interrupted command, skipped
required test, or unverified external dependency.
