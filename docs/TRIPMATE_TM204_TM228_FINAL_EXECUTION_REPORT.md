# TripMate TM204 and TM228 execution report

Execution date 2026-11-15. Source hashes, verbatim SRS excerpts and the initial task matrix are recorded in [the discovery matrix](TM204_TM228_DISCOVERY_MATRIX.md). Two local files named Report3_Software-Requirement-Specification.docx were older copies. The account rules were verified directly against a fresh export of the [original Report 3](https://docs.google.com/document/d/1VwPkqRyC6ZlbXBQC0ztdektt0vAyE3qt/edit).

| Issue | Outcome and acceptance |
|---|---|
| [TM-208](https://tripmate-capstone.atlassian.net/browse/TM-208) | Existing active-primary-only query retained. OpenAPI now requires the nullable key. Existing baseline catalogue checks and updated focused SQL/HTTP checks passed. |
| [TM-209](https://tripmate-capstone.atlassian.net/browse/TM-209) | Mobile already covers loading, absent and failed images. Web now reuses its neutral placeholder in these cases, with fixed dimensions and recovery when the URL changes. Focused thumbnail tests passed. |
| [TM-210](https://tripmate-capstone.atlassian.net/browse/TM-210), [TM-211](https://tripmate-capstone.atlassian.net/browse/TM-211) | Public Detail includes a privacy-filtered list of confirmed Published reviews. Detail now uses the same confirmed publication source as public Search. Canonical writes and legacy/POI readers retain their existing responsibilities. Updated detail SQL and focused writer/reader tests passed. |
| [TM-212](https://tripmate-capstone.atlassian.net/browse/TM-212) | Original SRS and controller routes identify Recommendations (TM-71), Booking (TM-73), and Payment (TM-74). No official Tour Favorite use case or live Tour Favorite contract was found. Product decision remains open. |
| [TM-228](https://tripmate-capstone.atlassian.net/browse/TM-228) | Account controls added for permitted roles, including audited lock/unlock and session revocation. Staff controls for existing operation entry points are aligned in Backend and Web. Route and landing Backend coverage and the provider commission integration remain unverified or incomplete. |

## TM-212 ownership

| Capability | Owner | Existing entry point |
|---|---|---|
| Tour Recommendations | TM-71, UC-25, §3.5.2 | GET `/api/v1/tour-recommendations`, POST `/api/v1/tour-recommendations/refresh` |
| Tour Detail | TM-72, UC-26, §3.5.3 | GET `/api/v1/tours/{tourId}` |
| Booking | TM-73, UC-27, §3.5.4 | POST `/api/v1/traveler/tour-bookings/quote` and POST `/api/v1/traveler/tour-bookings` |
| Payment | TM-74, UC-28, §3.5.5 | POST `/api/v1/traveler/bookings/{bookingId}/payments` |
| Tour Favorite | Product decision required | No Tour Favorite endpoint. Existing POI favorites have a separate purpose. |

Product should confirm whether to remove the Tour Favorite dependency from TM-212 or approve it under a dedicated requirement and owner. TM-74 remains the Payment dependency.

The [proposed Jira wording](TM212_OWNER_DECISION_PROPOSAL.md) is submitted for Owner review. The Jira title, description and requirements remain as recorded until explicit approval.

## TM-228 release dependencies

Apply the additive SQL migration before deploying the API. The account API filters by actor role before pagination or returning details. It preserves previous status for manual locks and invalidates signed access sessions and refresh tokens. An older locked Service Provider whose commission status cannot be verified remains locked pending integration with the authoritative provider workflow.

The existing Web route directory and Tour moderation queue have limited production integration. The landing proxy is gated to Staff. No landing management or route CRUD controller was found on the audited Backend branch. Those gaps and any additional role or session regression results must be resolved or documented before Jira completion.

| Operation | SRS role | Delivery evidence |
|---|---|---|
| Account directory, lock and unlock | Staff for Traveler, Tour Operator and Service Provider. Administrator for Staff. | [API contract](../api/contracts/staff-admin-user-accounts.md) |
| Operator application, tour decision and active-trip monitoring | Staff, §§3.9.2, 3.9.6, 3.9.7 | Backend rejects Administrator at these entry points. Web direct navigation uses the same role. |
| POI and routes | Staff creates POIs and owns routine routes. Staff and Administrator can update POIs. Administrator removes POIs. | Existing POI authorization was retained where already aligned. Web route screens require Staff. Backend route CRUD remains absent. |
| Booking monitoring and landing content | Staff, §§3.9.8 and 3.9.10 | Existing Booking gate retained. Web landing gate tightened. Backend landing management contract remains absent. |
| Algorithm configuration, reports and audit log | Administrator, §§3.9.5, 3.9.11 and 3.9.12 | Existing Administrator policies retained. |

### Follow-up ownership audit

The fresh export of the official original Report 3 has the same SHA-256 as the source in the discovery matrix. The latest Jira descriptions and active branches establish these dependencies:

| Gap | Existing owner and evidence | TM-228 integration consequence |
|---|---|---|
| Route create/list and update | [TM-101](https://tripmate-capstone.atlassian.net/browse/TM-101) has [Backend #167](https://github.com/FPTUCapstone/Capstone_BE/pull/167) with a Staff-only route controller and create/list/options. [TM-102](https://tripmate-capstone.atlassian.net/browse/TM-102) owns update. | Verify the Staff gate when the owner branches land. Leave route endpoints with those tickets. |
| Landing CMS | [TM-112](https://tripmate-capstone.atlassian.net/browse/TM-112) is marked Done, but [Backend #110](https://github.com/FPTUCapstone/Capstone_BE/pull/110) is open. Its controller currently admits Staff and Administrator, and the Jira description names Administrator. Report 3 §3.9.10 assigns the operation to Staff. | Align the owner PR and reconcile the outdated Jira role before integration. The Web proxy on #166 requires Staff. |
| Provider onboarding versus financial obligations | [TM-225](https://tripmate-capstone.atlassian.net/browse/TM-225) has [Backend #169](https://github.com/FPTUCapstone/Capstone_BE/pull/169) for onboarding and service requests. [TM-226](https://tripmate-capstone.atlassian.net/browse/TM-226) owns statements, receipt confirmation and BR-200 automatic locks. | The account endpoint continues to deny release when the required settlement cannot be verified. Verify the authoritative paid-state integration once TM-226 is available. |

Web and Docs may be reviewed independently. Final integration for account administration requires the Backend migration and API first, followed by the Web deployment. Completing the remaining TM-228 acceptance criteria also requires the owner integrations listed above.

## Verification

| Gate | Result |
|---|---|
| Backend catalogue baseline SQL integration | 101 passed, zero skipped |
| Backend updated Search/Detail SQL integration | 20 passed, zero skipped |
| Backend focused Detail and supporting unit tests | 77 passed, zero skipped |
| Backend account/session/OpenAPI focused API checks | 20 passed, zero skipped before subsequent operational changes |
| Backend account, Staff operations and OpenAPI focused integration | 28 passed, zero skipped |
| Backend SQL-backed authorization regression and persisted JWT fixtures | 54 passed, zero skipped |
| Backend SQL Server simultaneous lock attempts | Passed. One request succeeds and the other returns 409. |
| Follow-up account access audit | 15 targeted API tests passed and the Service Provider SQL test passed separately. Missing and inaccessible accounts produce identical 404s and equivalent denial events without target identifiers. Changed-file formatting passed. |
| Web clean baseline | 93 Node tests and 4,053 Vitest tests passed, 5 existing skips |
| Web thumbnail focused tests | 53 passed after 2 new regressions failed on the original implementation |
| Web full suite with account integration | 94 Node tests and 4,055 Vitest tests passed, 5 skips |
| Web moderation form | Two additional tests passed for required reasons and API denials |
| Web typecheck, ESLint and production build | Passed. Production build needed network access for the existing Google Fonts dependency. |
| Mobile `flutter test` | 2,609 passed, 4 skips |
| Mobile `flutter analyze` | No issues |

Human review should verify migration ordering, active session revocation, operational role gates and public review privacy before merge.

## Delivery

| Repository | Branch or baseline | Draft PR and CI |
|---|---|---|
| Backend | `feature/tm204-tm228-sequential-delivery` at `2700ad2c` | [Draft #171](https://github.com/FPTUCapstone/Capstone_BE/pull/171), no conflicts, one approval required, [current CI pending](https://github.com/FPTUCapstone/Capstone_BE/pull/171/checks) |
| Web | `feature/tm204-tm228-sequential-delivery` at `8a4abcd` | [PR #166](https://github.com/FPTUCapstone/Capstone_FE/pull/166) ready, no conflicts, `sekiro171` requested, [CI and Vercel passed](https://github.com/FPTUCapstone/Capstone_FE/pull/166/checks) |
| Mobile | develop `c7eef0fbc33e3d7e52b028fd28ddb6ba1212a38b` | Existing implementation, no new diff |
| Docs | `feature/tm204-tm228-sequential-delivery` | [PR #15](https://github.com/FPTUCapstone/Capstone_Docs/pull/15) ready, clean, local path and whitespace checks passed, no required CI reported, `sekiro171` requested |

[Backend run 43712557294](https://github.com/FPTUCapstone/Capstone_BE/actions/runs/43712557294) was verified against `bdeeeecf8aa38129c049e6a13dcff993a260c432`. It was queued behind the preceding SQL run and GitHub superseded it when the account audit fix was pushed. It recorded no test result. The current Backend head has its own run.

At follow-up audit, all three PRs had zero unresolved review threads. Web and Docs are **READY_FOR_HUMAN_REVIEW**. Backend remains Draft pending CI and review, and full TM-228 is **IMPLEMENTATION_INCOMPLETE** because of the owner integrations above. TM-212 remains open for explicit Owner approval. Evidence is recorded in Jira.
