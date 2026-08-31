# Product Context

TripMate lets Travelers create itinerary-linked private travel groups. UC-17 assigns the creator as the exclusive Group Host.

# Screen Objective

Design the Flutter Mobile **Create Travel Group** form with one required Group Name and a read-only selected-itinerary association.

# Target User

Authenticated Traveler with an eligible itinerary.

# Screen Content

Itinerary preview, Group Name input, Host-assignment note, Create action, exact validation/success/failure messaging.

# Required Components

Material 3 top app bar, itinerary card, outlined text field, single full-width CTA, inline error, toast/alert, disabled/loading button.

# Required States

Initial, input, inline validation error, loading/disabled, system failure, and success/navigation.

# Navigation Context

Eligible Itinerary → Create Travel Group → Travel Group Details & Members.

# UX Constraints

Single-column Flutter mobile layout, English UI, no public discovery, no alternate Host picker, no extra group fields.

# Open Questions

None.

# Stitch Prompt

Design a Material Design 3 Flutter mobile screen named **“Create Travel Group”** for TripMate. Actor: authenticated Traveler. Purpose: create a private travel group linked to the selected eligible itinerary; the creator automatically becomes Group Host.

Use a 390×844 mobile viewport. Top to bottom:

1. Top app bar with Back and title “Create Travel Group”.
2. Read-only itinerary card for **Hoi An Weekend — 26/08/2026**, showing Da Nang/Hoi An metadata and a small destination image.
3. Required outlined text field labeled “Group name”, sample value **“Hoi An Weekend Crew”**.
4. Compact read-only line/status chip stating the current Traveler will be the Group Host; do not add Host selection.
5. Full-width primary button “Create Travel Group”.
6. Message region/toast.

States:

- Initial: itinerary loaded, Group Name empty.
- Validation: exact inline text under Group Name, **“This field is required.”**
- Loading: button progress indicator and disabled repeated submission.
- Failure: exact alert **“TripMate is temporarily unable to process your request. Please check your connection and try again.”**
- Success: exact toast **“Travel group created! You are the Group Host. Share the invite code to add members.”**, then navigate to Travel Group Details & Members.

Keep the form minimal and travel-focused. Do not add group description, privacy marketplace, public discovery, member picker, or Host selector.

