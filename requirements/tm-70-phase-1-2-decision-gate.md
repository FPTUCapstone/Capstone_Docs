# TM-70 Phase 1.2 — TourCategory Production Decision Gate

**Status:** `BLOCKED_EXTERNAL_APPROVAL`
**Scope:** Production taxonomy seeds, legacy Tour backfill, category-code contract,
and final `commerce.Tours.category_id NOT NULL` enforcement.
**Last verified:** 2026-10-14

## 1. Authority and evidence reviewed

This decision record supplements, and does not replace, the approved TM-70 V2
contract and implementation plan at Docs baseline
`2ad6916dc1ff4f314c19e13258c088533a93cd9a`.

The following sources were checked for a verifiable approval:

- Report 3 V2 and the approved TM-70 V2 addendum/implementation plan;
- Backend `develop` at merge commit
  `c8cc7f4f88da0441c3607560ce58b9ea022544e0`, including the PR #54 review
  handoff and `TOUR_CATEGORY_CODE_ENCODING_DECISION_PENDING`;
- Jira TM-70, TM-211, and TM-204, including their descriptions and comments.

No current Product/BA approval for a production category list, no approved
legacy `TourId -> category` mapping, and no approved resolution of the category
code encoding contract were found. Jira TM-70, TM-211, and TM-204 contain no
approval comments as of the verification date. AI suggestions and test-only
fixtures are not approval evidence.

## 2. Pending decision register

All decisions remain `PENDING`. Phase 1.2 production implementation must not
begin until every applicable decision has an auditable approval reference.

| ID | Decision required | Current state | Required approver | Approval evidence required |
|---|---|---|---|---|
| TC-D1 | Restricted ASCII machine identifiers or Unicode identifiers | `PENDING` | Product/BA + Backend technical owner | Explicit selected representation |
| TC-D2 | Allowed character classes | `PENDING` | Product/BA + Backend technical owner | Exact allowlist/grammar |
| TC-D3 | Normalization, casing, and whitespace rules | `PENDING` | Product/BA + Backend technical owner | Canonicalization and rejection rules |
| TC-D4 | Maximum length and uniqueness semantics | `PENDING` | Product/BA + Backend technical owner | Length unit and comparison rules |
| TC-D5 | Collation behavior | `PENDING` | Backend technical owner, accepted by Product/BA | Exact SQL collation and case/accent behavior |
| TC-D6 | `VARCHAR` or `NVARCHAR` storage | `PENDING` | Backend technical owner, accepted by Product/BA | Selected SQL type and migration direction |
| TC-D7 | Domain, API, and SQL validation/rejection behavior | `PENDING` | Product/BA + Backend technical owner | Matching cross-layer validation contract |
| TC-D8 | Migration and backward-compatibility policy | `PENDING` | Data owner + Backend technical owner | Upgrade, mixed-version, rollback, and recovery decision |
| TC-D9 | Production category codes, English names, active flags, and every legacy Tour mapping | `PENDING` | Product/BA + data owner | Approved seed list and row-level mapping artifact |

`TOUR_CATEGORY_CODE_ENCODING_DECISION_PENDING` remains an unresolved P2 risk.
The current `VARCHAR(50) COLLATE Latin1_General_100_CI_AS` code column and the
Domain's Unicode-capable trim/nonblank/length checks are not a complete shared
encoding contract. No ASCII restriction or `NVARCHAR` migration is approved.

## 3. Current implementation inventory

The transitional Phase 1.1 foundation currently contains:

- `catalog.TourCategories` with integer identity `category_id`;
- `code VARCHAR(50) COLLATE Latin1_General_100_CI_AS` and a case-insensitive
  unique index `UX_TourCategories_Code`;
- English `name VARCHAR(120) COLLATE Latin1_General_100_CI_AS`;
- `is_active BIT` and the active-name lookup index;
- nullable `commerce.Tours.category_id`, its foreign key, and lookup index;
- Domain entity/configuration for `TourCategory` and nullable Tour mapping;
- guarded fresh/upgrade/idempotency SQL Server tests;
- no production category seed, no production backfill, and no final non-null
  enforcement.

Affected implementation components for the future approved change are:

- `TripMate.Domain.Entities.TourCategory` and its validation tests;
- `TourCategoryConfiguration` and `TourConfiguration`;
- `database/tripmate_schema_v7.sql`;
- an additive, transactional, idempotent SQL migration under
  `database/migrations/`;
- SQL migration parity, data-preservation, rollback, and rerun tests;
- deployment data artifacts owned by Product/BA and the data owner.

This inventory does not authorize the public taxonomy endpoint, search category
filter, clients, or any Phase 2 behavior.

## 4. Required production mapping artifact

TC-D9 must provide a controlled, reviewable artifact containing at least:

| Field | Requirement |
|---|---|
| `categoryCode` | Stable approved code following TC-D1 through TC-D7 |
| `englishName` | Approved public English name |
| `isActive` | Explicit production active state |
| `tourId` | Exact existing `commerce.Tours.tour_id` |
| `approvedCategoryCode` | Exactly one approved category code for that Tour |
| `approvalReference` | Product/BA approval link or immutable decision identifier |

The artifact must cover every persisted production Tour exactly once, contain no
unknown Tour/category identifiers, and be reviewed against a current preflight
inventory. Category assignments must not be inferred from titles,
descriptions, destinations, POIs, or test fixtures.

## 5. Future TDD and acceptance contract

After approval, implementation starts with failing tests for:

1. exact accepted/rejected category-code boundaries from TC-D1 through TC-D7;
2. Domain, EF, and SQL agreement on representation, collation, and uniqueness;
3. exact, idempotent insertion of the approved production taxonomy;
4. rejection of conflicting same-code/different-name seed data;
5. preservation of existing category identities and inactive categories;
6. complete, one-to-one legacy Tour mapping with no heuristic assignment;
7. failure and full transaction rollback when mapping coverage is incomplete;
8. `category_id NOT NULL` only after every Tour references an approved category;
9. preservation of Tour, TourMedia, identity, FK, and unrelated rows;
10. fresh-schema and upgrade-schema inventory parity;
11. migration rerun idempotency;
12. wrong-shape detection and rollback;
13. a reviewed `VARCHAR`/`NVARCHAR` migration path, depending on TC-D6;
14. inactive categories remaining stored but unavailable for future selection.

Test-only categories remain permitted only in isolated automated tests and must
be clearly labelled. They must never be promoted as the production taxonomy.

## 6. Production rollout and recovery checklist

The future implementation PR must record:

- immutable approval references for TC-D1 through TC-D9;
- backup/snapshot and exact target database identity (without credentials);
- isolated restoration rehearsal before any shared-environment execution;
- preflight row counts, duplicate-code checks, conversion/collision checks, and
  100% mapping coverage;
- fresh, upgrade, and rerun migration evidence on SQL Server;
- transaction rollback evidence for invalid seeds, missing mappings, and
  wrong-shape objects;
- post-migration FK, null-count, seed-count, mapping, identity, and TourMedia
  preservation checks;
- mixed-version compatibility and writer deployment order;
- roll-forward procedure if code representation or approved data is wrong;
- explicit prohibition on destructive guessing, silent conversion, or
  reverting to nullable merely to hide a failed rollout.

Until the gate is closed, no production seed, backfill, category-code contract
change, or `NOT NULL` migration is authorized. Phase 1.3 rating feasibility may
proceed independently, as allowed by the frozen implementation plan.
