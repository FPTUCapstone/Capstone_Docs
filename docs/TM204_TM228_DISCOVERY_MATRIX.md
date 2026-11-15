# TM204 and TM228 execution discovery

Date: 2026-11-15. This is a discovery record, not a completion claim.

## Requirement source correction

The canonical original Report 3 is [the document referenced by Jira](https://docs.google.com/document/d/1VwPkqRyC6ZlbXBQC0ztdektt0vAyE3qt/edit). A read-only DOCX export was retrieved on 2026-11-15, SHA256 `BE70E6E8006DB84F445AA59D72EEEBD0CEB4AE51F21BCD27BE5AA4AF994AF087`. It is not V2. Neither local file with the original filename is a current copy. No source document was edited.

| Local source | Bytes | SHA256 | Finding |
|---|---:|---|---|
| `D:\CapStone\report\Report3_Software-Requirement-Specification.docx` | 5763791 | `EF675A01A1F459C1A0994AB0F40B0C99BBAF55CCD68A0F882787D37A12C681B3` | Older Administrator-only text in 3.9.1; not current authority |
| `D:\CapStone\Report3_Software-Requirement-Specification.docx` | 9645892 | `74D81C867C068CF5312187337AFA6BD5A83F1F8E2A5EF0CC1E19415F4EDC00C8` | Also has older Administrator-only 3.9.1; Staff appears elsewhere |
| `D:\CapStone\Report3_GDoc_Export.docx` | 5208616 | `093D69D721831608F462DE0AC450E4BBF3A8A96477B991622FDEAD1EFA3D25BA` | Contains updated Staff/Admin account rules; corroborates live export |

The initial agent inference that original Report 3 lacked Staff was incorrect: it used stale local copies. TM-228 is authorized by the current original SRS and must not be held for that reason.

## Verbatim current original SRS excerpts

Section 3.9.1:

> This group covers the review of the platform accounts and the locking and the unlocking of an account by Staff or an Administrator (BR-168).

Section 3.9.1.1:

> Actors / Roles: Staff or Administrator role

> The system retrieves only the accounts within the scope of the signed-in role under BR-168, then applies the filters by role, status and search keyword.

> BR-182: Traveler and Tour Operator accounts are created only through self-registration (UC-01, UC-02); no Staff or Administrator function creates them, and no function changes the role of an existing account. Staff accounts are created only by an Administrator (UC-76) and Service Provider accounts only by Staff (UC-77).

Section 3.9.1.2:

> BR-168: A Staff account may view, lock and unlock Traveler, Tour Operator and Service Provider accounts only. An Administrator account may view, lock and unlock Staff accounts only. Locking changes only the access status, not the role.

> BR-175: The locking of an account requires a recorded reason and terminates all the active sessions and refresh tokens of that account.

> BR-176: A locked account cannot sign in until it has been unlocked by an account authorised under BR-168; an account locked automatically under BR-189 may be unlocked only after its pending debt has been fully recovered.

Section 3.9.1.3 also repeats BR-168 and BR-176 and states:

> BR-171: The unlocking of an account requires a recorded reason.

> BR-200: When a commission statement remains unpaid 15 days after its due date, the system locks the Service Provider account automatically and its services are no longer open for booking. The account may be unlocked by Staff only after every overdue statement has been paid.

Jira TM-228's Staff/Administrator split agrees with these rules. Its acceptance text omits Service Provider, while BR-168 explicitly includes it. The current original SRS controls.

## Initial execution matrix

| Task | SRS reference | Existing implementation | Missing work / classification | Repository | Dependencies | Proposed action |
|---|---|---|---|---|---|---|
| TM-208 | 3.5.1; BR-54/56/59; approved media addendum | Primary-only nullable DTO, bounded page media query, endpoint/OpenAPI/SQL tests; BE PR 29 and Docs PR 8 historical delivery | NOT_VERIFIED pending fresh runtime tests | BE, Docs | TM-207 | Retain and rerun contracts, filtering and query-count tests |
| TM-209 | 3.5.1; approved TM-208 contract | Web typed parser/card and Mobile model/entity/network thumbnail with neutral loading/error builder; Mobile PR 28 merged | MISMATCH: Web card lacks failed-image fallback | FE, Mobile | TM-208 | Regression test, minimal fallback, verify existing Mobile behavior |
| TM-210 | 3.5.3 UC-26; BR-54/56/59/60/96 | BE PR 93, FE PR 90 and Mobile PR 49 merged; typed gallery/itinerary/policy/schedule/operator/rating | MISMATCH: detail aggregate accepts publication-unknown legacy reviews. Published review list absent; older approved UC-26 spec explicitly deferred it | BE, Docs | TM-72, TM-211 | Reuse confirmed-published projection; document field provenance and list ownership without fabricating content |
| TM-211 | 3.5.3, 3.7.2; BR-91–96 | Canonical TripReviews, screened submission/edit, shared legacy reader and canonical-only public search projection; internal PR 83 merged | MISMATCH: detail reader includes legacy rows without publication provenance | BE, Docs | TM-210, TM-79 | Reuse canonical-only aggregation for public detail; retain unrelated legacy/POI behavior |
| TM-212 | 3.5.2 UC-25; 3.5.4 UC-27; 3.5.5 UC-28 | Recommendation merged BE 166/FE 161/Mobile 82; booking and payment owners exist | MISMATCH: Jira assigns Favorite to payment TM-74; Favorite has no official UC | Docs | TM-71/72/73/74 | Correct dependency evidence; verify existing APIs; hold Favorite for genuine product decision |
| TM-228 | 2.2.2, 3.9.1, 5.1; BR-168/175/176/182 | Staff role foundation, role-aware shell, some Staff-only operations; account directory is unavailable placeholder; approvals remain Administrator-only | MISSING_IMPLEMENTATION / MISMATCH | BE, FE, Docs | Existing auth/session/audit; TM-225 provider work remains separate | Add scoped account management and negative tests; align authorized operational policies and Web guards |

## Repository isolation

All four original checkouts contain unrelated changes. Dedicated `feature/tm204-tm228-sequential-delivery` worktrees were created from fetched `origin/develop` under `D:\CapStone\.codex-worktrees\tm204-delivery`. Original working trees and indexes are preserved. No merge or auto-merge is authorized.

Fetched develop baselines: BE `18d5137d413edafbf735d655b35feb6e5e44eb86`; FE `e16b1c3a541be3ee2a9776f85edf0ef151659bae`; Mobile `c7eef0fbc33e3d7e52b028fd28ddb6ba1212a38b`; Docs `69386637a82e21e710ac71b72dc7bb881e1c4465`.
