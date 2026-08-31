# TripMate Web UI Coverage Matrix

## Purpose and Authority

This matrix audits the current UC inventory in **Report 3 — Software Requirement Specification** for responsive Web UI coverage. Report 3 is the platform authority. Existing mobile-first screen specifications and Stitch prompts remain valid behavioral references for Flutter; they are not treated as Web artifacts and are not modified by this audit.

The matrix covers UC-01 through UC-72. The TripMate Landing Page is listed separately because Report 3 defines it as a Web screen, not as its own use case.

## Classification Legend

- `WEB`: Web is explicitly required by the canonical UC; mobile support is not established by that UC.
- `MOBILE_ONLY`: Report 3 explicitly limits the function to Flutter Mobile.
- `NON_SCREEN`: The UC is an action or system-triggered process and does not require a dedicated page. It may still produce a message, state, dialog, or update on another screen.
- `SHARED_WEB_MOBILE`: Report 3 supports the function on both responsive Web and Flutter Mobile.
- `ADMIN_WEB_ONLY`: Administrator function provided through the Web Administration System only.

`Existing Web Screen` refers to a dedicated Web Screen Specification in `ux/web-screen-specifications/`, not to an existing mobile-first artifact or implemented application page.

## Web UC Coverage

| UC | Use Case | Actor | Web Supported | Existing Web Screen | Web Screen Needed | Notes |
|---|---|---|---|---|---|---|
| UC-01 | Register Traveler Account | Guest | SHARED_WEB_MOBILE | Traveler Registration; Verify Account | YES — Batch 1 | Web entry is the public landing page. Standard registration creates Pending Verification; Google registration creates an Active account. |
| UC-02 | Register Tour Operator Account | Guest | WEB | Tour Operator Registration with Application Submitted state; Operator Application Status handoff | YES — Batch 1 | Canonical UC 3.2.2 explicitly places this flow on responsive Next.js Web. Application Submitted is not a standalone page. |
| UC-03 | Resubmit Tour Operator Application | Tour Operator | WEB | Operator Application Status; Resubmit Application | YES — Batch 1 | Reuses the company-information and document-upload structure from UC-02; no new account or application is created. |
| UC-04 | Sign In | Traveler / Tour Operator / Administrator | SHARED_WEB_MOBILE | Public Sign In; Admin Login | YES — Batch 1 | Public Sign In requires email/password and Google. Admin Login requires email/password; Administrator Google authentication is not established and is excluded from required controls. |
| UC-05 | Sign Out | Traveler / Tour Operator / Administrator | NON_SCREEN | None | NO dedicated screen | Navigation action that terminates the current session; confirmation/message belongs to the active workspace shell. |
| UC-06 | Reset Password | Traveler / Tour Operator / Administrator | SHARED_WEB_MOBILE | Progressive Password Recovery page: Request, Reset Code Entry, Set New Password, Success states | YES — Batch 1 | Canonical Web behavior uses an email-delivered, single-use 15-minute reset code. Separate verification/password routes are unnecessary. |
| UC-07 | Change Password | Traveler / Tour Operator / Administrator | SHARED_WEB_MOBILE | None | DEFERRED | UC-07 will be specified with the authenticated workspace shell batch. This does not block Batch 1 Stitch readiness. |
| UC-08 | Update Traveler Profile | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Place in Traveler workspace Profile. |
| UC-09 | Update Travel Preferences | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Place in Traveler workspace Preferences. |
| UC-10 | Create Scheduling Request | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Web trip-planning form; output continues to UC-11. |
| UC-11 | View Suggested Itinerary | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Web itinerary detail/timeline; may reuse POI detail handoff to UC-12. |
| UC-12 | Explore Points of Interest | Guest / Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Public/Traveler list and detail views can share routes with different authenticated actions. |
| UC-13 | Navigate Route | Traveler | MOBILE_ONLY | None | NO | Real-time GPS navigation is explicitly mobile-only. |
| UC-14 | Receive Real-Time Alerts | Traveler | MOBILE_ONLY | None | NO | Active-trip real-time alert handling is explicitly mobile-only. |
| UC-15 | Confirm Re-routing | Traveler | MOBILE_ONLY | None | NO | Real-time rerouting confirmation is explicitly mobile-only. |
| UC-16 | Download Offline Map & Itinerary | Traveler | MOBILE_ONLY | None | NO | Offline device map/itinerary capability is explicitly mobile-only. |
| UC-17 | Create Travel Group | Traveler | MOBILE_ONLY | None | NO | Travel group management is explicitly mobile-only in the canonical UC. |
| UC-18 | Invite Group Members | Traveler | MOBILE_ONLY | None | NO | Invitation code/QR group flow is explicitly mobile-only. |
| UC-19 | View Group Members | Traveler | MOBILE_ONLY | None | NO | Shared travel group membership view is explicitly mobile-only. |
| UC-20 | Remove Group Member | Traveler | MOBILE_ONLY | None | NO | Group-host removal action is explicitly mobile-only. |
| UC-21 | Leave Travel Group | Traveler | MOBILE_ONLY | None | NO | Leave-group action is explicitly mobile-only. |
| UC-22 | Configure Group Location Sharing | Traveler | MOBILE_ONLY | None | NO | Real-time GPS sharing is explicitly mobile-only. |
| UC-23 | Join Shared Group Trip | Traveler | MOBILE_ONLY | None | NO | Invitation-code/QR join flow is explicitly mobile-only. |
| UC-24 | Search Tours | Guest / Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Public and Traveler Web views should reuse one tour-search information architecture. |
| UC-25 | Receive Tour Recommendations | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Traveler-only personalized results; continue to UC-26. |
| UC-26 | View Tour Details | Guest / Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Public detail page; booking handoff requires Traveler authentication. |
| UC-27 | Book Tour | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Booking form/confirmation reached from Tour Details. |
| UC-28 | Make Electronic Payment | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Web uses redirect URL; Mobile uses deep link; both reconcile the same gateway result. |
| UC-29 | View QR E-ticket | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Ticket can be displayed on Web; scanning/check-in remains mobile-only under UC-43. |
| UC-30 | View Commercial Service | Traveler | MOBILE_ONLY | None | NO | Canonical UC explicitly limits commercial-service discovery to Mobile. |
| UC-31 | Book Commercial Service | Traveler | MOBILE_ONLY | None | NO | Canonical UC explicitly limits commercial-service booking to Mobile. |
| UC-32 | View Trip History | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Traveler workspace history list/detail. |
| UC-33 | Submit Trip Review | Traveler | SHARED_WEB_MOBILE | None | YES — later batch | Reuse eligible Trip History/Trip Detail context where possible. |
| UC-34 | Update Tour Operator Profile | Tour Operator | SHARED_WEB_MOBILE | None | YES — later batch | Place in Tour Operator workspace Profile. |
| UC-35 | Create Tour Package | Tour Operator | SHARED_WEB_MOBILE | None | YES — later batch | Tour editor in Tour Operator workspace. |
| UC-36 | Update Tour Package | Tour Operator | SHARED_WEB_MOBILE | None | YES — later batch | Reuse the Tour editor with lifecycle-aware editability. |
| UC-37 | Submit Tour for Approval | Tour Operator | SHARED_WEB_MOBILE | None | REUSE/ACTION — later batch | Submission action belongs to Tour Preview/Details; do not create a standalone page mechanically. |
| UC-38 | Create Coupon | Tour Operator | SHARED_WEB_MOBILE | None | YES — later batch | Coupon form in Tour Operator workspace. |
| UC-39 | Update Coupon | Tour Operator | SHARED_WEB_MOBILE | None | REUSE — later batch | Reuse Coupon form with eligible existing values loaded. |
| UC-40 | View Customer Bookings | Tour Operator | SHARED_WEB_MOBILE | None | YES — later batch | Booking list and booking/passenger detail. |
| UC-41 | Cancel Customer Booking | Tour Operator | SHARED_WEB_MOBILE | None | REUSE/DIALOG — later batch | Confirmation dialog launched from eligible Booking Details. |
| UC-42 | Initiate Booking Refund | Tour Operator | SHARED_WEB_MOBILE | None | REUSE/DIALOG — later batch | Refund action/form belongs to an eligible cancelled Booking Details context. |
| UC-43 | Check In Traveler by QR Code | Tour Operator | MOBILE_ONLY | None | NO | Requires device camera at the meeting point; explicitly mobile-only. |
| UC-44 | View Revenue Report | Tour Operator | SHARED_WEB_MOBILE | None | YES — later batch | Revenue & Analytics workspace screen. |
| UC-45 | Export Revenue Report | Tour Operator | SHARED_WEB_MOBILE | None | REUSE/ACTION — later batch | Export controls belong to Revenue & Analytics; no separate page unless approved later. |
| UC-46 | Request Payout Settlement | Tour Operator | SHARED_WEB_MOBILE | None | YES — later batch | Payout history/detail plus request action/form. |
| UC-47 | View User Accounts | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | User Accounts List and User Account Details. |
| UC-48 | Lock User Account | Administrator | ADMIN_WEB_ONLY | None | REUSE/DIALOG — later batch | Action and confirmation dialog on User Account Details. |
| UC-49 | Unlock User Account | Administrator | ADMIN_WEB_ONLY | None | REUSE/DIALOG — later batch | Action and confirmation dialog on User Account Details. |
| UC-50 | Approve Tour Operator Application | Administrator | ADMIN_WEB_ONLY | None | REUSE/ACTION — later batch | Action on Operator Application Details. |
| UC-51 | Reject Tour Operator Application | Administrator | ADMIN_WEB_ONLY | None | REUSE/DIALOG — later batch | Rejection-reason dialog on Operator Application Details. |
| UC-52 | Create POI | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Create form under POI master-data module. |
| UC-53 | Update POI | Administrator | ADMIN_WEB_ONLY | None | REUSE — later batch | Reuse POI form with existing data. |
| UC-54 | Remove POI | Administrator | ADMIN_WEB_ONLY | None | REUSE/DIALOG — later batch | Deactivate/remove confirmation from POI detail/list; lifecycle decision remains authoritative. |
| UC-55 | Create Route | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Create route form within Route Management. |
| UC-56 | Update Route | Administrator | ADMIN_WEB_ONLY | None | REUSE — later batch | Reuse route form with existing data. |
| UC-57 | Configure Algorithm Parameters | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | One configuration screen; no algorithm implementation controls. |
| UC-58 | View Active Trips | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Active Trips Monitor. |
| UC-59 | View Active Trip Details | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Read-oriented operational detail; no intervention actions unless separately approved. |
| UC-60 | Approve Tour Post | Administrator | ADMIN_WEB_ONLY | None | REUSE/ACTION — later batch | Action on Tour Content & Pricing Details. |
| UC-61 | Reject Tour Post | Administrator | ADMIN_WEB_ONLY | None | REUSE/DIALOG — later batch | Rejection/revision-reason dialog on Tour Details. |
| UC-62 | View Platform Bookings | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Platform booking list and details; read-oriented. |
| UC-63 | View Payout Records | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Payout Records List. |
| UC-64 | View Payout Details | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Payout Details; amount is backend-calculated and read-only. |
| UC-65 | Confirm Payout Settlement | Administrator | ADMIN_WEB_ONLY | None | REUSE/DIALOG — later batch | Confirmation action on eligible Payout Details. |
| UC-66 | Update Landing Page Content | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Admin content-management screen; public Landing Page is separately specified in Batch 1. |
| UC-67 | Export Statistical Reports | Administrator | ADMIN_WEB_ONLY | None | REUSE/ACTION — later batch | Statistical Reports page with supported export controls. |
| UC-68 | View Audit Logs | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Audit log list/search/filter. |
| UC-69 | View Audit Log Details | Administrator | ADMIN_WEB_ONLY | None | YES — later batch | Read-only Audit Log Details. |
| UC-70 | Handle Emergency Tour Cancellation | Traveler / Tour Operator | NON_SCREEN | None | NO dedicated screen | System-triggered process; effects are surfaced in existing trip/tour/booking status and notifications. |
| UC-71 | Process Automatic Refund | Traveler | NON_SCREEN | None | NO dedicated screen | System-triggered refund process; status belongs to existing booking/payment views. |
| UC-72 | Synchronize Offline Trip Data | Traveler | NON_SCREEN | None | NO dedicated screen | Background Mobile synchronization; no Web screen and no dedicated Mobile page required by the UC. |

