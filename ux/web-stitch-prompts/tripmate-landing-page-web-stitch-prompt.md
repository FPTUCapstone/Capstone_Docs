# Product Context

TripMate is an intelligent travel platform for Central Vietnam. The public landing page introduces the product, establishes trust, and directs visitors to approved discovery and account-entry journeys without presenting mobile-only capabilities as Web actions.

# Target Platform

Responsive desktop Web application at the public root. Design for 1440 px, 1280 px, and 768 px viewport widths with no horizontal scrolling.

# Target User

- First-time and returning travelers exploring TripMate.
- Prospective tour operators looking for the partner registration entry point.
- Public visitors who need to sign in or create an account.

# Screen Objective

Communicate TripMate's value for exploring Central Vietnam and provide clear paths to sign in, register, explore points of interest, and search tours.

# Connected Screens

- Public Sign In.
- Traveler Registration.
- Tour Operator Registration.
- Points of Interest discovery.
- Tour discovery.

# Layout Structure

1. A persistent public header with the TripMate identity and approved navigation actions.
2. A spacious hero area with a concise product introduction, Central Vietnam imagery, and prioritized discovery and account actions.
3. A service-entry section with distinct Points of Interest and Tours choices.
4. Optional configurable informational content that can be omitted without weakening the main introduction or navigation.
5. A restrained platform-information section.
6. A complete public footer.

# Required Content

- TripMate product name and concise travel-platform positioning.
- Central Vietnam context.
- Approved actions: Sign In, Register Traveler, Register Tour Operator, Explore Points of Interest, and Search Tours.
- Brief explanatory copy for Points of Interest and Tours.
- Trustworthy platform information and footer content.

# Required Components

- TripMate brand mark or wordmark treatment.
- Public navigation bar.
- Primary and secondary call-to-action buttons.
- Hero image or composed travel visual.
- Two service-entry cards or panels: Points of Interest and Tours.
- Configurable informational content region.
- Footer with grouped informational links.

# Required States

- **Initial:** Public shell and navigation are visible while content begins loading.
- **Loading:** Preserve the page structure and navigation while configurable content loads.
- **Loaded:** Default populated landing page with published content and all approved public handoffs.
- **Empty:** Optional-content-empty variant where the configurable content region is absent while the hero, navigation, service entries, platform information, and footer remain coherent.
- **Validation Error:** Not applicable; this page contains no input form.
- **Business Error:** Not applicable; no landing-page business transaction is performed.
- **System Error:** Retain the public shell and authentication and registration links where possible; do not show technical details or invent content.
- **Success:** Not applicable as a standalone state; approved actions navigate to connected screens.
- **Disabled Action:** No persistent disabled call to action is required. Use visible keyboard focus and hover states for every interactive control.
- **Responsive:** Navigation and content reflow as a Web page without becoming a mobile-app tab shell.

# Navigation Context

- Sign In leads to the proposed `/sign-in` destination.
- Register Traveler leads to the proposed `/register` destination.
- Register Tour Operator leads to the proposed `/partner/register` destination.
- Explore Points of Interest leads to the proposed `/pois` destination.
- Search Tours leads to the proposed `/tours` destination.
- Routes are navigation context for the design, not implementation requirements.

# Responsive Behavior

- At 1440 px, use a broad centered content frame, generous whitespace, a balanced hero composition, and side-by-side service entries.
- At 1280 px, preserve the same information hierarchy with moderately reduced spacing and image width.
- At 768 px, collapse the header into a compact accessible navigation treatment, stack hero content and service entries, keep all calls to action visible, and avoid clipped content or horizontal scrolling.
- Images scale and crop intentionally; text and controls never overlay important image subjects.

# Visual Direction

- Deep navy as the primary structural color, teal for positive or exploratory accents, coral for selective high-priority emphasis, and light neutral surfaces.
- Professional tourism photography rooted in Central Vietnam rather than generic resort imagery.
- Restrained map or geographic motifs may appear as subtle background detail.
- Modern, credible, calm, and spacious; avoid a playful travel-app aesthetic.
- Maintain a single coherent visual language with the public authentication screens.

