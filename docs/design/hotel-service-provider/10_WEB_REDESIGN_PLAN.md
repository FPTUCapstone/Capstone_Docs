# 10 — TRIPMATE HOTEL WORKFLOW WEB REDESIGN PLAN
## Anthropic Frontend-Design Methodology (Pass 1 — Design Thinking)
### Next.js Web Administration & Service Provider Workspace (W102, W103, W104, W105, W106)

---

## 1. Product Context, User Audience & Operational Roles

TripMate is an intelligent travel planning platform operating in Central Vietnam (Da Nang, Hoi An, Hue). While travelers discover and book hotels via mobile devices, the commercial hotel workflow is executed through a Next.js responsive web application by two strictly separated actors:

### 1.1 The Hotel Service Provider (Receptionist & Hotel General Manager)
- **Role:** Front desk receptionist during operating hours; general manager / hotel accountant during monthly billing cycles.
- **Environment:** Multi-tab browser environment, often running alongside local hotel property management systems (PMS), reservation channels, and front desk check-in registers.
- **Primary Tasks:**
  - Fast scanning of incoming booking requests (W103).
  - Rapid decision-making: verifying internal room availability and confirming or rejecting requests with mandatory reason (W104).
  - Post-stay outcome reporting: recording actual guest checkout spend or no-show status once stay date passes (W104).
  - Monthly financial ledger reconciliation: reviewing commission statements, viewing completed booking breakdowns, and reporting bank transfer reference codes (W105).
- **Core User Need:** Maximum information density, zero layout jitter, tabular numbers, immediate keyboard accessibility, and unambiguous operational state feedback.

### 1.2 The TripMate Internal Staff (Platform Administrator & Financial Operator)
- **Role:** TripMate operational staff member.
- **Environment:** Internal administrative console on desktop displays (1440×900, 1280×800).
- **Primary Tasks:**
  - Provisioning commercial service provider accounts and linking verified commercial Points of Interest (POIs) (W102).
  - Reconciling hotel commission payments: reviewing submitted bank transaction references and verifying credits before confirming payment receipts (W106).
- **Core User Need:** Structured form layouts with clear policy guidance, comprehensive audit traceability, error prevention dialogs, and reliable search/filtering across service providers.

---

## 2. Visual Direction: Authentic Coastal Heritage

### 2.1 Aesthetic Philosophy
The visual language must evoke an authentic commercial hotel operations platform rooted in Central Vietnam hospitality rather than a generic SaaS template. It combines the architectural warmth of Hoi An ancient town (weathered terracotta, Thu Bon river patinas, sand-plastered walls) with the disciplined precision of modern hotel accounting software.

### 2.2 Rejection of Generic AI Design Tropes
| AI-Generated Pattern / Trope | Operational Flaw in Hotel Web UI | TripMate Intentional Redesign Solution |
| :--- | :--- | :--- |
| **SaaS Card Kit:** Wrapping every field and micro-table in identical rounded white cards with soft drop shadows | Destroys vertical scanning; fragments tables; creates enormous wasted whitespace; feels like a hobbyist dashboard. | Full-width, cohesive work surfaces; crisp hairline borders (`1px solid #DCE2DD`); structured two-column grids (65/35 split); data tables spanning the full content width. |
| **Narrow Floating Forms:** 400px–500px forms floating in the center of a 1440px viewport with vast empty margins | Wastes over 60% of available screen real estate; looks unfinished; forces excessive vertical scrolling. | Balanced 65/35 two-column layout: Form fields on the left (65%), contextual operational policy guide on the right (35%). |
| **Sparse 2–3 Row Tables:** Presenting empty-looking tables with minimal demo records | Leaves the table looking abandoned; prevents evaluation of pagination, sticky headers, and responsive wrap. | Populated with 8–12 representative commercial records reflecting realistic Central Vietnam properties, room types, and statuses. |
| **Native `<select>` Dropdowns:** OS-native blue/grey select menus with clipped options | Clashes with the design language; cannot support search, multiselect, or custom metadata; cannot be keyboard-styled. | Custom accessible dropdown component suite (`CustomSelect`, `SearchableMultiSelect`) styled with TripMate tokens and full ARIA support. |
| **Unapproved Policy Hardcoding:** Hardcoding 24h countdowns, 10% commission rates, or 1:1 POI restrictions | Misrepresents unapproved team proposals as finalized business rules, violating SRS Section 10 governance. | Parameterized labels and clear informational notes distinguishing approved rules (BR-196 direct payment) from pending proposals (BR-192, BR-194, BR-198). |
| **Language Inconsistency:** Mixing Vietnamese and English or ignoring CR-09 | Violates canonical SRS CR-09 specification requiring English UI for this version. | 100% consistent English UI text with structured localization readiness. |

