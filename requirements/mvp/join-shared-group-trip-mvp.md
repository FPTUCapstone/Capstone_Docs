# MVP Goal

Allow an authenticated Traveler to join one available travel group using a valid invitation code or QR invitation.

# Target User

Authenticated Traveler holding a group invitation.

# Core User Problem

The Traveler needs a reliable way to redeem an invitation and gain permitted shared-trip access without duplicate membership.

# Core User Journey

Open Join Group → enter code or scan QR → validate invitation/group/membership → create active membership → synchronize permitted itinerary/group data → show success and group.

# Must Have

- Flutter Mobile only.
- Support invitation-code input and QR invitation scanning.
- Validate invitation existence, usability, expiry where applicable, and group availability.
- Prevent duplicate active membership.
- Create one active membership and record Joined Timestamp.
- Make associated itinerary and permitted group information accessible.
- Use MSG56, MSG57, MSG58, and MSG127 exactly.
- Prevent double submission.

# Should Have

None defined.

# Could Have

None defined.

# Out of Scope

- Invitation usage quotas.
- Public group discovery.
- Generating invitations.
- Location-sharing opt-in.

# MVP User Journey

Traveler submits code/QR, receives exact validation outcome, and opens the group after successful membership creation.

# Dependencies

- Existing usable invitation and available group.
- Report 3 BR-46 to BR-48, CR-03, CR-04, CR-06, CR-10 to CR-13, CR-15, MSG01, MSG56 to MSG58, MSG125 to MSG127.

# Risks

- Race conditions could create duplicate active membership.
- A stale invitation may reference an unavailable group.

# Open Questions

No invitation usage quota is defined and none is assumed.

