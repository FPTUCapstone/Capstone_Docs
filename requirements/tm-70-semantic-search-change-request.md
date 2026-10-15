# TM-70 Semantic Search Contract Change Request

| Document control | Value |
|---|---|
| Status | **PROPOSED — PENDING OWNER/PRODUCT/BA APPROVAL** |
| Date | 2026-10-15 |
| Scope | UC-24 Search Tours; proposed keyword matching and relevance replacement |
| Docs branch base | `2ad6916dc1ff4f314c19e13258c088533a93cd9a` |
| Primary authority | `Report3_Software-Requirement-Specification-V2.docx`, §3.5.1 UC-24; [canonical reference](https://docs.google.com/document/d/1WNp9w7HfhMDH4M9vy9HecMYCxpeePL9O/edit) |
| Frozen contract | [V2 addendum](tm-70-uc24-search-tours-v2-addendum.md), [implementation plan](tm-70-uc24-implementation-plan.md) |
| Shared eligibility authority | [Review clarification](tm-70-review-lifecycle-minimal-clarification.md) |
| Required contract approvers | Owner, Product, BA; all approvals pending |

This document requests a future change. No proposal below overrides approved
requirements until explicit approval identifies the accepted decisions and their
effective contract version. Owner preference authorizes exploration and a PoC,
not a production replacement. The canonical SRS, frozen addendum and frozen plan
are unchanged. The PoC is isolated from `GET /api/v1/tours` and uses synthetic data.

## 1. Motivation and authority

The owner selected embedding search as the desired future direction. Semantic
retrieval may recognize Vietnamese intent, synonyms and paraphrases that the
all-token lexical contract cannot match. Improvement is a hypothesis to evaluate,
not an established quality result. UC-25 recommendations and its >80% similarity
rule are separate scope; that threshold must not be imported into UC-24.

Read-only inspection of the local Report 3 V2 DOCX confirms §3.5.1 specifies
keyword and supported filters, Relevance, pagination under CR-01, anonymous
access, Approved/Public visibility and display-time availability. The detailed
all-token classes come from the approved clarification layer, not an assertion
that the SRS originally specified embeddings. MSG64 remains the Tour no-result
message and MSG127 the search/system failure message; MSG157/158 are the frozen
addendum's range corrections. No message renumbering is proposed.

The canonical Google Drive document was also fetched read-only during mission
preflight on 2026-10-15 (document ID `1WNp9w7HfhMDH4M9vy9HecMYCxpeePL9O`, DOCX MIME type,
reported modification `2026-10-07T18:05:12.042Z`). Its UC-24 content agrees with
the local V2 source. Canonical content was not edited.

## 2. Exact frozen contract impact

The frozen baseline is commit `a923a5f3222c572a7fa4eefa82e696a55e554638`.
The addendum and plan retain their approved status; this separate CR does not
edit them or declare any frozen criterion satisfied by semantic similarity.
Read-only Git comparison to this branch's `2ad6916dc1ff4f314c19e13258c088533a93cd9a`
base is empty for both files. Their blobs remain
`3645a6a5c8f1aa5bf8c6b177b8614b8c755cdb94` (addendum) and
`6dcff1ca39ab3493aba7ef11149226fe3134bc91` (plan).

| Frozen reference | Proposed effect; approval still required |
|---|---|
| Addendum §4.1 `keyword`, §5; AC-05 | Replace whitespace all-token substring inclusion with a semantic score predicate. Title/Destination/Operator field scope remains proposed unchanged. A Tour may match without every literal token. |
| Addendum §5; AC-06 | Case/accent-insensitive lexical equivalence is no longer guaranteed by a learned model. Preserve trim/NFC input handling; replace the lexical proof obligation with an explicitly approved Vietnamese case/accent evaluation requirement. No parity is claimed without measurement and acceptance. |
| Addendum §5 match precedence, §10 `relevance`; AC-22 | Replace best-per-token/worst-token five-class order with cosine score DESC and proposed ties in §5 below. Exact Title no longer automatically wins. |
| Addendum §4.1 sort default, §10 defaults; AC-26/27 | Preserve keyword-present `relevance`, keyword-absent `departureDate`, empty-keyword relevance fallback. Only the meaning of non-empty-keyword relevance changes. |
| Addendum §5.1; AC-08 | Preserve independent lexical Destination filter and conjunction; only the keyword conjunct is proposed to become semantic. |
| Addendum §§11,14.1; AC-28/37 | Preserve exact global totals and explicit one-based pages in the recommended production option. Approximate candidate diagnostics in the PoC do not satisfy these ACs. Any approximate public contract requires an additional explicit response/UI decision. |
| Addendum §13; AC-29 | Preserve Clear Filters, keyword retention and default-sort reset; rerun the approved semantic meaning only after rollout approval. |
| Review clarification §§6.2,6.3.2/3,7.2 C7–8; Test R-8 | Relevance class language would require a versioned reconciliation. Published-only aggregation, pre-page filtering and exact totals remain required. No bypass of Review/POI gates. |
| Plan Tasks 2.3/2.7; Phases 3/4 keyword parity, Phase 7 SQL/UAT/E2E | Replace only lexical keyword/class acceptance tests after approval; preserve dedicated Destination, provider-backed filters, parity, ordering and count regressions. |

AC-05, AC-06 and AC-22 cannot be presented as passing unchanged under semantic
search. All other ACs remain in force, with the interactions above explicitly
retested. AC-01..04,07,09..21,23..25,30..36 receive no proposed weakening.

## 3. Searchable text and exclusions

**Recommended proposal:** retain only public Tour Title, public Destination
names and public Tour Operator name for both indexed passages and comparison
baseline. Construct text deterministically with explicit field labels, normalized
whitespace/NFC and stable Destination ordering. Hash the canonical UTF-8 text.
Persist the construction version with each vector.

Description, category text, itinerary, POI content, reviews, private operator
information and user profiles are excluded. Expanding fields requires a separate
labelled comparison and Owner/Product/BA decision; travel-style queries may be
unanswerable from these three fields. No recommendation/persona feature is added.

## 4. Semantic relevance definition

**Proposed candidate:** `intfloat/multilingual-e5-small`, expected dimension 384,
CPU inference, `query: ` for query text and `passage: ` for Tour text, normalized
vectors, cosine similarity. Query and index must use the same pinned model
revision, tokenizer, prefixes, normalization and construction version. Actual
model execution and dependency versions belong to the PoC evidence record in §17.

The PoC pins revision `fd1525a9fd15316a2d503bf26ab031a61d056e98`. The
[model card](https://huggingface.co/intfloat/multilingual-e5-small/blob/fd1525a9fd15316a2d503bf26ab031a61d056e98/README.md)
defines 384 dimensions, query/passage prefixes, normalized embeddings and a
512-token input limit. Truncation behavior is part of the versioned pipeline and
must be disclosed in evaluation; a valid HTTP response is not evidence that a
long input was fully represented.

Similarity measures vector proximity, not probability, factual relevance or a
percentage of matched requirements. No generated embedding or random score may
substitute for real model inference. No per-request model reload is allowed.

## 5. Ordering and deterministic ties

**Recommended proposal:** for `sort=relevance`, semantic cosine score DESC,
then the eligible unrounded Published aggregate DESC with null last, then Title
ASC and TourId ASC using the approved SQL/provider comparison rules. Retaining
the rating tie-break keeps the shared eligibility gate relevant. Exact score ties
must be tested; do not silently introduce an epsilon, rounded-score bucket or
lexical boost. Quantization/approximate index effects must be recorded.

For an unchanged corpus, model version and request snapshot, pages must be
stable. Non-relevance sorts continue to use the frozen orders on the semantic
eligible set; semantic score does not silently replace the selected primary sort.

## 6. Threshold policy

**Open decision SEM-D1:** choose and validate a model/version-specific threshold,
or explicitly accept no threshold. No numeric threshold or quality target is
invented here. The recommended production direction is a calibrated threshold
approved from labelled evaluation before rollout, because nearest neighbors alone
can return unrelated Tours. A no-threshold PoC may return diagnostic neighbors
without claiming they satisfy UC-24 match acceptance. Model changes require
recalibration; UC-25's >80% rule does not apply.

## 7. No matches, unrelated and ambiguous queries

**Proposal:** a valid query with zero globally eligible matches returns a
successful empty page and MSG64. Low similarity must not be relabelled a system
error. Dependency failures must not be relabelled zero matches. Under a
no-threshold policy, unrelated queries can still return neighbors; Product/BA
must expressly accept that consequence or reject that policy. Ambiguous intent
must not silently add destination, price, category or other constraints.

## 8. Lexical fallback

**Recommended proposal SEM-D2:** no automatic lexical fallback. A failed semantic
request fails consistently under §12, preserving displayed results. Hybrid
retrieval, lexical rescue and exact-name boosts require separately specified
score fusion, inclusion, totals and ordering tests. Retaining a disabled
production implementation for controlled rollback is distinct from per-request
fallback and does not authorize a partial UC-24 rollout.

## 9. Interaction with independent criteria

SQL Server remains authoritative. Approved/Public visibility and every explicit
filter are hard predicates, never embedding hints: Destination retains its
accent/case-insensitive substring rule; category uses an active server-owned
code; duration is exact; price uses inclusive whole-VND bounds; dates retain
Asia/Ho_Chi_Minh semantics and the single request-level `asOfUtc`; minimumRating
uses the unrounded confirmed Published-only mean and excludes unrated Tours.
Apply all predicates before global rank/count/page. Preserve past/completed and
sold-out discoverability, Cancelled/disabled exclusions, nullable fields, primary
thumbnail, and availability/booking separation. Natural-language references to
conditions do not override the explicit canonical parameters.

## 10. Retrieval, paging and totals

**Recommended production option SEM-D3:** preserve the frozen envelope and exact
`totalCount`/`totalPages`. Complete coverage of SQL-eligible, current-version,
freshly indexed records and exact score evaluation of the full eligible set is
required before count/rank/page. Use bounded batches and set-based SQL predicates;
never load an unbounded corpus or issue one SQL query per candidate. If complete
coverage cannot be established within approved resource limits, fail rather than
emit an invented exact total. This option needs measured feasibility and scale
approval; it is not claimed implemented by the PoC.

Approximate HNSW top-K plus filtering can omit qualifying Tours and reorder
pages. Over-fetching improves candidate supply but cannot establish global recall
or exact totals. Metadata filters may accelerate retrieval but are stale hints;
SQL must recheck all authorization and business predicates. A bounded PoC may
report `candidateCount`, exhaustion/truncation and retrieval mode in diagnostics,
never map these to public `totalCount`. Filtering the first 20 neighbors is not
complete search. If approximate production paging is preferred, approve a new
versioned envelope and explicit Web/Mobile navigation semantics before rollout;
AC-28/37 and §14.1 would then need an approved replacement.

## 11. Freshness and publication lifecycle

**Proposal SEM-D4:** use stable TourId vector keys, idempotent upserts, source
content hash, model revision and text-construction version. SQL remains the
source of public content and visibility. Before returning a candidate, verify its
SQL visibility and that indexed content/model version is current; stale vectors
cannot authorize a result. Missing/stale embeddings are observable coverage gaps,
not evidence of no matches. Production completeness policy must be approved
before they can be silently excluded.

Publication indexes only approved public content after authorized rollout;
unpublication/deletion invalidates or removes vectors, with SQL checks protecting
the interval before vector deletion. Republishing requires current content.
Propose an idempotent reconciliation job and retry/dead-letter monitoring;
transactional outbox versus scheduled reconciliation and freshness SLA remain
open decisions. Synthetic PoC indexing does not authorize production writes.

## 12. Failure and degraded behavior

**Proposal SEM-D5:** bounded timeouts/cancellation, failure classification and
health checks for inference, Qdrant and SQL. Malformed/dimension-mismatched,
non-finite, wrong-version vectors and incomplete exact-count retrieval fail
closed. No response may expose unpublished Tours or report success after a
dependency error. Public failure uses existing ProblemDetails conventions and
MSG127 while clients preserve previous displayed results/criteria (AC-32/37).
Availability-only failure retains the frozen per-item `unknown` behavior.
Retry budgets, timeouts and operational SLOs need measured approval, not guesses.

## 13. Privacy, security, versioning and costs

**Proposal:** index only the three public fields; no raw query retention by
default, no query content in routine logs/traces. Record aggregate timings,
failure classes, model/version and counts. Treat embeddings and operator content
as application data; define access, retention and deletion policy before rollout.
Private networking/authentication protect inference and Qdrant; local Qdrant must
bind loopback. Pin dependencies and model revision, review model license/supply
chain, avoid repository model weights/secrets and impose approved query/resource
limits without undocumented public parameters.

CPU inference, model download/cache, vector storage/index RAM, SQL rechecks,
reindexing, backups and monitoring add costs. No cost estimate or capacity claim
is established without host/workload measurements. Upgrade through a separate
versioned collection, complete reindex, validate coverage, then controlled alias
cutover; do not mix vectors across model revisions.

## 14. Web/Mobile impact

Both platforms retain submit/apply triggering, anonymous access, canonical key
names, dynamic sort defaults, page-size 20, explicit page controls, Clear Filters,
search/detail/back context and required nullable response properties. No public
similarity score, new sort value, debug flag, query parameter or model selector is
proposed. Result meaning/order changes even if the schema is unchanged; update
parity fixtures and approved UX expectations after contract approval. Any new
explanation/copy must be English resource-backed. Approximate totals would be a
separate cross-platform breaking contract decision under SEM-D3.

## 15. Proposed acceptance criteria

These are proposed test obligations, not recorded passing tests or approved
numeric quality targets.

| ID | Proposed obligation / frozen trace |
|---|---|
| SEM-AC01 | Only the approved three public fields are deterministically embedded; same pinned query/passage model contract; replaces AC-05 lexical membership. |
| SEM-AC02 | Real-model Vietnamese exact-name, accents/no-accents, synonyms, natural-language, destination, travel-style, multi-condition, irrelevant and ambiguous labels are evaluated; quality thresholds and label review approved before release; replaces AC-06 lexical guarantee. |
| SEM-AC03 | Approved threshold/no-threshold membership and no-result behavior are tested without claiming cosine as probability; AC-07 empty omission preserved. |
| SEM-AC04 | Global semantic DESC and exact-score tie fixtures prove rating-null-last/Title/TourId order; replaces AC-22 classes. |
| SEM-AC05 | SQL rechecks exclude unpublished/unapproved and stale candidates, preserve all explicit filters and same request snapshot; AC-03,08..21,30 preserved. |
| SEM-AC06 | Missing/stale/incorrect-version embeddings, idempotent updates, unpublication and republishing are tested under the approved coverage/freshness policy. |
| SEM-AC07 | Multi-page tests prove complete eligible-set ranking and exact totals for SEM-D3's recommended option; candidate truncation never passes AC-28/37. |
| SEM-AC08 | Price/rating/departure primary sorts and defaults retain AC-23..27; no semantic override of explicit primary sorts. |
| SEM-AC09 | Real inference/Qdrant/SQL integration plus outages, timeout, malformed vectors and cancellation prove §12; skipped integrations are reported separately. |
| SEM-AC10 | No production route activation, no fabricated score, no public diagnostic fields, no production embeddings before separate rollout approval. |
| SEM-AC11 | Web/Mobile parity, stale results, no-store, resource copy and context restoration retain AC-29..37; contract fixtures identify semantic version. |
| SEM-AC12 | Independently reviewed labels, lexical comparison, quality/latency/resource results and approved deployment capacity/SLOs exist before production acceptance. |

## 16. Migration and rollout proposal

1. Review this CR and record explicit Owner/Product/BA decisions for every open
   product contract item; obtain Backend/DB/operations and Web/Mobile review.
2. Resolve model availability, real integration tests, benchmark quality and
   capacity; choose exact production retrieval/count strategy and freshness policy.
3. After separate implementation approval, build versioned non-production index
   from authorized data; prove SQL filters, freshness, failure, count and parity.
4. Obtain approved production indexing/deployment plan and readiness sign-off;
   production seed/backfill and Review/POI gates remain independent blockers.
5. Run shadow comparisons without changing public results or retaining raw
   queries; approve any experiment traffic/privacy scope separately.
6. Roll out through a reviewed internal feature boundary with explicit contract
   version/cutover and monitoring. No undocumented public query switch.
7. Roll back route activation to the previous authorized complete behavior on
   failure; preserve SQL data and Review provenance. Vector collection cleanup
   must be scoped to the disposable/new collection, never SQL volumes.

No automatic merge, production indexing or deployment is authorized by this CR.

## 17. Evidence, open decisions and approval record

### Baseline dependency snapshot

At the 2026-10-15 mission preflight, [Backend PR #54](https://github.com/FPTUCapstone/Capstone_BE/pull/54)
was merged at `c8cc7f4f88da0441c3607560ce58b9ea022544e0`.
[PR #72](https://github.com/FPTUCapstone/Capstone_BE/pull/72) remained
open/unmerged with green checks at HEAD `7c26f00153e9a5f7b0fdcc4150d4e69c06f27f64`; its migration
must not be copied as an approved dependency. PR state is time-sensitive and must
be rechecked at delivery. Docs #10 / TC-D1..TC-D9 production taxonomy decisions
remain open; no taxonomy names, seeds, backfill or NOT NULL enforcement are
approved here. Shared Review/POI compatibility ownership/release gate remains
open. PoC success does not close Task 2.3 contract approval or Task 2.6 runtime
eligibility acceptance.

### Delivery dependency recheck and implementation evidence

During this mission, PR #72 was merged at `2026-10-15T07:32:17Z` as
`b4d7c6a47f96adf0b921004e7204b7f30f0ccb51`, now the latest develop snapshot.
The implementation branches started from the verified preflight develop
`bdb70d8a74002dc5c99f124c7dfa46f4a648c82b`; no unmerged index migration was
copied, and this mission did not modify or merge PR #72. Its merge does not close
the shared Review/POI eligibility release gate.

- [Backend foundation PR #74](https://github.com/FPTUCapstone/Capstone_BE/pull/74): isolated V2 parsing/validation/DTO infrastructure; production Tour Search unchanged.
- [Independent features PR #75](https://github.com/FPTUCapstone/Capstone_BE/pull/75): explicitly stacked on #74; prepared independent SQL filters/projection and anonymous active taxonomy endpoint.
- [Semantic PoC PR #76](https://github.com/FPTUCapstone/Capstone_BE/pull/76), HEAD `305a6c31af8c8645f0f66febe2e4f46e2433c556`: isolated actual model/Qdrant implementation.
- [Immutable PoC execution evidence](https://github.com/FPTUCapstone/Capstone_BE/blob/305a6c31af8c8645f0f66febe2e4f46e2433c556/docs/tm-70-semantic-poc-evidence.md) and [local operation instructions](https://github.com/FPTUCapstone/Capstone_BE/blob/305a6c31af8c8645f0f66febe2e4f46e2433c556/tools/semantic-poc/README.md).

All implementation PRs remain Draft for verification/review and relevant approval
gates. No implementation PR or this proposed CR is automatically merged.

### Benchmark evidence status

PoC implementation and execution are a separate Backend branch. This CR does
not claim real inference, Qdrant compatibility or numeric performance results
until linked execution evidence is available. Required comparison is the frozen
three-field all-token lexical baseline, not a weakened title-only approximation.

The proposed PoC algorithm uses exact cosine retrieval over a controlled
synthetic collection capped at 256 entries, with a 257th sentinel rejecting
oversized collections. SQL batch rechecks the existing approved/published/active
operator predicate and current three-field content/model fingerprint. Its exact
indexed-corpus eligible count is not a claim of complete SQL universe coverage,
and does not satisfy frozen public global totals. It intentionally has no
threshold, fallback or production API wiring; §6 and §8 remain approval decisions.
Docker was initially unavailable, then restored. The isolated Docker project ran real pinned E5 inference and Qdrant 1.13.4; the evidence below records executed tests and a small synthetic comparison.

| Evidence | Current record |
|---|---|
| Model revision, dependency pins, collection and fixture corpus | Actual `intfloat/multilingual-e5-small@fd1525a9fd15316a2d503bf26ab031a61d056e98`, 384-dimensional normalized CPU inference; Torch 2.10.0+cpu / Transformers 5.3.0, resolved dependency lock. Qdrant 1.13.4, disposable `tm70_poc_<uuid>` Cosine collection; 12 synthetic Tours. |
| Labelled Vietnamese queries and label reviewer | 12 AI-authored Vietnamese graded queries across all requested intent classes; no independent Product/BA label validation. |
| Recall@5/@10, Precision@5, nDCG@10 | Actual semantic versus frozen lexical approximation: 0.981818/0.454545; 1.000000/0.454545; 0.300000/0.116667; 0.969186/0.454545 respectively. Recall/nDCG average 11 positive-label queries; precision averages all 12. No quality threshold or superiority claim. |
| p50/p95 query, inference and vector latency | One short local Docker run: embedding+vector 34.12/55.23 ms; inference computation 11.23/13.77 ms; vector HTTP 11.03/21.38 ms. Excludes SQL, startup and index build; not a production SLO. |
| CPU, memory, index build/update time | Inference RSS 1,201,692,672 bytes; query CPU delta 0.45 seconds / 105.27% one-core equivalent (two threads). 12-vector build/update 1170.49/446.44 ms; Docker snapshots 846.9 MiB inference / 92.68 MiB Qdrant, different accounting from RSS. |
| SQL authority integration | `SemanticPocSqlServerTests`: 3 passed, 0 failed, 0 skipped on real SQL Server during mission verification. Deterministic test vectors isolate orchestration; these tests do not prove model inference or Qdrant compatibility. |
| Real-model/Qdrant/failure tests | Combined .NET/SQL/E5/Qdrant Release tests: 4 passed / 0 failed / 0 skipped, including real-model hidden-Tour rejection, idempotent upsert and cleanup. Python 6 tests and real isolated embedding/Qdrant outage experiments passed failure expectations; unavailable states returned exit 1. Full PoC solution with Redis and SQL disabled: 3,215 passed / 0 failed / 386 skipped; provider-backed cases executed separately, skips never counted as passes. |
| Proposed numerical acceptance thresholds | Unset; require Owner/Product/BA approval and operational feasibility review |


The standalone comparator approximates the frozen three-field, accent-insensitive,
all-token/worst-best-class algorithm in Python; it does not prove SQL collation
parity and does not benchmark an existing keyword endpoint (the current production
endpoint has no keyword input). All fixture ratings are null. The semantic PoC
implements SQL publication/operator visibility and freshness checks, while explicit
production destination/category/date/price/duration/rating/availability interaction
acceptance remains unimplemented and gated. No-threshold retrieval returned
irrelevant items for the zero-relevance query; this makes SEM-D1 a necessary product
decision. Actual collection cleanup was checked; no production embeddings were written.
Measured host: Intel Core i5-12500H (12 cores / 16 logical processors); Docker Linux/WSL2 exposed 16 CPUs and 8,155,713,536 bytes of memory. Inference used two Torch threads; concurrent development/test workloads were present. This was not isolated load testing.

Small synthetic fixtures cannot establish production relevance, coverage,
Vietnamese linguistic robustness or scale. Report cold/warm timing conditions,
hardware, corpus/query sizes, model revision, collection settings, candidate cap,
retrieval exactness, label method and measurement command with any later numbers.

| Decision | Required decision owner(s) | Status |
|---|---|---|
| SEM-D1 relevance membership/threshold and no-match policy | Owner / Product / BA | Pending |
| SEM-D2 no fallback versus separately specified hybrid behavior | Owner / Product / BA | Pending |
| SEM-D3 exact count/coverage feasibility versus versioned approximate API | Owner / Product / BA, Backend/DB, Web/Mobile | Pending |
| SEM-D4 freshness/missing-vector policy, synchronization and SLA | Owner / Product / BA, Backend/DB/operations | Pending |
| SEM-D5 failure contract, resource limits, SLO and capacity budget | Owner / Product / BA, Backend/operations | Pending |
| Field expansion, ranking ties, exact-name priority, accent expectations | Owner / Product / BA | Pending; three-field scope and §5 ties only proposed |
| Label review, quality targets and benchmark acceptance | Owner / Product / BA | Pending |
| Model/license/security and production rollout readiness | Owner with engineering/security/operations review | Pending |

| Approval role | Decision/evidence | State |
|---|---|---|
| Owner | No semantic contract approval record attached | Pending |
| Product | No semantic contract approval record attached | Pending |
| BA | No semantic contract approval record attached | Pending |

**PROPOSED — PENDING OWNER/PRODUCT/BA APPROVAL**. Engineering evidence can
support approval; it cannot substitute for these product decisions.