---

## 3. Core Color Token System & Discrepancy Resolution

### 3.1 Design Token Discrepancy Clarification
> [!IMPORTANT]
> **Token Reconciliation Notice:** Deliverable `08_VISUAL_REFINEMENT_REPORT.md` referenced a primary teal of `#006B5F`. That token originated from the Mobile V3 Concept B (Travel Editorial) palette (`--tm-mobile-teal`).
> For the Web Service Provider and Staff Administration workspace, the authoritative, verified palette established in `04_DESIGN_SYSTEM.md` is strictly preserved: **`--color-teal-primary: #174B4A`** and **`--color-teal-deep: #103B39`**. The two palettes are not mixed.

### 3.2 Authoritative Web Coastal Heritage Palette
```css
:root {
  /* Brand Primary & Secondary Teals */
  --color-teal-primary: #174B4A;   /* Primary actions, active navigation, focused headers */
  --color-teal-deep:    #103B39;   /* Button hover states, active sidebar highlight */
  --color-teal-tint:    #E8F2F1;   /* Soft table hover, active row selection */
  --color-teal-muted:   #3D6B69;   /* Supporting interactive indicators */

  /* Terracotta & Warm Copper Accents */
  --color-copper-accent: #B96D4C;  /* Attention callouts, pending status indicators */
  --color-copper-text:   #995436;  /* High-contrast urgency text, highlighted pricing */
  --color-copper-soft:   #FFF2EB;  /* Pending notification backgrounds */

  /* Neutral Canvas & Surfaces */
  --color-canvas-sand:   #F7F5EF;  /* Warm page background, eliminates screen glare */
  --color-surface-pure:  #FFFFFF;  /* Cards, tables, modal surfaces, inputs */
  --color-surface-subtle:#FDFCF9;  /* Contextual sidebar backgrounds, read-only wells */

  /* Typography & Ink Hierarchy */
  --color-ink-primary:   #1C2928;  /* Primary headings, data values, table cell text */
  --color-ink-secondary: #5B6E69;  /* Field labels, column headers, metadata */
  --color-ink-muted:     #8E9E9B;  /* Placeholder text, disabled labels */
  --color-ink-inverted:  #FFFFFF;  /* Text on dark buttons and badges */

  /* Hairlines & Borders */
  --color-border-subtle: #DCE2DD;  /* Table row dividers, container borders, input borders */
  --color-border-strong: #B8C4BE;  /* Focused input borders, active tabs */
  --color-border-focus:  #174B4A;  /* Accessible focus ring */

  /* Semantic Status Tokens (Accessible Dual-Encoding) */
  --status-pending-bg:   #FEF3E2;  --status-pending-text:   #92400E;
  --status-confirmed-bg: #E6F7F0;  --status-confirmed-text: #065F46;
  --status-rejected-bg:  #FDECEF;  --status-rejected-text:  #991B1B;
  --status-cancelled-bg: #F1F5F9;  --status-cancelled-text: #475569;
  --status-completed-bg: #E8F7F4;  --status-completed-text: #103B39;
  --status-noshow-bg:    #F1EBFE;  --status-noshow-text:    #5B21B6;
  --status-reported-bg:  #FEF3E2;  --status-reported-text:  #B45309;
  --status-paid-bg:      #E6F7F0;  --status-paid-text:      #065F46;
}
```

