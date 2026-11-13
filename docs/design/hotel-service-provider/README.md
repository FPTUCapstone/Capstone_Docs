# TripMate Hotel & Commercial Service Provider Workflow — UI/UX Prototype

This directory contains the high-fidelity interactive prototype, visual specifications, design system tokens, and QA verification evidence for the **TripMate Hotel Booking & Commercial Service Provider Workflow** across Web and Mobile platforms.

---

## 1. Quick Start: How to Open & Explore the Prototype

1. Navigate to [`prototype/index.html`](prototype/index.html) and open it directly in any modern web browser (Google Chrome, Microsoft Edge, Safari, Firefox).
2. **Presentation Mode (Default):**
   - The interface opens in clean, distraction-free product mode representing the authentic end-user experience without developer toolbars or frame borders.
3. **Review Mode (Inspector):**
   - Click the floating **`[⚙️ Review Mode]`** pill in the bottom-right corner of the window.
   - **Role Switcher:** Switch between **Hotel Provider** (Web W103, W104, W105), **Staff Admin** (Web W102, W106), and **Traveler Mobile** (M74, M75).
   - **Viewport Picker:** Test responsive viewports across Desktop (`1440×900`), Laptop (`1280×800`), Tablet Landscape (`1024×768`), and Mobile devices (`390×844`, `360×800`, `412×915`).
   - **Interactive Actions:** Filter booking requests, confirm/reject requests, record checkout revenue, report bank wire transfers, and reconcile commission statements.
   - **Reset:** Click `Reset` in the top bar to return all mock state to the canonical SRS baseline.

---

## 2. Screen Inventory & Strict Role Separation

The prototype enforces strict workspace and role boundaries in compliance with the canonical Software Requirement Specification (original Report 3):

```
TripMate Platform
 ├── Traveler Mobile Application (Flutter)
 │     ├── M74: Commercial Services Search & List (UC-30)
 │     └── M75: Hotel Details (A), Booking Request Form (B), Result (C), Outcomes (D) (UC-31)
 │
 ├── Hotel Service Provider Workspace (Next.js Responsive Web)
 │     ├── W103: Service Booking Requests Queue (UC-78)
 │     ├── W104: Booking Request Details, Decision & Outcome (UC-79, UC-80)
 │     └── W105: Commission Statements & Payment Reporting (UC-82)
 │
 └── Staff Administration Console (Next.js Responsive Web)
       ├── W102: Create Service Provider Account & POI Linking (UC-77)
       └── W106: Commission Reconciliation Workstation (UC-83)
```
*Note: UC-81 (Monthly Commission Statement Generation) is an automated system background job (§3.10.7) and does not have an interactive user screen.*

---

## 3. Design System & Layout Architecture

### 3.1 Web Coastal Heritage Tokens
Established in [`04_DESIGN_SYSTEM.md`](04_DESIGN_SYSTEM.md) and applied across all Web screens:
- **Primary Brand Teal:** `#174B4A` (WCAG AAA against white)
- **Deep Hover Teal:** `#103B39`
- **Terracotta Copper Accent:** `#B96D4C` / Text: `#995436`
- **Warm Sand Canvas:** `#F7F5EF`
- **Pure Surface:** `#FFFFFF`
- **Primary Ink:** `#1C2928` / Secondary Ink: `#5B6E69`
- **Subtle Borders:** `#DCE2DD`

### 3.2 Anti-Generic-AI Layout Overhaul
- **Eliminated Narrow Floating Forms:** W102 features a 65% / 35% two-column split with contextual policy guidance.
- **Full-Width Data Tables:** W103 and W106 expand across the available 1190px workspace width with fixed, proportional column allocations.
- **State-Dependent Action Hierarchy:** W104 surfaces actions only when valid for the current lifecycle state (Confirm/Reject for Pending; Outcome recording for Confirmed post-stay date; read-only summaries for Terminal states).
- **Master-Detail Financial Ledger:** W105 presents a 60% / 40% ledger and statement detail breakdown with prominent DEMO bank account markings.
- **Reusable Custom Dropdowns:** Replaced all native `<select>` controls with custom-styled `CustomSelect` and `SearchableMultiSelect` components.

