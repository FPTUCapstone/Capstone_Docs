# Web Screen Inventory

| # | Screen | Related UC | Actor | Route Suggestion |
|---|---|---|---|---|
| 1 | TripMate Landing Page | Public SRS screen; content maintained by UC-66 | Guest | `/` |

# Screen: TripMate Landing Page

## Related UC

- No standalone UC. The screen is canonical in Report 3 section 3.1.2.1.
- UC-66 maintains supported landing-page content; UC-66 administration UI is outside Batch 1.
- Public handoffs: UC-01 Register Traveler Account, UC-02 Register Tour Operator Account, UC-04 Sign In, UC-12 Explore Points of Interest, UC-24 Search Tours, and UC-26 View Tour Details.

## Actor

Guest.

## Platform

Responsive Next.js Web.

## Purpose

Introduce TripMate as a Central Vietnam travel-planning and travel-services platform and provide clear entry points to the approved public functions. The page must not imply that mobile-only live navigation, rerouting, offline access, group location sharing, QR scanning, or commercial-service booking is available on Web.

## Entry Conditions

- No authentication is required.
- Published landing-page content is available from the content managed through UC-66.

## Entry Points

- Direct navigation to the public Web root.
- Public links, search results, or campaign links controlled outside this specification.

## Exit / Navigation

- Sign In → `/sign-in`.
- Register as Traveler → `/register`.
- Register as Tour Operator → `/partner/register`.
- Explore POIs → `/pois` (route proposal; Batch 2 specification required).
- Search Tours → `/tours` (route proposal; Batch 2 specification required).
- Select a tour → `/tours/[id]` (route proposal; Batch 2 specification required).

These destinations describe the approved future Web information architecture. Their presence in this specification does not claim that the current frontend implementation already provides them.

## Suggested Route

`/`

Route suggestions are UX/architecture proposals, not business requirements.

## Main Layout Regions

1. Public top navigation with TripMate identity and authentication/registration entry points.
2. Primary introduction area explaining the supported TripMate value proposition.
3. Public service-entry area for Points of Interest and Tours.
4. Configurable published-content regions supplied by the landing-page content model.
5. Platform guidance that distinguishes Web planning/discovery from Mobile active-trip capabilities where platform messaging is shown.
6. Footer containing required informational and policy navigation when those destinations are approved.

The exact number, order, and editorial format of configurable content blocks is not specified by Report 3 and must not be treated as a locked requirement.

## Required Data

- Published TripMate landing-page content supported by UC-66.
- Public navigation destinations for Sign In, Traveler Registration, Tour Operator Registration, POI exploration, and Tour search.
- Public tour/POI content only if the approved landing-page configuration selects those supported content types.

No ranking rule, featured-item rule, campaign model, or personalization is established for this page.

## Components

### Inputs

None required by the canonical landing-page description.

### Read-only Information

- TripMate product identity.
- Published introduction and supported-platform information.
- Approved, currently published landing-page content.
- Clear labels for public service entry points.

### Primary Actions

- Explore Points of Interest.
- Search Tours.
- Sign In.

The relative visual priority among these actions is a UX proposal, not a business rule.

### Secondary Actions

- Register as Traveler.
- Register as Tour Operator.
- Open approved informational/policy destinations when available.

## Table / List Columns

Not applicable. If published POIs or tours are shown, use responsive content cards rather than an operational data table.

## Filters / Search

No search input is required on the landing page. Search actions may navigate to UC-12 or UC-24, where the approved search/filter behavior will be specified in Batch 2.

## Modal / Dialog Behavior

None required.

## Validation

Not applicable; there is no required data-entry form on this screen.

## Business Rules

- The public page must expose only public/Guest-accessible functions.
- Administrator functions must not appear in public navigation.
- Mobile-only functions may be explained as Mobile capabilities but must not be presented as executable Web actions.
- Public CTA destinations may be specified for the approved Web design even when implementation is pending; documentation must not label an unimplemented destination as currently available.
- UC-66 governs which supported landing content is published; this screen does not define the Admin editing workflow.

## Application Messages

No locked application message is assigned to the public landing page in Report 3.

## States

### Initial

Public shell is visible while content begins loading.

### Loading

Preserve the page structure and navigation while configurable content loads. A loading treatment is a UX requirement for continuity, not a new business function.

### Loaded

Published content and all authorized public handoffs are available.

### Empty

If no optional configurable content is published, retain the product introduction and public navigation. Do not show invented featured tours or POIs.

### System Error

Keep the public shell and authentication/registration links usable where possible. Do not expose technical error details.

### Responsive

Navigation and content regions reflow without converting the page into a Mobile-app tab shell.

## Navigation Map

```text
TripMate Landing Page
├── Sign In → Public Sign In
├── Register as Traveler → Traveler Registration
├── Register as Tour Operator → Tour Operator Registration
├── Explore POIs → POI Explore/List (Batch 2)
└── Search Tours → Tours Search/List (Batch 2)
    └── Select Tour → Tour Details (Batch 2)
```

## Responsive Behavior

### 1440px

- Full public top navigation with visible labels and separated authentication/registration actions.
- Use a wide, travel-oriented composition with clear geographic imagery or map-aware visual language where published content supports it.
- Optional content cards may use multiple columns without increasing the approved content scope.

### 1280px

- Preserve the same hierarchy with narrower gutters and fewer columns where needed.
- Keep the main public actions visible without horizontal scrolling.

### 768px

- Collapse public navigation into an accessible compact menu.
- Stack major regions and content cards vertically or in a two-column grid where readable.
- Keep Sign In and registration choices easy to distinguish; do not adopt Mobile-only bottom navigation.

## Open Questions

- Which landing-page content types are actually configurable under UC-66?
- Are featured tours or featured POIs approved content types, and what selects them?
- What exact policy/informational pages exist for footer links?
- Is a dedicated public CTA for itinerary planning approved before authentication, or should planning begin only after Sign In?

## UX Suggestions

- Use a coherent public TripMate shell with deep navy, teal, coral accent, light neutral surfaces, and restrained geographic/map motifs.
- Favor authentic Central Vietnam travel imagery or map context only when approved assets/content exist.
- Give POI exploration and tour discovery distinct entry cards so Guests understand the difference between self-planning and packaged tours.
- Explain Mobile active-trip capabilities as platform information, never as active Web controls.