---

## 4. Typography System & Hierarchy

The web interface uses a dual-font typographic architecture:
- **Body & Interface:** `Inter`, `-apple-system`, `Segoe UI`, `Roboto`, `sans-serif` (Optimal x-height, clear differentiation between numerals and glyphs, clean rendering in dense tables).
- **Numeric & Codes:** `Consolas`, `Menlo`, `Monaco`, `Courier New`, `monospace` (Tabular alignment for request codes, bank transaction references, monetary figures).

### Typographic Scale
| Level | Font Size / Line Height | Weight | Letter Spacing | Applied Elements |
| :--- | :---: | :---: | :---: | :--- |
| **H1 Page Title** | 22px / 28px | 700 (Bold) | -0.02em | Screen main titles (W102, W103, W104, W105, W106) |
| **H2 Section Title** | 16px / 22px | 600 (SemiBold) | -0.01em | Table section headers, panel headings |
| **H3 Card / Modal Title**| 14px / 20px | 600 (SemiBold) | 0.00em | Modal dialog titles, detail section labels |
| **Body Primary** | 13.5px / 19px | 400 (Regular) | 0.00em | Table cell text, form input content |
| **Body Medium** | 13.5px / 19px | 500 (Medium) | 0.00em | Guest names, service titles |
| **Table Header (TH)** | 11.5px / 16px | 600 (SemiBold) | +0.02em | Table column titles (uppercase, muted ink) |
| **Form Label** | 12.5px / 16px | 600 (SemiBold) | 0.00em | Input labels with mandatory `*` indicators |
| **Helper / Caption** | 11.5px / 15px | 400 (Regular) | 0.00em | Validation feedback, audit timestamps, secondary metadata |
| **Monospace Data** | 12.5px / 16px | 600 (SemiBold) | +0.01em | `HBR-20261113-001`, `VCB-20261205-982341`, currency values |

---

## 5. Spacing, 12-Column Grid & Layout Architecture

### 5.1 Eliminating Whitespace & Narrow Forms
Previous screens suffered from forms constrained to 500px–720px floating inside a 1440px canvas, leaving 50%–60% of the screen blank.
The redesign resolves this by:
1. **Full-Width Operational Tables (W103, W105 Master, W106):** Tables utilize 100% of the content container width with balanced column distributions, eliminating awkward right-side voids.
2. **65/35 Two-Column Split Architecture (W102, W104, W105 Detail):**
   - **Primary Action Column (65%):** Data entry forms, request parameters, operational decision buttons, and transaction ledgers.
   - **Contextual Side Panel (35%):** Business rule guidelines, audit logs, guest identity cards, payment instructions, and policy notes.
3. **Structured 8px Spacing Grid:** Spacing tokens: `4px`, `8px`, `12px`, `16px`, `20px`, `24px`, `32px`.

### 5.2 Application Shell Dimensions
- **1440px Viewport (Desktop):** 250px fixed navigation sidebar + 1190px fluid content area (30px padding).
- **1280px Viewport (Laptop):** 230px fixed navigation sidebar + 1050px fluid content area (24px padding).
- **1024px Viewport (Tablet Landscape):** 64px compact icon rail + 960px content area with internal table horizontal scrolling.

---

## 6. Strict Role Separation & Workspace Architecture

The system enforces strict role and workspace isolation:

```
[TripMate Web Platform]
  ├── Service Provider Workspace (Hotel Receptionist / General Manager)
  │     ├── W103: Service Booking Requests (Table, Filters, Status)
  │     ├── W104: Service Booking Request Details & Operational Outcome
  │     └── W105: Commission Statements & Payment Reporting Ledger
  └── Staff Administration Workspace (TripMate Staff Operator)
        ├── W102: Create Service Provider Account & POI Linking
        └── W106: Commission Reconciliation & Receipt Verification
```
*No cross-workspace navigation, no generic dashboard, no tour operator links, and no hotel PMS inventory management.*

---

## 7. Mandatory Custom Dropdown Component Architecture

All native `<select>` controls are eliminated and replaced by a unified, accessible custom dropdown component suite:

### 7.1 `CustomSelect`
- **Used For:** W102 Category, W103 Status Filter, W106 Service Provider Filter, W106 Period Filter, W106 Status Filter.
- **Features:**
  - Custom trigger surface with TripMate border tokens (`#DCE2DD`), hover highlight (`#174B4A`), and custom SVG chevron.
  - Floating menu positioned via fixed/absolute coordinates with high `z-index: 1200` to prevent table clipping.
  - Active checkmark indicator on selected option.
  - Full keyboard accessibility: `ArrowDown`/`ArrowUp` to navigate, `Enter`/`Space` to select, `Escape` to close.

### 7.2 `SearchableMultiSelect` / `CustomCombobox`
- **Used For:** W102 Linked Points of Interest (POIs).
- **Features:**
  - Integrated search filter input at the top of the dropdown menu.
  - Selection tags/chips inside trigger box with remove buttons.
  - Rich option items displaying: POI Name, POI Identifier (`POI-DANANG-0042`), Category badge, Physical Address, and Active Status indicator.
  - "Select All" / "Clear All" convenience actions.

---

## 8. Screen-by-Screen ASCII Wireframes

### 8.1 Screen W102 — Create Service Provider Account (Staff)
```
+----------------------------------------------------------------------------------------------------+
| TRIPMATE STAFF ADMIN  |  Create Service Provider Account                    [Staff: John Smith v]  |
+-----------------------+----------------------------------------------------------------------------+
| [=] Staff Workspace   |  Home > Service Providers > Create Account                                  |
|   * Provider Accounts |  CREATE SERVICE PROVIDER ACCOUNT                                           |
|   * Reconciliation    |  Provision a commercial partner account and link active commercial POIs.   |
|                       +---------------------------------------+------------------------------------+
|                       | PRIMARY ACCOUNT PROVISIONING (65%)   | CONTEXTUAL POLICY PANEL (35%)     |
|                       |                                       |                                    |
|                       | Full Name (*)                         | [i] Account Role Information       |
|                       | [ Tran Thi Lan                      ] | Commercial Service Provider        |
|                       |                                       | accounts are provisioned exclusively|
|                       | Business Name (*)       Category (*)  | by TripMate Staff (UC-77).         |
|                       | [ Hoi An Riverside R. ] [ Hotel     v]|                                    |
|                       |                                       | [*] Security Credentials           |
|                       | Email Address (*)       Phone (*)     | A temporary password will be       |
|                       | [ lan.tran@hotel.vn   ] [ 0905123456] | generated and emailed to the user. |
|                       |                                       | Password must be changed on first  |
|                       | LINKED COMMERCIAL POI(s) (*)          | sign-in. Staff never sees password.|
|                       | [ Search and select active POIs...  v]|                                    |
|                       |   [x] Hoi An Riverside (POI-0042) [x] | [*] Audit Logging                  |
|                       |                                       | Account creation is permanently    |
|                       | Note: POI linking configuration (BR-  | recorded under BR-174 audit trail. |
|                       | 192 pending Owner approval).          |                                    |
|                       |                                       +------------------------------------+
|                       | [ Cancel ]       [ Create Account & Dispatch Credentials ]                 |
+-----------------------+----------------------------------------------------------------------------+
```

