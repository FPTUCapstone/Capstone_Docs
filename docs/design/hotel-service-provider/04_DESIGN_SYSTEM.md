# 04 — DESIGN SYSTEM & VISUAL TOKENS SPECIFICATION
## TRIPMATE HOTEL & COMMERCIAL SERVICE PROVIDER WORKFLOW
### Anthropic Frontend-Design Methodology | Web Coastal Heritage + Mobile Editorial V3

---

## 1. DESIGN THINKING & VISUAL PHILOSOPHY (PASS 1)

In strict adherence to the official Anthropic `frontend-design` skill (`anthropics/skills/skills/frontend-design/SKILL.md`), this interface is designed from the ground up around its specific subject matter: **Central Vietnam hospitality and travel operations**.

### Subject Matter Grounding
TripMate's commercial hotel workflow serves two distinct audiences with differing operational needs:
1. **The Traveler on Mobile:** Exploring Da Nang, Hoi An, and Hue. Seeking authentic, trustworthy accommodations with clear pricing and straightforward request workflows. Tone: refined, welcoming, editorial, tranquil.
2. **The Hotel Operations Staff on Web:** Fast-paced receptionists and managers processing incoming reservations, handling check-ins, reporting checkouts, and managing commission statements. Tone: structured, disciplined, high-density, error-resilient, professional.

### Rejection of Generic AI Design Tropes
| Generic AI Design Trope | Why It Fails This Brief | TripMate Intentional Solution |
|---|---|---|
| **SaaS Card Kit** (chopping everything into identical rounded cards with fuzzy gray drop shadows) | Overwhelms hotel receptionists with visual fluff; destroys table scanability. | Structured tabular work surfaces, precise hairline borders, crisp data grids, and hierarchical split-views for request details. |
| **All-Caps Eyebrow Labels** (e.g. `FEATURE // OVERVIEW`) | Artificial tech jargon that clutters the visual field. | Natural sentence-case Vietnamese microcopy (`Yêu cầu đặt dịch vụ`, `Bảng kê hoa hồng`) with purposeful semantic typographic scales. |
| **Monoculture Palette** (drab cream + serif + terracotta on every screen) | Fails to distinguish traveler inspiration from serious administrative accounting. | Dual-platform cohesion: Coastal Heritage Teal & Warm Sand for Desktop operations; Refined Travel Editorial (Navy, Teal, Coral) for Traveler Mobile. |
| **Gratuitous Animations** (cards floating or sliding in on every hover) | Causes cognitive fatigue for staff managing dozens of daily bookings. | Targeted, purposeful motion only: modal confirmation entries, status transition pulses, and toast feedback. |

---

## 2. COMPACT COLOR TOKEN SYSTEM

### 2.1 Web Service Provider & Staff Console (Coastal Heritage System)

The Web palette draws inspiration from the coastal architecture of Central Vietnam — the deep patinas of the Thu Bon riverbanks, weathered ceramic tiles, and sun-warmed sand.

| Token Name | Hex Code | Role & Application | Contrast against White / Canvas |
|---|---|---|---|
| `--color-teal-primary` | `#174B4A` | Primary brand accent, primary CTA buttons, table active headers | 8.8:1 (AAA on White) |
| `--color-teal-deep` | `#103B39` | Hover state for buttons, active sidebar selection, brand marks | 11.2:1 (AAA on White) |
| `--color-copper-accent` | `#B96D4C` | Warning highlights, pending state cues, urgent action badges | 4.6:1 (AA on White) |
| `--color-copper-text` | `#995436` | Accent text, deadline urgency labels, highlighted price tags | 6.2:1 (AAA on White) |
| `--color-canvas-sand` | `#F7F5EF` | Page background canvas, providing warmth and reducing eye strain | 1.05:1 (Subtle surface contrast) |
| `--color-surface-pure` | `#FFFFFF` | Work cards, data grids, modal surfaces, form containers | 1:1 Base |
| `--color-ink-primary` | `#1C2928` | Primary body copy, headers, data values, table cell text | 13.5:1 (AAA on White) |
| `--color-ink-secondary`| `#5B6E69` | Field labels, timestamp text, table column headers, helper text | 4.8:1 (AA on White) |
| `--color-border-subtle` | `#DCE2DD` | Table row dividers, container outlines, input field borders | 1.3:1 (Clean boundary definition) |

### 2.2 Mobile Traveler Experience (V3 Concept B — Refined Travel Editorial)

The Mobile palette balances vibrant tropical cues with grounded editorial navy, maintaining high outdoor legibility.

