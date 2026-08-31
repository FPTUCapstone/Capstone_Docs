# Batch 1 Web Screen Inventory

## Final Inventory

| # | Screen | Related UC | Actor | Type | Proposed Route | Stitch Ready |
|---|---|---|---|---|---|---|
| 1 | TripMate Landing Page | Public SRS screen; UC-66 content source | Guest | PAGE | `/` | YES |
| 2 | Public Sign In | UC-04 | Guest with Traveler or Tour Operator account | PAGE | `/sign-in` | YES |
| 3 | Admin Login | UC-04 | Administrator | PAGE | `/admin/login` | YES |
| 4 | Google Authorization | UC-01, UC-04 public authentication | Guest | EXTERNAL_HANDOFF | External Google Authentication Service | NO — not a TripMate Stitch screen |
| 5 | Traveler Registration | UC-01 | Guest | PAGE | `/register` | YES |
| 6 | Verify Account | UC-01 | Guest with Pending Verification account | PAGE | `/verify-account` | YES |
| 7 | Tour Operator Registration | UC-02 | Business Guest | PAGE | `/partner/register` | YES |
| 8 | Application Submitted | UC-02 | Newly registered Tour Operator applicant | STATE | Within Tour Operator Registration, then `/partner/application` | YES |
| 9 | Operator Application Status | UC-02, UC-03 | Authenticated Tour Operator applicant | PAGE | `/partner/application` | YES |
| 10 | Resubmit Application | UC-03 | Tour Operator with latest application in Rejected status | PAGE | `/partner/application/resubmit` | YES |
| 11 | Public Password Recovery | UC-06 | Unauthenticated Traveler or Tour Operator | PAGE | `/forgot-password` | YES |
| 12 | Admin Password Recovery | UC-06 | Unauthenticated Administrator | PAGE | `/admin/forgot-password` | YES |
| 13 | Reset Code Entry | UC-06 | User who requested password recovery | STATE | Within originating Password Recovery page | YES |
| 14 | Set New Password | UC-06 | User with verified reset code | STATE | Within originating Password Recovery page | YES |
| 15 | Password Reset Success | UC-06 | User whose password was reset | STATE | Within originating Password Recovery page, then matching Sign In | YES |
| 16 | Change Password | UC-07 | Authenticated Traveler, Tour Operator, or Administrator | PAGE | DEFERRED | NO — authenticated workspace shell batch |

## Final UC → Screen Decisions

| UC | Current Screen | Decision | Reason |
|---|---|---|---|
| Public SRS screen | TripMate Landing Page | KEEP | Report 3 defines a public Web landing screen and approved public handoffs. |
| UC-01 | Traveler Registration | KEEP | Required data-entry surface with distinct account-creation validation. |
| UC-01 | Verify Account | KEEP | Standard registration creates Pending Verification; the Guest must submit an emailed code and may return from Sign In/resend handling. |
| UC-02 | Tour Operator Registration | KEEP | Required long-form account/company/document submission. |
| UC-02 | Application Submitted | STATE_ONLY | Report 3 requires confirmation and Pending Review/Pending Approval communication, but does not require a separate standalone page. Continue to Application Status. |
| UC-02 / UC-03 | Operator Application Status | KEEP | Required to represent Pending Review, Rejected, and Approved outcomes and gate resubmission/workspace entry. |
| UC-03 | Resubmit Application | KEEP | Distinct authenticated correction task with lengthy pre-filled company/document form; Report 3 names a Resubmit Application screen. |
| UC-04 | Public Sign In | KEEP | Public Traveler/Tour Operator authentication has public registration and recovery handoffs. |
| UC-04 | Admin Login | KEEP | Report 3 lists a separate Admin Web login context; no public registration actions are allowed. |
| UC-04 | Google Authorization | KEEP | External provider handoff only; it is not a TripMate page or Stitch target. |
| UC-06 | Forgot Password | MERGE | Becomes the Request state of one progressive Password Recovery page. |
| UC-06 | Enter Reset Code | STATE_ONLY | Linear next state in the same recovery task; no independent direct-entry requirement is approved. |
| UC-06 | Set New Password | STATE_ONLY | Linear next state after successful code verification; remains in the same recovery shell. |
| UC-07 | Change Password | DEFER | Web support is approved, but cross-role Account Settings placement belongs with authenticated workspace shells. It does not block the rest of Batch 1. |