---

## 4. Requirement Traceability Matrix (Canonical SRS Report 3)

| Screen | Actor | Use Case | SRS Section | Approved Rules | Pending Approvals |
| :---: | :--- | :--- | :---: | :--- | :--- |
| **M74–M75** | Traveler | UC-30, UC-31 | §3.6.1, §3.6.2 | BR-187–BR-191, BR-196 (Direct Payment) | — |
| **W102** | Staff | UC-77 | §3.9.1.5 | BR-174 (Audit), MSG01, MSG05, MSG201 | BR-192 (1:1 POI restriction) |
| **W103** | Service Provider | UC-78 | §3.6.3 | CR-01 (Pagination), CR-07 (Date/Time), MSG128 | BR-194 (24h response auto-expiry) |
| **W104** | Service Provider | UC-79, UC-80 | §3.6.4, §3.6.5 | BR-196 (Direct Payment), CR-05, MSG123, MSG206 | BR-197 (3-day auto-complete), BR-198 (10% rate) |
| **W105** | Service Provider | UC-82 | §3.6.6 | MSG172, MSG208 | BR-198 (10% rate), BR-199 (Monthly/7-day term) |
| **W106** | Staff | UC-83 | §3.6.7 | BR-174 (Audit), CR-05 (Confirmation), MSG209 | BR-200 (15-day auto-lockout) |

---

## 5. Visual QA Evidence & Screenshots

All screenshot captures are archived in:
- **Web Redesign Suite (19 captures):** [`screenshots/web-redesign/`](screenshots/web-redesign/)
- **Preserved Baseline Suite (18 captures):** [`screenshots/baseline/`](screenshots/baseline/)

Key captures include:
- W102 Account Provisioning: [`screenshots/web-redesign/web-w102-create-provider-1440x900.png`](screenshots/web-redesign/web-w102-create-provider-1440x900.png)
- W103 Operational Table: [`screenshots/web-redesign/web-w103-booking-requests-1440x900.png`](screenshots/web-redesign/web-w103-booking-requests-1440x900.png)
- W103 Custom Dropdown Open: [`screenshots/web-redesign/web-w103-filter-dropdown-open-1440x900.png`](screenshots/web-redesign/web-w103-filter-dropdown-open-1440x900.png)
- W104 Pending Decision: [`screenshots/web-redesign/web-w104-pending-decision-1440x900.png`](screenshots/web-redesign/web-w104-pending-decision-1440x900.png)
- W104 Rejection Modal: [`screenshots/web-redesign/web-w104-rejection-modal-1440x900.png`](screenshots/web-redesign/web-w104-rejection-modal-1440x900.png)
- W104 Record Outcome: [`screenshots/web-redesign/web-w104-record-outcome-1440x900.png`](screenshots/web-redesign/web-w104-record-outcome-1440x900.png)
- W105 Commission Ledger: [`screenshots/web-redesign/web-w105-commission-ledger-1440x900.png`](screenshots/web-redesign/web-w105-commission-ledger-1440x900.png)
- W105 Payment Reported: [`screenshots/web-redesign/web-w105-payment-reported-1440x900.png`](screenshots/web-redesign/web-w105-payment-reported-1440x900.png)
- W106 Reconciliation Workstation: [`screenshots/web-redesign/web-w106-reconciliation-1440x900.png`](screenshots/web-redesign/web-w106-reconciliation-1440x900.png)
- W106 Confirm Receipt Modal: [`screenshots/web-redesign/web-w106-receipt-modal-1440x900.png`](screenshots/web-redesign/web-w106-receipt-modal-1440x900.png)

---

## 6. Known Limitations & Governance Status Taxonomy