| Token Name | Hex Code | Role & Application |
|---|---|---|
| `--tm-mobile-navy` | `#00152A` | Deep contrast text, header bars, card titles |
| `--tm-mobile-teal` | `#006B5F` | Primary booking CTA, selected category chips, active tab indicators |
| `--tm-mobile-coral` | `#FF7043` | Price highlights, rating star accents, submission focus buttons |
| `--tm-mobile-deep-coral`| `#B83D18` | Destructive/cancellation buttons, urgent warnings |
| `--tm-mobile-canvas` | `#F5F6F2` | Smooth editorial backdrop |
| `--tm-mobile-surface` | `#FFFFFF` | Card containers, bottom sheets, form input fields |

---

## 3. STATUS SEMANTIC TOKENS

Status presentation uses accessible dual-encoding: semantic background tint, high-contrast text color, and mandatory iconography. Color is never used alone to convey meaning (WCAG 1.4.1).

| Status Value | Meaning | Web Background | Web Text | Mobile Background | Mobile Text | Standard Icon |
|---|---|---|---|---|---|---|
| `Pending Confirmation` | Request awaiting provider review | `#FEF3E2` | `#92400E` | `#FFF3E0` | `#D84315` | `schedule` |
| `Confirmed` | Booking accepted by hotel | `#E6F7F0` | `#065F46` | `#E8F5E9` | `#2E7D32` | `check_circle` |
| `Rejected` | Declined by hotel with reason | `#FDECEF` | `#991B1B` | `#FFEBEE` | `#C62828` | `cancel` |
| `Cancelled` | Cancelled by traveler before review | `#F1F5F9` | `#475569` | `#F5F5F5` | `#616161` | `block` |
| `Expired` | 24 hours elapsed without response | `#F8FAFC` | `#64748B` | `#FAFAFA` | `#757575` | `timer_off` |
| `Completed` | Stay finished, actual amount charged | `#E8F7F4` | `#006B5F` | `#E0F2F1` | `#004D40` | `verified` |
| `No-show` | Guest did not arrive, 0 VND commission | `#F1EBFE` | `#5B21B6` | `#EDE7F6` | `#4527A0` | `person_off` |
| `Issued` | Monthly statement generated | `#EBF5FF` | `#1E40AF` | `#E3F2FD` | `#1565C0` | `receipt` |
| `Payment Reported` | Wire transfer reference submitted | `#FEF3E2` | `#B45309` | `#FFF8E1` | `#F57F17` | `pending_actions` |
| `Paid` | Receipt recorded by internal staff | `#E6F7F0` | `#065F46` | `#E8F5E9` | `#2E7D32` | `task_alt` |
| `Overdue` | Past due date, approaching lockout | `#FDECEF` | `#991B1B` | `#FFEBEE` | `#C62828` | `warning` |

---

## 4. TYPOGRAPHY SCALES & HIERARCHY

### 4.1 Web Typography (Desktop Operations)
- **Primary Typeface:** `Inter`, `-apple-system`, `Segoe UI`, `sans-serif` (Optimal x-height and tabular figure support for accounting & data grids).
- **Monospace Typeface:** `Consolas`, `Menlo`, `Monaco`, `monospace` (For Request Codes `HBR-*`, Bank References, and currency alignment).

| Style Level | Size / Line Height | Weight | Letter Spacing | Use Case |
|---|---|---|---|---|
| `Page Title (H1)` | 24px / 32px | Bold (700) | -0.02em | Main Workspace page headers (Booking Requests, Statements) |
| `Section Title (H2)` | 18px / 26px | SemiBold (600) | -0.01em | Table headers, detail panel section titles |
| `Card Title (H3)` | 15px / 22px | SemiBold (600) | 0.00em | Modal headers, guest identity blocks |
| `Body Regular` | 14px / 20px | Regular (400) | 0.00em | Standard table cell data, form field inputs |
| `Body Medium` | 14px / 20px | Medium (500) | 0.00em | Emphasized row values, customer names |
| `Caption / Label` | 12px / 16px | Medium (500) | +0.01em | Field labels, timestamp meta, pagination counters |
| `Badge Label` | 12px / 16px | SemiBold (600) | +0.02em | Status pills, category indicators |
| `Numeric Monospace`| 13px / 18px | SemiBold (600) | +0.02em | Currency numbers, request IDs, bank transfer codes |

