# Web Screen Inventory

| # | Screen | Related UC | Actor | Route Suggestion |
|---|---|---|---|---|
| 1 | Public Sign In | UC-04 | Guest with Traveler or Tour Operator account | `/sign-in` |
| 2 | Admin Login | UC-04 | Administrator | `/admin/login` |

Both screens implement the same canonical authentication behavior. They use different Web entry contexts because Report 3 lists `Admin Login` separately from the Guest-facing sign-in experience.

# Screen: Public Sign In

## Related UC

UC-04 Sign In. It also hands off to UC-01, UC-02, UC-03, and UC-06.

## Actor

Guest holding a Traveler or Tour Operator account.

## Platform

Responsive Next.js Web.

## Purpose

Authenticate a registered public user, apply account-status rules, establish the session, and direct the user to the role-appropriate Web entry screen.

## Entry Conditions

- User is unauthenticated.
- A registered account or linked Google identity exists.

## Entry Points

- TripMate Landing Page.
- Protected public/Traveler/Tour Operator Web route that requires authentication.
- Post-registration or post-password-reset redirect.

## Exit / Navigation

- Active Traveler → Traveler Home / Dashboard.
- Active Tour Operator → Tour Operator Dashboard.
- Pending Approval Tour Operator → Operator Application Status; workspace functions remain locked.
- Rejected Tour Operator → Operator Application Status with resubmission entry.
- Forgot Password → UC-06.
- Register → UC-01 Traveler Registration.
- Tour Operator registration remains accessible through the public shell/landing page.

## Suggested Route

`/sign-in`

Suggested post-authentication routes: `/traveler`, `/operator`, and `/partner/application`. The destination screen behavior is supported by role resolution and the Report 3 screen inventory; the URL paths are architecture proposals.

## Main Layout Regions

1. Public TripMate header with a return to the Landing Page.
2. Authentication context/identity panel using restrained travel imagery or geographic language.
3. Sign-in form panel.
4. Registration and password-recovery handoffs.
5. Inline/form-level status message area.

## Required Data

- Email Address.
- Password.
- Remember-me selection.
- Verified Google identity when that method is selected.
- Account status and assigned role returned by authentication processing.

## Components

### Inputs

- Email Address.
- Password, masked by default with an accessible show/hide control.
- Remember me checkbox.

### Read-only Information

- Screen title and concise authentication guidance.
- Exact validation, access-status, and success messages listed below.

### Primary Actions

- Sign In.
- Continue with Google.

### Secondary Actions

- Forgot Password.
- Register as Traveler.
- Return to Landing Page.
- Reach Register as Tour Operator through the public navigation or an explicitly labeled secondary link.

## Table / List Columns

Not applicable.

## Filters / Search

Not applicable.

## Modal / Dialog Behavior

Google Authentication Service may take the user through an external authorization handoff. Do not design provider implementation details or an internal Google credential form.

## Validation

- Email and Password are required.
- Email must have a valid email format.
- Incorrect email and incorrect password share one non-disclosing credential error.
- After five consecutive failed attempts, the account is temporarily locked for 15 minutes (BR-11).
- Only Active or Pending Approval accounts may obtain an authenticated session; Pending Verification and Locked accounts are refused according to the detailed UC rules.
- A Google identity must match a linked TripMate account.

## Business Rules

- **BR-03:** Submitted password is verified against the stored hash; plain password is never stored or logged.
- **BR-05:** Pending Verification account cannot authenticate until identifier verification.
- **BR-07:** Pending Approval Tour Operator may authenticate, but Tour Operator Workspace functions remain locked until approval.
- **BR-10:** Only Active or Pending Approval accounts may obtain an authenticated session; Locked accounts are refused.
- **BR-11:** Five consecutive failed sign-in attempts cause a 15-minute temporary lock; success resets the counter.
- **BR-12:** Successful sign-in issues access and refresh tokens; the assigned role is used for authorization. Token implementation is not represented in the UI.

## Application Messages

Use the exact locked content from Report 3 section 5.3:

| Code | Content | Web Usage |
|---|---|---|
| MSG01 | `This field is required.` | Empty required field. |
| MSG02 | `Invalid email format. Please enter a valid email address (e.g., user@example.com).` | Invalid Email Address format. |
| MSG09 | `Incorrect email or password. Please try again.` | Non-disclosing credential or unlinked-Google error. |
| MSG10 | `Your Tour Operator account is pending verification. You will be notified once approved.` | Pending Approval Tour Operator status after authentication. |
| MSG11 | `Your account has been locked due to policy violations. Please contact support@tripmate.com.` | Administratively locked/suspended account. |
| MSG12 | `Welcome back to TripMate! Signed in successfully.` | Successful sign-in toast. |
| MSG127 | `TripMate is temporarily unable to process your request. Please check your connection and try again.` | Generic system/authentication failure. |

Report 3 UC-04 references MSG10 for any locked account and MSG11 for Pending Verification, but the locked message list assigns MSG10 to pending Tour Operator verification and MSG11 to a locked account. No exact locked message currently exists for Pending Verification. Do not repurpose either message silently; show this as an unresolved content dependency.

See `docs/batch-1-message-reconciliation.md` for the complete locked-message audit. Missing copy may use neutral placeholder wording only as a UX suggestion; the visible blocked state remains required.

## States

### Initial

Empty form; no validation messages.

### Loading

Authentication is in progress; prevent duplicate submission and preserve entered Email Address.

### Loaded

Form is ready for input.

### Validation Error

MSG01 or MSG02 appears with the relevant field.

### Business Error

- MSG09 for invalid credentials or an unlinked Google identity.
- MSG10 for a Pending Approval Tour Operator status notice.
- MSG11 for an administratively locked/suspended account.
- Pending Verification requires corrected locked copy before final UI generation.