# Accessibility

- Meet accessible color contrast for text, buttons, links, and focus indicators.
- Use semantic heading hierarchy and readable line lengths.
- Provide text labels for navigation and actions rather than relying on icons alone.
- Ensure all controls have visible keyboard focus states and comfortable target sizes.
- Do not place essential text inside photography.

# UX Constraints

- Do not add a search input merely because the page includes a Search Tours action.
- Keep the approved calls to action distinct and easy to scan.
- The page must still work when optional configurable content is unavailable.
- Mobile-only capabilities may be mentioned only as non-interactive product information when necessary; they must not become Web actions.
- Do not imply personalized recommendations, rankings, or featured inventory without approved data.

# Things NOT to Design

- No mobile application screen.
- No live navigation or rerouting interface.
- No offline-map interaction.
- No group location-sharing interface.
- No QR-scanning interface.
- No commercial-service booking flow.
- No unapproved featured-destination carousel, ranking, personalization, or search form.
- No signed-in traveler, operator, or administrator dashboard.

# Open Questions

- Final production copy, imagery, logo assets, footer links, and configurable informational content remain content decisions.
- Whether featured tours or Points of Interest are approved landing-page content types, and how they would be selected, remains unresolved; they are omitted.
- Whether a dedicated public itinerary-planning call to action is approved before authentication remains unresolved; it is omitted.
- Exact production routes and behavior remain implementation decisions; the paths above are proposed navigation references.

# Stitch Prompt

Design a responsive desktop Web application, not a mobile app.

Create the public TripMate landing page for an intelligent travel platform focused on Central Vietnam. Produce responsive Web layouts at 1440 px, 1280 px, and 768 px with no horizontal scrolling. The page must feel modern, trustworthy, calm, and spacious rather than playful.

Use a shared visual system with deep navy structural areas, teal exploratory accents, limited coral emphasis, light-neutral backgrounds, strong accessible contrast, generous whitespace, and professional Central Vietnam tourism photography. A restrained map or geographic motif may appear as subtle background texture, but it must not compete with content.

Build the page in this order:

1. A persistent public header with the TripMate identity and clearly labeled actions for Sign In, Register Traveler, and Register Tour Operator.
2. A spacious hero section with concise TripMate positioning, Central Vietnam imagery, and prioritized actions for Explore Points of Interest and Search Tours. These are action buttons, not a search form.
3. Two clear service-entry cards or panels for Points of Interest and Tours, each with short explanatory copy and one relevant action.
4. An optional configurable informational region that can disappear cleanly without leaving a broken layout.
5. A restrained platform-information section that supports trust without inventing statistics, rankings, endorsements, or personalized content.
6. A structured public footer with placeholder information groups.

Show the proposed navigation relationships: Sign In to `/sign-in`, Register Traveler to `/register`, Register Tour Operator to `/partner/register`, Explore Points of Interest to `/pois`, and Search Tours to `/tours`. Treat those paths as design annotations, not visible technical copy.

Include an initial shell, a structure-preserving loading treatment for configurable content, a default populated layout, an optional-content-empty layout, and a safe system-error treatment that keeps authentication and registration navigation usable without technical details. No validation, transaction-success, or persistent disabled-action state is needed because the page has no form. Provide clear hover and keyboard-focus states for links and buttons. At 1440 px use a wide centered frame and balanced text-image hero. At 1280 px tighten spacing while preserving hierarchy. At 768 px use a compact accessible navigation treatment and stack the hero and service entries vertically. Keep typography readable, controls comfortably sized, images intentionally cropped, and all content free from clipping.

Do not create a mobile app screen, live navigation, rerouting, offline maps, group location sharing, QR scanning, a commercial-service booking flow, a search input, featured rankings, personalization, or signed-in dashboards. Mobile-only capabilities must not appear as actionable Web features.