### 8.2 Screen W103 — Service Booking Requests (Service Provider)
```
+----------------------------------------------------------------------------------------------------+
| TRIPMATE RECEPTION    |  Service Booking Requests            [Hoi An Riverside Resort | Front Desk]|
+-----------------------+----------------------------------------------------------------------------+
| [=] Hotel Workspace   |  BOOKING REQUESTS (UC-78)                                                  |
|   * Booking Requests  |  Manage incoming traveler reservation requests and record service outcomes.|
|   * Statements        +----------------------------------------------------------------------------+
|                       | FILTERS: [ Status: All v ] [ Date From: dd/mm ] [ Date To: dd/mm ]         |
|                       |          [ Keyword search: code, guest...       ]  [ Search ]  [ Reset ]   |
|                       +----------------------------------------------------------------------------+
|                       | REQ CODE | SERVICE & ROOM     | GUEST NAME  | DATE & TIME  | QTY | STATUS | ACTION |
|                       |----------+--------------------+-------------+--------------+-----+--------+--------|
|                       | HBR-0082 | Hoi An Riverside   | Nguyen An   | 15/12 14:00  |  1  | Pending|[View >]|
|                       |          | Deluxe River King  | 0912345678  |              |     |        |        |
|                       | HBR-0045 | Hoi An Riverside   | Le Nam      | 10/11 14:00  |  1  | Confirm|[View >]|
|                       |          | Superior Garden    | 0903888999  | (Stay passed)|     |        |        |
|                       | HBR-0012 | Hoi An Riverside   | Trinh Mai   | 02/11 14:00  |  2  | Complet|[View >]|
|                       | HBR-0009 | Hoi An Riverside   | David Miller| 28/10 14:00  |  1  | Rejected       |
|                       | ... (8-12 representative records populated)                                |
|                       +----------------------------------------------------------------------------+
|                       | Showing 1 - 10 of 28 requests          [ < Previous ] [ 1 ] [ 2 ] [ Next > ]|
+-----------------------+----------------------------------------------------------------------------+
```

### 8.3 Screen W104 — Request Details & Operational Actions (Service Provider)
```
+----------------------------------------------------------------------------------------------------+
| TRIPMATE RECEPTION    |  < Back to Booking Requests                                                |
+-----------------------+----------------------------------------------------------------------------+
| [=] Hotel Workspace   |  REQUEST HBR-20261113-0082                [ STATUS: PENDING CONFIRMATION ] |
|   * Booking Requests  |  Submitted: 13/11/2026 14:15 | Direct payment at hotel upon check-in (BR-196) |
|   * Statements        +---------------------------------------+------------------------------------+
|                       | RESERVATION PARAMETERS (62%)          | GUEST INFO & ACTIONS (38%)         |
|                       |                                       |                                    |
|                       | Property: Hoi An Riverside Resort     | Guest: Nguyen Van An               |
|                       | Room:     Deluxe River View King      | Phone: 0912 345 678                |
|                       | Check-in: 15/12/2026 at 14:00         | Email: an.nguyen@example.com       |
|                       | Quantity: 1 Room                      | Special Request: Quiet room.       |
|                       | Est. Total: 1,850,000 VND (Ref only)  |                                    |
|                       |                                       +------------------------------------+
|                       | [ STATE A: PENDING ACTIONS ]          | [ STATE B: RECORD OUTCOME ]        |
|                       | [ Confirm Request ]  [ Reject Req ]   | (Visible only after stay date)     |
|                       | (Opens CR-05 Modal) (Opens Reason Box)| Actual Amount: [ 1,850,000 VND ]   |
|                       |                                       | [ Complete Stay ] [ Mark No-Show ] |
+-----------------------+----------------------------------------------------------------------------+
```