## Authentication Decisions

| Context | Required methods/controls | Excluded or unresolved |
|---|---|---|
| Public Sign In | Email Address, Password, Remember me, Sign In, Continue with Google, Forgot Password, Register | Phone/OTP sign-in is not present in the latest detailed UC-04 screen layout. |
| Admin Login | Email Address, Password, Remember me, Sign In, Forgot Password | Google authentication for Administrator is not explicitly established. Do not include the control in Stitch unless separately approved. |
| Traveler Registration | Standard form plus Continue with Google | Google provider UI is external. |

## Post-login Navigation

| Role | Required destination behavior | Proposed route | Requirement or proposal |
|---|---|---|---|
| Traveler | Direct to the Traveler Home / Dashboard entry screen after successful authentication. | `/traveler` | Destination behavior is required by role resolution and the SRS screen inventory; route is a proposal. |
| Tour Operator — approved/Active | Direct to Tour Operator Dashboard. | `/operator` | Destination behavior is required by role resolution and the SRS screen inventory; route is a proposal. |
| Tour Operator — Pending Approval or Rejected | Direct to Operator Application Status; operational workspace functions remain locked. | `/partner/application` | Status/restriction behavior is required; route is a proposal. |
| Administrator | Direct to Admin Dashboard. | `/admin` | Destination behavior is required by role resolution and the SRS screen inventory; route is a proposal. |

## Route Review

No Batch 1 route is documented as `APPROVED_EXISTING`; Report 3 defines screen destinations but does not approve URL paths.

| Route | Classification | Reason |
|---|---|---|
| `/` | PROPOSED | Conventional public root for the approved Landing Page. |
| `/sign-in` | PROPOSED | Public authentication path. |
| `/admin/login` | PROPOSED | Separate Admin authentication context. |
| `/register` | PROPOSED | Traveler Registration path. |
| `/verify-account` | PROPOSED | Resumable account-verification destination. |
| `/partner/register` | PROPOSED | Tour Operator Registration path. |
| `/partner/application/submitted` | UNNECESSARY | Application Submitted is a state, not a standalone page. |
| `/partner/application` | PROPOSED | Operator Application Status destination. |
| `/partner/application/resubmit` | PROPOSED | Authenticated correction form. |
| `/forgot-password` | PROPOSED | Public progressive Password Recovery page. |
| `/admin/forgot-password` | PROPOSED | Admin-context progressive Password Recovery page. |
| `/verify-reset-code` | UNNECESSARY | Reset Code Entry is a Password Recovery state. |
| `/reset-password` | UNNECESSARY | Set New Password is a Password Recovery state. |
| `/admin/verify-reset-code` | UNNECESSARY | Admin Reset Code Entry is a state. |
| `/admin/reset-password` | UNNECESSARY | Admin Set New Password is a state. |
| Change Password route | DEFERRED | Define with authenticated workspace Account Settings architecture. |

## Shared Web Conventions

- Public pages share one TripMate travel identity: deep navy, teal, coral accent, light neutral surfaces, and restrained geographic/map-aware visual language.
- Admin authentication shares the same form controls, typography, accessibility, and message treatment, but uses a professional operational context rather than consumer travel promotion.
- Labels remain sentence/title case as written in Report 3: Email Address, Password, Confirm Password, Full Name, Phone Number, Business Licence Number, Tax Code, and related operator fields.
- Field errors appear adjacent to the relevant control; form-level business/system messages use a consistent status region.
- Submitting controls expose a loading state and prevent duplicate submission.
- Success is a state first; create a separate page only when the user must revisit it or it is a durable task destination.
- Missing/ambiguous locked messages use neutral placeholder copy marked as UX suggestion; locked text is never rewritten.
- No visible session-expiry behavior is defined for Batch 1. This is non-blocking for static Stitch design and must not be invented.
- All pages specify 1440px, 1280px, and 768px behavior, visible keyboard focus, accessible labels, error association, and no horizontal scrolling.