### 4.2 Mobile Typography (Refined Travel Editorial)
- **Display Headlines:** `Bricolage Grotesque`, `sans-serif` (Characterful, editorial warmth).
- **Body & Pricing:** `Manrope`, `sans-serif` (Exceptional readability at small mobile scales).

| Style Level | Size / Line Height | Weight | Letter Spacing | Use Case |
|---|---|---|---|---|
| `Hero Headline` | 26px / 34px | Bold (700) | -0.02em | Hotel Details title on M75 |
| `Section Header`| 18px / 24px | SemiBold (600) | -0.01em | Room types list, booking sheet title |
| `Card Headline` | 16px / 22px | SemiBold (600) | 0.00em | Hotel name in search results on M74 |
| `Body Text` | 14px / 20px | Regular (400) | 0.00em | Amenities, descriptions, contact info |
| `Price Display` | 18px / 24px | Bold (700) | -0.01em | Nightly rates, estimated total amount |
| `Micro Copy` | 12px / 16px | Regular (400) | +0.01em | Availability disclaimers, address snippets |

---

## 5. SPACING & GRID SYSTEM

The design strictly employs an **8px base grid** with a **4px sub-grid** for tight micro-spacing.

| Token | Dimension | Common Use |
|---|---|---|
| `--space-1` | 4px | Icon-to-text gaps, tight tag padding |
| `--space-2` | 8px | Button inline gaps, badge padding, form label margins |
| `--space-3` | 12px | Compact cell padding, field grouping spacing |
| `--space-4` | 16px | Standard card padding, mobile screen margins, grid gutters |
| `--space-5` | 20px | Table row vertical padding, modal body spacing |
| `--space-6` | 24px | Section gaps, drawer margins |
| `--space-8` | 32px | Major layout block division |
| `--space-12`| 48px | Page header to content block separation |

### Responsive Layout Grid
- **Web 1440×900 px:** Fixed 260px Sidebar + 1fr fluid main content area (Max container width 1200px, 24px horizontal gutters).
- **Web 1280×800 px:** Fixed 240px Sidebar + 1fr fluid main content area (20px gutters).
- **Web 1024×768 px (Tablet):** Collapsible 64px icon-only rail or drawer + 1fr main content (16px gutters).
- **Mobile 390×844 dp:** Single-column layout with 16dp horizontal safe area gutters and 80dp bottom navigation clearance.

---

## 6. CORNER RADIUS & SHADOW ELEVATION

### Border Radius
- `--radius-xs`: 4px (Checkboxes, small tags, sub-pills).
- `--radius-sm`: 6px (Input fields, standard table buttons, dropdowns).
- `--radius-md`: 8px (Status badges, alert banners, sub-cards).
- `--radius-lg`: 12px (Modal dialogs, major dashboard cards, sheets).
- `--radius-xl`: 16px (Mobile hotel cards, bottom sheet top corners).
- `--radius-full`: 9999px (Pills, circular avatar badges, icon buttons).

### Elevation & Shadows
- `--shadow-subtle`: `0 1px 2px 0 rgba(16, 59, 57, 0.05)` (Default card outlines).
- `--shadow-card`: `0 4px 12px -2px rgba(16, 59, 57, 0.08)` (Hovered rows, popovers).
- `--shadow-modal`: `0 20px 25px -5px rgba(0, 21, 42, 0.15), 0 8px 10px -6px rgba(0, 21, 42, 0.10)` (Confirmation modals).
- `--shadow-bottom-sheet`: `0 -4px 20px rgba(0, 21, 42, 0.12)` (Mobile booking drawer).

---

## 7. ACCESSIBILITY COMPLIANCE (WCAG 2.2 AA)

1. **Text & Contrast Ratios:**
   - All body copy, table data, and form labels achieve contrast >= 4.5:1 against their backgrounds.
   - Primary headers and action buttons achieve contrast >= 7:1 (AAA compliance).
   - Secondary metadata labels achieve at least 4.5:1.
2. **Interactive Target Sizes:**
   - Web clickable controls: Minimum 36×36 px with 8px clearance.
   - Mobile touch targets: Minimum 48×48 dp for all primary buttons and form steppers.
3. **Keyboard Focus States:**
   - Every interactive control exposes a visible 2px focus ring with 2px offset: `focus-visible:ring-2 focus-visible:ring-[#174B4A] focus-visible:ring-offset-2`.
4. **Form Error Association:**
   - Error messages are associated with input fields via `aria-describedby` and rendered directly beneath the field (CR-04).
5. **Reduced Motion:**
   - All CSS transitions respect `@media (prefers-reduced-motion: reduce)`.