### 8.4 Screen W105 — Commission Statements & Ledger (Service Provider)
```
+----------------------------------------------------------------------------------------------------+
| TRIPMATE RECEPTION    |  Commission Statements Ledger                                              |
+-----------------------+----------------------------------------------------------------------------+
| [=] Hotel Workspace   |  COMMISSION STATEMENTS (UC-82)                                             |
|   * Booking Requests  |  Review monthly statements and report wire transfer references.            |
|   * Statements        +---------------------------------------+------------------------------------+
|                       | STATEMENT LEDGER (60%)                | STATEMENT DETAIL & REPORTING (40%) |
|                       |                                       |                                    |
|                       | PERIOD  | COMPLETED | AMOUNT | STATUS | Statement: COMM-202611-0012        |
|                       |---------+-----------+--------+--------| Period:    01/11/2026 - 30/11/2026 |
|                       | 11/2026 | 24 stays  | 4.44M  | Issued | Stays:     24 completed bookings   |
|                       | 10/2026 | 18 stays  | 3.25M  | Paid   | Commission: 4,440,000 VND          |
|                       | 09/2026 | 21 stays  | 3.90M  | Paid   | Status:    Issued (Due: 07/12)     |
|                       | 08/2026 | 15 stays  | 2.70M  | Paid   |                                    |
|                       |                                       | TripMate Bank Account (DEMO):      |
|                       |                                       | Bank: Vietcombank - CN Da Nang     |
|                       |                                       | Account: 0071001234567 (DEMO ONLY) |
|                       |                                       |                                    |
|                       |                                       | Bank Transfer Reference (*):       |
|                       |                                       | [ VCB-20261205-982341            ] |
|                       |                                       | [ Report Payment Reference ]       |
+-----------------------+----------------------------------------------------------------------------+
```

### 8.5 Screen W106 — Commission Reconciliation (Staff)
```
+----------------------------------------------------------------------------------------------------+
| TRIPMATE STAFF ADMIN  |  Commission Reconciliation Workstation               [Staff: Operator John]|
+-----------------------+----------------------------------------------------------------------------+
| [=] Staff Workspace   |  COMMISSION RECONCILIATION (UC-83)                                         |
|   * Provider Accounts |  Verify partner bank transfers and record official payment receipts.       |
|   * Reconciliation    +----------------------------------------------------------------------------+
|                       | FILTERS: [ Provider: All v ] [ Period: All v ] [ Status: Reported v ]      |
|                       |          [ Search reference or provider...      ]  [ Filter ]  [ Reset ]   |
|                       +----------------------------------------------------------------------------+
|                       | PROVIDER          | PERIOD  | DUE DATE | STATUS     | BANK REF       | ACT |
|                       |-------------------+---------+----------+------------+----------------+-----|
|                       | Hoi An Riverside  | 11/2026 | 07/12/26 | Reported   | VCB-20261205...| [R] |
|                       | Da Nang Beachfront| 11/2026 | 07/12/26 | Reported   | TCB-998821...  | [R] |
|                       | Hue Imperial Stays| 10/2026 | 07/11/26 | Paid       | VCB-112233...  | [v] |
|                       | ... (8-10 representative records populated)                                |
|                       +----------------------------------------------------------------------------+
|                       | [ Action [R]: Record Receipt -> Opens CR-05 Modal -> Updates to Paid ]     |
+-----------------------+----------------------------------------------------------------------------+
```

---

## 9. Critique Against Generic AI Patterns & Pass 1 Approval

- **Layout Grid:** Verified 12-column layout with 65/35 master-detail splits eliminates empty floating forms.
- **Visual Warmth:** Coastal Heritage palette grounded in Central Vietnam architecture replaces cold, sterile white-card SaaS templates.
- **Component Quality:** Custom accessible dropdowns (`CustomSelect`, `SearchableMultiSelect`) replace crude browser native pickers.
- **Requirements Compliance:** Strict adherence to canonical SRS (CR-09 English language, BR-196 direct payment, parameterized pending policies BR-192/BR-194/BR-198).
- **Pass 1 Gate:** Fully satisfied. Proceed to Pass 2 (Build & Critique).
