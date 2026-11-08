# TripMate — Web Scope / UI Coverage Matrix

> **Status:** LIVE SRS REBASELINED — BLOCKED BY SOURCE CONFLICTS. This traceability matrix does not amend the SRS or authorize disputed implementation.

## Source of Truth and Baseline

- Canonical SRS: [`Report3_Software-Requirement-Specification.docx`](https://docs.google.com/document/d/1VwPkqRyC6ZlbXBQC0ztdektt0vAyE3qt/edit) — **original non-V2**.
- Live baseline independently downloaded on **2026-11-08**: Drive revision `0B5_dfpH4IC_wMlAwM25FL25jZ2FWdVZKTFhqRVdOdFRoeDdZPQ`, SHA-256 `d51b7dbbd09d20784284faa9c12ddcdeadb3c0b3bb2010801a0b2f03c4fd2f46`, 9,570,836 bytes. This is the baseline reconciled by this matrix.
- Historical comparison snapshot: SHA-256 `cd97de4bd74df3b5cdc159171ee964093d4bb7ba99ca65b52232f88b11de1c6b`, 9,571,142 bytes. It is retained only to classify requirement drift and is not the current authority.
- Requirement IDs follow live **SRS Table 3 — Use Case Description**, covering **UC-01 through UC-75** (75 entries). The live Product Overview/actors, Tables 3/4.1/4.2/5, non-screen functions, detailed §§3.2–3.10, business rules, validation, post-conditions, application messages and external-interface/quality sections were reconciled. Remaining contradictions are explicitly classified below.
- `Capstone_Docs` history and existing UI specs are evidence only; neither V2 nor GitHub/Jira nor an implementation overrides the canonical SRS without explicit, traceable approval.
- Existing Web Screen means a Web screen **specification** under `ux/web-screen-specifications/`, **not proof of implementation or production deployment**.

## Platform/Scope Legend

| Classification | Meaning |
|---|---|
| `SHARED_WEB_MOBILE` | Detailed SRS explicitly supports responsive Web and Flutter Mobile. |
| `WEB_CONFIRMED` | Web UI explicitly confirmed; no implied rejection or confirmation of Mobile support. |
| `MOBILE_ONLY` | Detailed SRS explicitly limits the specific function to Flutter. |
| `INTERNAL_WEB_ONLY` | Staff/Administrator Web administration workspace; individual permissions may be disputed. |
| `WEB_WORKFLOW` | Broader system process with an explicit Web interaction or visible state. |
| `NON_SCREEN` | Action/system operation requiring no dedicated Web page, though existing views may show results. |
| `CONFLICT_WEB_MOBILE` | Official SRS sections disagree on Web ownership; **owner decision required**. |

## Canonical Web UC Coverage

| UC | SRS Table 3 Use Case | Actor (SRS Catalogue) | Web Supported | Existing Web Screen Spec | Web Screen Needed | Requirements Trace / Caveat |
|---|---|---|---|---|---|---|
| UC-01 | Register Traveler Account | Guest | SHARED_WEB_MOBILE | Traveler Registration; Verify Account | YES — Batch 1 | §3.2.1: public Web and Mobile; Pending Verification via email code; Google can create Active account. |
| UC-02 | Register Tour Operator Account | Guest | SHARED_WEB_MOBILE | Tour Operator Registration; Application Submitted state; Operator Application Status | YES — Batch 1 | §3.2.2 explicitly supports both Flutter and Next.js. Operator registration awaits Staff approval; submitted notice is a state, not separate screen. |
| UC-03 | Resubmit Tour Operator Application | Tour Operator | SHARED_WEB_MOBILE | Operator Application Status; Resubmit Application | YES — Batch 1 | §3.2.3 explicitly supports both platforms; rejected applicant reuses account, not new registration. |
| UC-04 | Sign In | Guest | SHARED_WEB_MOBILE | Public Sign In; Admin Login | YES — Batch 1 | Table 3 actor is Guest; §3.2.4 lists Traveler/Operator/Administrator, while authorization Table 5 also grants Staff administration login. ROLE_CONFLICT. |
| UC-05 | Sign Out | Traveler / Tour Operator / Staff / Administrator | NON_SCREEN | None | NO dedicated page | Menu/sign-out action on existing shell; Table 3 includes Staff but detailed §3.2.5 omits Staff. ROLE_CONFLICT. |
| UC-06 | Reset Password | Guest | SHARED_WEB_MOBILE | Password Recovery (public and Admin contexts; code/new-password states) | YES — Batch 1 | §3.2.6 email reset code and new-password states; Table 5 lists Staff administration reset context, detailed UC omits Staff. ROLE_CONFLICT. |
| UC-07 | Change Password | Traveler / Tour Operator / Staff / Administrator | SHARED_WEB_MOBILE | None | DEFERRED — shells | §3.2.7 authenticated account settings; Table 3 and authorization Table 5 include Staff, detailed actor list omits Staff. ROLE_CONFLICT. |
| UC-08 | Update Traveler Profile | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.2.8: Traveler Profile/Edit Profile on responsive Web and Flutter. |
| UC-09 | Update Travel Preferences | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.2.9: preferences with interest tags, styles, budget; shared platforms. |
| UC-10 | Create Scheduling Request | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.3.1: Scheduling Request form shared across Web and Flutter. |
| UC-11 | View Suggested Itinerary | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.3.2: suggested itinerary timeline and map overview on Web and Flutter. |
| UC-12 | Explore Points of Interest | Guest / Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Table 3 and Table 5 include Guest POI exploration; §3.3.3 detailed actor names Traveler only. ROLE_CONFLICT. |
| UC-13 | Navigate Route | Traveler | MOBILE_ONLY | None | NO Web screen | §3.3.4 real-time navigation UI is Flutter-only. |
| UC-14 | Receive Real-Time Alerts | Traveler | MOBILE_ONLY | None | NO Web screen | §3.3.5 real-time trip alerts within Mobile navigation; no corresponding Web alert screen. |
| UC-15 | Confirm Re-routing | Traveler | MOBILE_ONLY | None | NO Web screen | §3.3.6 rerouting proposal/confirmation is Flutter-only. |
| UC-16 | Download Offline Map & Itinerary | Traveler | MOBILE_ONLY | None | NO Web screen | §3.3.7 offline map/itinerary download is Flutter-only. |
| UC-17 | Create Travel Group | Traveler | MOBILE_ONLY | None | NO Web screen | §3.4.1 create travel group is Flutter-only. |
| UC-18 | Invite Group Members | Traveler | MOBILE_ONLY | None | NO Web screen | §3.4.2 invite members by code/QR is Flutter-only. |
| UC-19 | View Group Members | Traveler | MOBILE_ONLY | None | NO Web screen | §3.4.3 group members screen is Flutter-only. |
| UC-20 | Remove Group Member | Traveler | MOBILE_ONLY | None | NO Web screen | §3.4.4 group-host remove action/dialog is Flutter-only. |
| UC-21 | Leave Travel Group | Traveler | MOBILE_ONLY | None | NO Web screen | §3.4.5 leave group confirmation is Flutter-only. |
| UC-22 | Configure Group Location Sharing | Traveler | MOBILE_ONLY | None | NO Web screen | §3.4.6 GPS sharing settings are Flutter-only. |
| UC-23 | Join Shared Group Trip | Traveler | MOBILE_ONLY | None | NO Web screen | §3.4.7 join shared trip by code/QR is Flutter-only. |
| UC-24 | Search Tours | Guest / Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.5.1: public search on both Web and Mobile. Search, filters and availability follow detailed SRS. |
| UC-25 | Receive Tour Recommendations | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.5.2: Traveler tour recommendations on both Web and Mobile. |
| UC-26 | View Tour Details | Guest / Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.5.3: public Tour Details on both Web and Mobile; booking requires Traveler access. |
| UC-27 | Book Tour | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.5.4: Traveler Booking UI on Web and Mobile. |
| UC-28 | Make Electronic Payment | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.5.5: payment entry Web/Mobile; gateway-hosted page is external, not TripMate UI. |
| UC-29 | View QR E-ticket | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.5.6: Traveler QR e-ticket VIEW on Web/Mobile; check-in SCANNING remains UC-43 Mobile-only. |
| UC-30 | View Commercial Service | Traveler | MOBILE_ONLY | None | NO Web screen | §3.6.1 explicitly calls Commercial Service Detail/Booking UI Flutter-only. |
| UC-31 | Book Commercial Service | Traveler | CONFLICT_WEB_MOBILE | None | HOLD — owner decision | Product Overview says commercial-service booking is Mobile-specific; §3.6.2 Interface explicitly says Flutter AND Next.js. AMBIGUOUS_REQUIREMENT — no Web inclusion/exclusion decided. |
| UC-32 | View Trip History | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.7.1: Traveler trip history available Web and Mobile. |
| UC-33 | Submit Trip Review | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.7.2: post-trip review Web and Mobile. |
| UC-34 | Update Tour Operator Profile | Tour Operator | WEB_CONFIRMED | None | YES — later batch | §3.8.1 explicitly specifies Next.js Operator Profile; general Product Overview does not prove separate Mobile UI. |
| UC-35 | Create Tour Package | Tour Operator | WEB_CONFIRMED | None | YES — later batch | §3.8.2.1 Next.js Tour Package create form. |
| UC-36 | Update Tour Package | Tour Operator | WEB_CONFIRMED | None | YES — later batch | §3.8.2.2 Next.js Tour Package update form; reuse editor and lifecycle guards. |
| UC-37 | Submit Tour for Approval | Tour Operator | WEB_CONFIRMED | None | REUSE/ACTION — later batch | §3.8.2.3 Next.js tour submission confirmation/action; Staff approves via UC-59. |
| UC-38 | Create Coupon | Tour Operator | WEB_CONFIRMED | None | YES — later batch | §3.8.3.1 Next.js create coupon form. |
| UC-39 | Update Coupon | Tour Operator | WEB_CONFIRMED | None | REUSE — later batch | §3.8.3.2 Next.js coupon update; reuse coupon form. |
| UC-40 | View Customer Bookings | Tour Operator | WEB_CONFIRMED | None | YES — later batch | §3.8.4.1 Next.js Operator customer bookings; scope limited to owned tours. |
| UC-41 | Cancel Tour Booking | Tour Operator | WEB_CONFIRMED | None | REUSE/DIALOG — later batch | Live §3.8.4.2 adds Traveler self-cancellation, tiered BR135/BR157 refund preview and MSG170 confirmation below 72 hours, then routes pre-transfer refunds to UC-67 and post-transfer refunds to UC-42. Table 3 still lists only Tour Operator; residual text still says UC-42 executes the gateway refund. PARTIALLY_RESOLVED. |
| UC-42 | Confirm Booking Refund | Tour Operator | WEB_CONFIRMED | None | YES/REUSE — later batch | Live §3.8.4.3 makes this Operator bank-return confirmation within three working days and TripMate receipt confirmation, not gateway execution. Its data-processing/abnormal text, BR76 and undefined message placeholders still retain the old gateway model. PARTIALLY_RESOLVED; do not conflate with UC-67. |
| UC-43 | Check In Traveler by QR Code | Tour Operator | MOBILE_ONLY | None | NO Web screen | §3.8.4.4: camera-based QR check-in scanning is Flutter-only. |
| UC-44 | View Revenue Report | Tour Operator | WEB_CONFIRMED | None | YES — later batch | §3.8.5.1 Next.js operator revenue report, only own tours. |
| UC-45 | Export Revenue Report | Tour Operator | WEB_CONFIRMED | None | REUSE/ACTION — later batch | §3.8.5.2 Next.js revenue export dialog/action. |
| UC-46 | View User Accounts | Staff/ Administrator | INTERNAL_WEB_ONLY | None | YES — role approval hold | Table 3 and authorization Table 5 grant Staff + Administrator role-scoped account views, but §3.9.1.1 says Administrator only. ROLE_CONFLICT; do not implement disputed access. |
| UC-47 | Lock User Account | Staff/ Administrator | INTERNAL_WEB_ONLY | None | REUSE/DIALOG — hold permissions | Table 3 and Table 5 grant Staff + Administrator role-scoped locking, but §3.9.1.2 says Administrator only. ROLE_CONFLICT. |
| UC-48 | Unlock User Account | Staff/ Administrator | INTERNAL_WEB_ONLY | None | REUSE/DIALOG — hold permissions | Table 3 and Table 5 grant Staff + Administrator role-scoped unlocking, but §3.9.1.3 says Administrator only. ROLE_CONFLICT. |
| UC-49 | Approve Tour Operator Application | Staff | INTERNAL_WEB_ONLY | None | REUSE/ACTION — role hold | Table 3, trigger and actor text say Staff; the same §3.9.2.1 credentials, BR115/BR07/BR118, flow and post-conditions say Administrator. ROLE_CONFLICT. |
| UC-50 | Reject Tour Operator Application | Staff | INTERNAL_WEB_ONLY | None | REUSE/DIALOG — role hold | Table 3, actor and processing text say Staff; §3.9.2.2 trigger, credentials, BR115/BR118, flow and post-conditions say Administrator. ROLE_CONFLICT. |
| UC-51 | Create POI | Staff/ Administrator | INTERNAL_WEB_ONLY | None | YES — later batch | Table 3 and the §3.9.3.1 actor/purpose text name Staff and Administrator, while the same detailed section's credentials, BR-115, flow and validation remain Administrator-only. Current FE and BE creation authorization is Staff-only. `MISMATCH`; no SRS-compliance claim. |
| UC-52 | Update POI | Staff/ Administrator | INTERNAL_WEB_ONLY | None | REUSE — later batch | §3.9.3.2: STAFF and Administrator may update POIs. |
| UC-53 | Remove POI | Administrator | INTERNAL_WEB_ONLY | None | REUSE/DIALOG — later batch | Live §3.9.3.3 and BR121 consistently require hard deletion only when unreferenced and deactivation when referenced; previous lifecycle ambiguity is resolved by the current SRS. |
| UC-54 | Create Route | Staff | INTERNAL_WEB_ONLY | None | YES — later batch | §3.9.4.1: STAFF creates routes; old matrix incorrectly had UC-54 as Remove POI. |
| UC-55 | Update Route | Staff | INTERNAL_WEB_ONLY | None | REUSE — later batch | §3.9.4.2: STAFF updates routes. |
| UC-56 | Configure Algorithm Parameters | Administrator | INTERNAL_WEB_ONLY | None | YES — later batch | §3.9.5: Administrator configures scheduling/matching algorithm parameters. |
| UC-57 | View Active Trips | Staff | INTERNAL_WEB_ONLY | None | YES — later batch | §3.9.6.1: STAFF active trips monitor; no invented intervention rights. |
| UC-58 | View Active Trip Details | Staff | INTERNAL_WEB_ONLY | None | YES — later batch | §3.9.6.2: STAFF active trip details; read-oriented. |
| UC-59 | Approve Tour Post | Staff | INTERNAL_WEB_ONLY | None | REUSE/ACTION — later batch | §3.9.7.1: STAFF approves Tour Posts. |
| UC-60 | Reject Tour Post | Staff | INTERNAL_WEB_ONLY | None | REUSE/DIALOG — later batch | §3.9.7.2: STAFF rejects Tour Posts with reason. |
| UC-61 | View Platform Bookings | Staff | INTERNAL_WEB_ONLY | None | YES — later batch | §3.9.8: STAFF platform-wide booking monitoring; no unapproved booking interventions. |
| UC-62 | Update Landing Page Content | Staff | INTERNAL_WEB_ONLY | None | YES — later batch | §3.9.10: STAFF maintains public landing-page content. |
| UC-63 | Export Statistical Reports | Administrator | INTERNAL_WEB_ONLY | None | REUSE/ACTION — later batch | §3.9.11: Administrator exports platform statistical reports. |
| UC-64 | View Audit Logs | Administrator | INTERNAL_WEB_ONLY | None | YES — later batch | §3.9.12.1: Administrator audit log list/search. |
| UC-65 | View Audit Log Details | Administrator | INTERNAL_WEB_ONLY | None | YES — later batch | §3.9.12.2: Administrator audit log details. |
| UC-66 | Handle Weather Warning and Operator Cancellation | Traveler / Tour Operator | WEB_WORKFLOW | None | YES/REUSE — placement hold | Live Table 3, §3.10.1 and non-screen #13 now agree TripMate warns only and never auto-cancels. §3.10.1 nevertheless authorizes Operator **or Administrator**, while Table 3 omits Admin; screen ownership/rescheduling and the stale “emergency cancellation” service reference remain unresolved. PARTIALLY_RESOLVED. |
| UC-67 | Process Refund to Traveler | Traveler | NON_SCREEN | None | NO dedicated page — reuse status | Live §3.10.2 makes System + gateway execute pre-transfer refunds directly or post-transfer refunds after UC-42 receipt; Table 3 still lists Traveler, and §3.8.4.2/BR76 retain stale UC-42 gateway wording. PARTIALLY_RESOLVED; status belongs on existing booking views. |
| UC-68 | Submit Post-Trip Complaint | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.8.6.1: Traveler complaint Web and Flutter form; 3-day deadline and evidence constraints. |
| UC-69 | Respond to Post-Trip Complaint | Tour Operator | WEB_CONFIRMED | None | YES — later batch | §3.8.6.2: Tour Operator console complaint response; full/partial/reject and deadlines. |
| UC-70 | Synchronize Offline Trip Data | Traveler | MOBILE_ONLY | None | NO Web screen | §3.10.3 offline Mobile synchronization banner/result; no Web screen. |
| UC-71 | Schedule Payout Settlement | System | NON_SCREEN | None | NO dedicated page | Live Table 3, BR141 and non-screen #5 specify one automatic per-booking settlement after the 24-hour window at the first daily 10:00 run. §3.10.4 Normal Flow is accidentally copied from UC-70 offline sync, and legacy manual payout material remains. PARTIALLY_RESOLVED; do not add Operator request UI. |
| UC-72 | View Payout Records | Staff/ Administrator | INTERNAL_WEB_ONLY | None | YES — role/flow hold | Live Table 3 defines read-only per-booking records for Staff/Admin, but §3.9.9.1 remains a Staff-only manual payout-request queue. ROLE_AND_FLOW_CONFLICT. |
| UC-73 | Review Failed Payout Settlement | Administrator | INTERNAL_WEB_ONLY | None | REUSE — owner review | Table 3 lists Administrator failed-payout review but no corresponding named detailed function section. UI/actions NOT_VERIFIED. |
| UC-74 | Process Payout Settlement | System | NON_SCREEN | None | NO dedicated page | Live Table 3, §3.10.5 and non-screen #6 specify automatic daily 10:00 transfer with hourly retry up to `[N]`; §3.9.9.3 and screen/auth tables retain Administrator confirmation and manual requests. PARTIALLY_RESOLVED; do not implement approval UI as canonical. |
| UC-75 | Respond to Partial Refund Proposal | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | §3.8.6.3: Traveler accepts/declines partial refund proposal on Web or Flutter within 3 days. |

## Classification Totals

| Classification | Count |
|---|---:|
| `SHARED_WEB_MOBILE` | 21 |
| `WEB_CONFIRMED` | 12 |
| `MOBILE_ONLY` | 14 |
| `INTERNAL_WEB_ONLY` | 22 |
| `NON_SCREEN` | 4 |
| `CONFLICT_WEB_MOBILE` | 1 |
| `WEB_WORKFLOW` | 1 |
| **Total** | **75** |

## Public Web Screen Outside the UC List

| Screen | Actor | Existing Web Screen Spec | Requirement | Design Coverage |
|---|---|---|---|---|
| TripMate Landing Page | Guest | `ux/web-screen-specifications/tripmate-landing-page-web-screen-spec.md` | §3.1.2.1, Table 4.1; content managed via UC-62 by Staff | Batch 1 screen spec exists; implementation NOT_VERIFIED |

## Proposed Web UI Batches (Planning Only)

1. **Batch 1 — Public + Authentication:** Landing Page, UC-01–04 and UC-06; UC-05 action in shells; UC-07 deferred. Existing six Web screen specifications are design artifacts only.
2. **Batch 2 — Traveler:** UC-08–12, UC-24–29, UC-32–33, UC-68 and UC-75; UC-67 refund status reused in booking details. **UC-31 HOLD**.
3. **Batch 3 — Tour Operator:** UC-34–42, UC-44–45, UC-66 operator cancellation interaction, UC-69; UC-43 is Mobile-only. Do not invent Tour Operator payout requests.
4. **Batch 4 — Staff operations:** role-disputed UC-46–48, UC-49–52, UC-54–55, UC-57–62, UC-72 (read-only; role conflict).
5. **Batch 5 — Administrator governance:** disputed UC-46–48, UC-53, UC-56, UC-63–65, UC-72–73. Payout manual-vs-auto conflict requires owner decision.

## Mandatory Flow Delta Audit

- **UC-71/74 payout:** live Table 3 and §3.10.4 BR141 use one settlement per paid, confirmed booking after a 24-hour `Awaiting Window`, admitted to the first daily 10:00 run; cancellations during the window are excluded and routed to UC-67 under BR142. The catalogue amount deducts the fixed 10% commission and pending partner debt, and BR146 makes the calculated amount immutable. §3.10.5/BR145 retries gateway transfer hourly up to unresolved `[N]`, then marks `SettlementFailed` for UC-73 escalation. BR139 remains definitionally inconsistent: detailed refund text defines a refund-share formula, while consolidated §5.1 defines an aggregated payout-request formula. Consolidated BR141–146 entries are absent.
- **UC-41/42/67 refund:** live §3.8.4.2 BR135 uses refund tiers of 100% (>14 days), 50% (7–14 days), 30% (72 hours–7 days) and 0% (<72 hours); BR157 provides a 100% exception within 24 hours after payment. A below-72-hour cancellation shows a 0% preview and requires confirmation. Pre-transfer cancellation routes directly to UC-67 (BR142); post-transfer cancellation enters `Awaiting Partner Return`, requires UC-42 Operator return within three working days, and reaches UC-67 only after TripMate confirms the full credited amount (BR139/143/144). BR154 applies the 72-hour booking/payment cutoff; BR156/157 remain part of this policy set. Residual §3.8.4.2/BR76 and §3.8.4.3 abnormal text still assign gateway work to UC-42, so the handoff is not fully coherent.
- **UC-66 weather:** live Table 3 and §3.10.1 make detection a warning only; the Tour Operator decides whether to continue, reschedule or cancel, and Travelers are notified. §3.10.1 also permits an Administrator declaration even though Table 3 does not list Admin. Operator cancellation is 100% refundable and should route through the UC-41/42/67 model, but the detailed flow does not fully specify rescheduling and contains stale refund-submission wording.
- **UC-46–51 authorization:** BR168 now distinguishes Staff lock/unlock of Traveler/Operator accounts from Administrator lock/unlock of Traveler/Operator/Staff accounts, while §§3.9.1.1–3 remain Administrator-only. UC-49/50 retain Staff-vs-Administrator contradictions. UC-51 remains Staff/Admin in catalogue and actor/purpose text, Administrator-only in the same detailed section, and Staff-only in observed FE/BE authorization.

## Live Screen, Message and Interface Reconciliation

- **Web screen inventory (Table 4.1):** live rows still include #28 `Payout Records List`, #29 `Payout Details`, #30 `Confirm Payout Settlement`, #39 `Payout Request Review`, #40 `Approve Payout Request Confirmation` and #41 `Reject Payout Request Confirmation`. This contradicts the 2026-11-08 change-log claim that #30/#39–41 were removed and #28/#29 renamed.
- **Mobile inventory (Table 4.2):** live rows still include #91 `Payout Settlement History` and #92 `Payout Request`; #92 was not removed as claimed.
- **Authorization (Table 5):** the same legacy payout screens remain authorized, so the actual table does not yet express an automatic-only UC-71/74 model.
- **Non-screen functions:** #5/#6 now point to automatic UC-71/74 and #13 is warning-only UC-66. However, #12 still routes weather monitoring toward an “emergency cancellation process,” and the service label for #13 remains stale.
- **Application messages (Table 5.3):** the live table ends at MSG156 and contains no definition for MSG157–172. Detailed flows retain 16 placeholders: `[MSG-new-A]`–`[D]`, `[F]`–`[P]` and `[Y]`. MSG170–172 are referenced but undefined; their copy and acceptance behavior are `NOT_VERIFIED`.
- **External interfaces and quality requirements:** §4.1 confirms payment-gateway and weather-service dependencies; §4.2 confirms Web browser compatibility and WCAG 2.2 AA for Web administration. These constraints do not resolve the disputed actors, refund handoff, cancellation ownership or payout approval model.

## Live SRS Conflict Reconciliation

`Resolution status` classifies every prior conflict against the verified live binary; it does not mean the implementation is approved.

| Conflict | Affected UCs | Resolution status | Live evidence | Matrix treatment |
|---|---|---|---|---|
| C-01 | UC-04–07 | STILL_CONFLICTING | Table 3/Table 5 include Staff; §§3.2.4–3.2.7 list Traveler, Operator and Administrator but omit Staff. | STAFF_AUTH_CONFLICT; do not infer security permissions. |
| C-02 | UC-12 | STILL_CONFLICTING | Table 3/Table 5 include Guest; detailed POI browsing names Traveler. | Preserve Guest catalogue scope pending owner decision. |
| C-03 | UC-31 | STILL_CONFLICTING | Product Overview calls commercial-service booking Mobile-specific; §3.6.2 explicitly names Flutter and Next.js. | `CONFLICT_WEB_MOBILE`; Web design HOLD. |
| C-04 | UC-34–45 | STILL_CONFLICTING | Product Overview permits Operator Web/Mobile; detailed Operator interfaces are predominantly Next.js Web. | Web is confirmed; Mobile inclusion/exclusion remains undecided. |
| C-05 | UC-41 | STILL_CONFLICTING | Table 3 actor remains Tour Operator while §3.8.4.2/Table 5 also grant Traveler self-cancellation. | Keep distinct owned/customer permission scopes. |
| C-06 | UC-46–48 | PARTIALLY_RESOLVED | Table 3/Table 5 and new BR168 define Staff/Admin role scopes, but triggers, credentials, validations and flows remain Administrator-only. | No access-policy change without approval. |
| C-07 | UC-49–50 | STILL_CONFLICTING | Table 3/actor text say Staff; §3.9.2.1–2 credentials, BR115/BR118 and flows say Administrator (UC-50 trigger also says Administrator). | Keep catalogue actors; authorization HOLD. |
| C-08 | UC-53 | RESOLVED_BY_CURRENT_SRS | §3.9.3.3 and BR121 consistently delete only unreferenced POIs and deactivate referenced POIs. | Use the conditional lifecycle; no policy hold remains. |
| C-09 | UC-66 | PARTIALLY_RESOLVED | Table 3/§3.10.1/non-screen #13 now agree warning-only/no auto-cancel; §3.10.1 still permits Admin and Operator while Table 3 omits Admin, and non-screen #12/name remain stale. | Web workflow retained; ownership/rescheduling HOLD. |
| C-10 | UC-67 | PARTIALLY_RESOLVED | §3.10.2 now defines pre/post-transfer gateway execution, but Table 3 actor and stale §3.8.4.2/BR76 handoff disagree. | Reuse status UI; no dedicated page or duplicate execution. |
| C-11 | UC-72 | STILL_CONFLICTING | Table 3 defines Staff/Admin read-only per-booking records; §3.9.9.1 remains a Staff-only manual payout-request queue. | Preserve read-only catalogue scope. |
| C-12 | UC-71/73/74 | PARTIALLY_RESOLVED | Table 3, BR141/145 and non-screen #5/#6 now define automatic per-booking daily settlement; actor descriptions, §§3.9.9.1–3 and screen/auth/message inventory retain manual requests/review/confirmation. | Automatic model is traceability baseline; legacy approval UI is non-compliant evidence. |
| C-13 | UC-71/74 | RESOLVED_BY_CURRENT_SRS | Current Table 3/§§3.10.4–5 use daily 10:00 and hourly retry; the former seven-days-before-departure wording is absent. | Remove the obsolete timing claim. |
| C-14 | UC-73 | STILL_CONFLICTING | Table 3 names Administrator failed-settlement review; no corresponding detailed named function defines UI/actions. | `NOT_VERIFIED`; do not invent controls. |
| C-15 | UC-42/67/70–74 | PARTIALLY_RESOLVED | Some BR references changed, but BR139 still collides between refund-share and consolidated payout formula; §3.10.4 still reuses BR136 in copied sync text; `[N]` and missing consolidated rules remain. | BUSINESS_RULE_COLLISION; formulas/retries need BA correction. |
| C-16 | UC-42/66/71–74 | STILL_CONFLICTING | Tables 4.1/4.2/5 still contain manual payout screens and stale labels; non-screen #12 and legacy messages remain, despite updated catalogue/non-screen rows. | Treat stale inventory only as conflict evidence. |
| C-17 | UC-02/03 | RESOLVED_BY_CURRENT_SRS | Current detailed sections explicitly support Web and Mobile. | `SHARED_WEB_MOBILE`. |
| C-18 | UC-42 | PARTIALLY_RESOLVED | Name is now Confirm Booking Refund and the main flow uses bank return, but stale gateway-processing text and undefined messages remain. | Correct name retained; handoff remains blocked. |
| C-19 | UC-46–75 | RESOLVED_BY_CURRENT_SRS | Live Table 3 contains exactly 75 canonical IDs through UC-75. | Current numbering retained. |
| C-20 | UC-51 | STILL_CONFLICTING | Table 3 and §3.9.3.1 actor/purpose say Staff/Admin; credentials, BR115, flow and validation say Admin-only; observed FE/BE source authorizes Staff-only. | `IMPLEMENTED_SOURCE_CONTRACT_CONFLICT`; no compliance claim. |
| C-21 | UC-71 | NEW_CONFLICT | §3.10.4 Normal Flow is copied from UC-70 offline synchronization (local records, sync banner, MSG154–156, BR136). | Disregard copied flow for UI authority; require SRS correction. |
| C-22 | UC-42/67–69/74 | NEW_CONFLICT | Change log claims all placeholders replaced and MSG157–172 added, but Table 5.3 ends at MSG156 and detailed flows retain `[MSG-new-A]`–`[D]`, `[F]`–`[P]` and `[Y]`. | Message copy/validation is `NOT_VERIFIED`; no invented text. |
| C-23 | UC-71–74 | NEW_CONFLICT | Change log says payout screens 30, 39–41 and 92 were removed and 28/29 renamed; live Tables 4.1/4.2/5 still contain the old names and screens. | Inventory removal/renaming is not accepted as complete. |
| C-24 | UC-46–48 | NEW_CONFLICT | §3.9.1.1 routes locking to UC-48 and unlocking to UC-49; canonical IDs are UC-47 and UC-48. | Treat these as stale cross-references. |

## Existing Web Design Evidence

- `docs/BATCH_1_WEB_SCREEN_INVENTORY.md`: historical Batch 1 screen/route choices; not implementation proof.
- `docs/batch-1-message-reconciliation.md`: message conflicts must remain visible.
- `ux/web-screen-specifications/`: Web-specific specs observed for public/auth Batch 1. Mobile-first `ux/screen-specifications/` or Stitch prompts are not automatically Web deliverables.
- `Application Submitted` remains a UC-02 state; UC-06 uses progressive recovery; UC-07 remains deferred.
- Prior `READY FOR STITCH: YES` applies to the old Batch 1 design review only and does not certify UC-46–75 or the new conflict list.

## Governance and Approval Gate

This is a **traceability update**, not a change to the official SRS, source code, Jira or deployed screens. Before design or implementation decisions affecting any disputed UC, obtain traceable Owner approval and record the chosen precedence. In particular: Staff account permissions, UC-31 Web availability, UC-66 cancellation UI ownership, the UC-41/42/67 refund handoff, and UC-71–74 payout authority.

**Implementation verification:** targeted FE/BE source evidence was reviewed read-only for contract comparison, including the UC-51 Staff-only proxy/controller behavior. Deployment and live-environment behavior remain `NOT_VERIFIED`; source presence is not production proof. Final requirements approval is blocked by the live SRS conflicts C-01–C-24, not by unexamined source drift.