## Public Web Screen Outside the UC List

| UC | Use Case | Actor | Web Supported | Existing Web Screen | Web Screen Needed | Notes |
|---|---|---|---|---|---|---|
| N/A | TripMate Landing Page | Guest | WEB | TripMate Landing Page | YES — Batch 1 | Canonical screen in Report 3 section 3.1.2.1. Provides public entry points to POIs, tours, authentication, Traveler registration, and Tour Operator registration. Content is administratively maintained through UC-66. |

## Batch Plan

1. **Batch 1 — Public + Authentication:** Landing Page, UC-01, UC-02, UC-03, UC-04, UC-06. UC-07 is deferred until authenticated shells are defined.
2. **Batch 2 — Traveler planning / tour discovery:** UC-08–12, UC-24–29, UC-32–33.
3. **Batch 3 — Tour Operator workspace:** UC-34–42, UC-44–46.
4. **Batch 4 — Admin core management:** UC-47–57, UC-60–62, UC-66.
5. **Batch 5 — Admin monitoring / financial / reporting:** UC-58–59, UC-63–65, UC-67–69.

## Source Conflicts Affecting Coverage

1. Report 3 Product Overview states that Guests, Travelers, and Tour Operators can use either application, and the mobile-oriented screen inventory includes Tour Operator registration/status. However, the canonical UC-02 and UC-03 definitions explicitly name responsive Next.js Web interfaces. This matrix uses the more specific UC definitions (`WEB`) and leaves the existing Mobile artifacts untouched.
2. Report 3 section 3.1.2.2 is captioned “Web Application Screen List,” but several rows explicitly describe Mobile screens and mobile-only capabilities. Platform classification therefore comes from the Product Overview and each detailed UC, not from the table caption alone.
3. UC-04 defines one authentication use case for all roles, while the screen inventory separately lists `Admin Login` and `Sign In`. Batch 1 preserves one authentication behavior but specifies two Web presentation contexts: public authentication and Admin authentication.
4. Existing requirement/MVP/flow/screen artifacts for UC-01 through UC-07 contain older decisions that conflict with Report 3, including Traveler registration fields/verification, supported sign-in methods, and reset mechanics. Web specifications use Report 3 as primary scope and retain the older documents only as behavioral references.
5. Several detailed UC sections reference `MSG` codes whose locked content in Report 3 section 5.3 describes a different event. Web specifications call out these mismatches rather than silently changing a code or inventing replacement copy.

## Batch 1 Reconciliation Status

- Final screen/state decisions and route classifications: `docs/BATCH_1_WEB_SCREEN_INVENTORY.md`.
- Complete application-message audit: `docs/batch-1-message-reconciliation.md`.
- `Application Submitted` is a UC-02 success state.
- UC-06 uses one progressive Password Recovery page in public and Admin shell contexts.
- Google Authentication is an external handoff for public authentication and is not a required Admin Login control.
- UC-07 remains deferred to authenticated workspace shell design.
- **READY FOR STITCH: YES** for the six Batch 1 Web Screen Specification files, subject to the placeholder-copy rules in the message reconciliation document.