To ensure transparent engineering handoff, all aspects of the prototype are categorized using strict project governance tags:

| Category | Status Tag | Item / Description |
| :--- | :---: | :--- |
| **Visual Design** | `VISUAL PASS` | W102, W103, W104, W105, W106 layouts, typography, tables, and color tokens meet high-fidelity criteria. |
| **Approved Business Rules** | `VISUAL PASS` | Direct hotel payment upon check-in (BR-196) clearly communicated across traveler and provider screens. |
| **Language Specification** | `SRS MISMATCH` | Canonical SRS §5.2 Common Requirements mandates English UI for this version. The M74/M75 prototype retains approved Vietnamese text. Recorded as an open mismatch; visual design preserved without unauthorized modification. |
| **Common Requirements Reference** | `SRS MISMATCH` | Previous documentation referenced "CR-09" for English language. In canonical Report 3, Common Requirements are CR-01 (Pagination), CR-05 (Confirmation), CR-06 (Toast), and CR-07 (Date/Time); language is specified directly under §5.2 text. |
| **Pagination Traversal** | `FUNCTIONAL NOT VERIFIED` | Pagination summary indicators render per CR-01, but multi-page state traversal (> 1 page) is mock-limited and not verified against backend pagination APIs. |
| **CustomSelect ARIA** | `ACCESSIBILITY GAP` | Custom dropdowns feature custom styling and click dismissal, but full WCAG 2.2 AA ARIA 1.2 combobox role mappings, active-descendant focus management, and Home/End keystrokes remain an implementation gap. |
| **Unapproved Revision Rules** | `PENDING OWNER APPROVAL` | BR-192 (1:1 POI), BR-194 (24h auto-expiry), BR-197 (3-day auto-completion), BR-198 (10% rate), BR-199 (7-day payment term), and BR-200 (15-day lockout) represent team proposals awaiting Owner approval. They are parameterized in the prototype and not locked into finalized behavior. |

---

## 7. Deliverables Reference Index

- [`00_SRS_TRACEABILITY.md`](00_SRS_TRACEABILITY.md): Canonical SRS requirement traceability matrix.
- [`01_INFORMATION_ARCHITECTURE.md`](01_INFORMATION_ARCHITECTURE.md): Navigation hierarchy and route mapping.
- [`02_USER_FLOWS.md`](02_USER_FLOWS.md): Detailed user journey diagrams.
- [`03_SCREEN_SPECIFICATIONS.md`](03_SCREEN_SPECIFICATIONS.md): Screen-by-screen specifications.
- [`04_DESIGN_SYSTEM.md`](04_DESIGN_SYSTEM.md): Design tokens, typography, and component styling.
- [`05_INTERACTION_SPECIFICATION.md`](05_INTERACTION_SPECIFICATION.md): Micro-interactions and state models.
- [`06_OPEN_DECISIONS.md`](06_OPEN_DECISIONS.md): Governance and open decision register.
- [`07_DESIGN_QA_REPORT.md`](07_DESIGN_QA_REPORT.md): Initial baseline design QA audit.
- [`08_VISUAL_REFINEMENT_REPORT.md`](08_VISUAL_REFINEMENT_REPORT.md): Human-crafted visual polish pass report.
- [`09_VISUAL_QA_EVIDENCE.md`](09_VISUAL_QA_EVIDENCE.md): Baseline QA evidence matrix.
- [`10_WEB_REDESIGN_PLAN.md`](10_WEB_REDESIGN_PLAN.md): Pass 1 design thinking and ASCII wireframes.
- [`11_WEB_REDESIGN_DECISIONS.md`](11_WEB_REDESIGN_DECISIONS.md): Pass 2 architectural and governance decisions.
- [`12_WEB_REDESIGN_QA_REPORT.md`](12_WEB_REDESIGN_QA_REPORT.md): Visual QA, responsive audit, and compliance evaluation.