### System Error

MSG127 is shown without creating a session.

### Success

MSG12 appears, then the role/status-based navigation is applied.

### Disabled

Submit actions are disabled only while a submission is in progress; no other disabled rule is specified.

### Unauthorized

An already-authenticated user reaching this route should be directed to the role-appropriate entry screen. This navigation behavior is a UX proposal.

No visible session-expiry treatment is defined in the approved Batch 1 sources. Do not add a session-expired state to Stitch as a requirement.

### Responsive

The authentication panel remains readable and keyboard-accessible at all supported widths.

## Navigation Map

```text
Landing / Protected Handoff
└── Public Sign In
    ├── Active Traveler → Traveler Workspace
    ├── Active Tour Operator → Tour Operator Workspace
    ├── Pending/Rejected Operator → Application Status
    ├── Forgot Password → Forgot Password
    ├── Register → Traveler Registration
    └── Error → Public Sign In
```

## Responsive Behavior

### 1440px

- Use a two-region composition: travel/product context and a focused sign-in form.
- Keep the form width constrained; do not stretch fields across the viewport.

### 1280px

- Preserve the two-region hierarchy with reduced side padding.

### 768px

- Stack or remove nonessential imagery while retaining the public header, form, and all required handoffs.
- Keep actions full-width only where that improves clarity; this remains a Web page, not a Mobile-app shell.

## Open Questions

- What exact locked message should be used for a Pending Verification account?
- What URL paths will implement the approved Traveler Home / Dashboard, Tour Operator Dashboard, and Operator Application Status destinations?
- Does `Remember me` change visible expiry information, or is it only a preference control?
- Should the public sign-in page show both registration types directly or leave Tour Operator registration in the public navigation?

## UX Suggestions

- Maintain one shared authentication visual system across registration and reset flows.
- Keep account-status messages separate from credential validation so a successfully authenticated Pending Approval operator understands the restriction.

# Screen: Admin Login

## Related UC

UC-04 Sign In.

## Actor

Administrator.

## Platform

Responsive Next.js Web.

## Purpose

Authenticate an Administrator into the Web-only operational administration workspace without presenting a consumer travel landing experience.

## Entry Conditions

- Administrator is unauthenticated.
- Administrator account exists and its status permits authentication.

## Entry Points

- Direct Admin workspace entry.
- Redirect from a protected `/admin/*` route.

## Exit / Navigation

- Successful authentication → Admin Dashboard.
- Forgot Password → Admin reset flow implemented by UC-06 behavior.
- Failure → remain on Admin Login.

## Suggested Route

`/admin/login`

## Main Layout Regions

1. Minimal TripMate Admin identity/header.
2. Secure authentication panel.
3. Status/error message region.
4. Password-recovery handoff.

## Required Data

Email Address, Password, Remember me selection, account status, and assigned role.

## Components

### Inputs

- Email Address.
- Password.
- Remember me checkbox.

### Read-only Information

- Admin context label.
- Required application messages.

### Primary Actions

- Sign In.

### Secondary Actions

- Forgot Password.

No public account-registration action belongs in the Admin Login context.

## Table / List Columns

Not applicable.

## Filters / Search

Not applicable.

## Modal / Dialog Behavior

None required. Google authorization is not a required Admin Login control because Report 3 does not explicitly establish Google authentication for Administrator accounts.

## Validation

Email Address and Password are required; Email Address format, credential validation, failed-attempt locking, account-status checks, and role authorization follow UC-04. A successfully authenticated non-Administrator must not enter the Admin workspace.

## Business Rules

BR-03, BR-05, BR-10, BR-11, and BR-12 from UC-04 apply. Role authorization remains enforced after authentication.

The generic UC-04 Google alternative is not sufficient evidence that Administrators use Google authentication. Admin Login requires Email Address and Password only unless a role-specific approval is added later.

## Application Messages

Use MSG01, MSG02, MSG09, MSG11, MSG12, and MSG127 exactly as listed for Public Sign In. MSG10 is specific to Pending Approval Tour Operators and is not an Admin state.

## States

### Initial

Empty Admin login form.

### Loading

Credentials are being verified; duplicate submission is prevented.

### Loaded

Form ready for input.

### Validation Error

MSG01 or MSG02.

### Business Error

MSG09 for invalid credentials; MSG11 for locked/suspended account.

### System Error

MSG127.

### Success

MSG12, then Admin Dashboard.

### Unauthorized

Valid credentials for a non-Administrator must not grant access to `/admin/*`. Exact user-facing copy is not locked in Report 3.

No visible session-expiry treatment is defined for Batch 1.

### Responsive

The operational login panel remains centered and usable at all supported widths.

## Navigation Map

```text
Protected Admin Route → Admin Login
Admin Login ├── Success + Admin role → Admin Dashboard
            ├── Forgot Password → Admin Reset Password
            └── Error / wrong role → Admin Login
```

## Responsive Behavior

### 1440px

- Center a restrained authentication panel within a professional operational background.
- Avoid consumer-tour promotional cards.

### 1280px

- Preserve constrained form width and clear Admin identity.

### 768px

- Use a single-column layout with all form controls and status messages visible without horizontal scrolling.

## Open Questions

- Is Google authentication enabled for Administrator accounts? It is excluded from the required Admin Login design unless explicitly approved.
- What exact message should a valid non-Administrator see after attempting to access Admin Login?
- What is the approved Admin Dashboard route?

## UX Suggestions

- Use a professional operational administration interface rather than a consumer travel page.
- Share form controls, accessibility, and message treatment with Public Sign In while keeping the surrounding context distinct.
