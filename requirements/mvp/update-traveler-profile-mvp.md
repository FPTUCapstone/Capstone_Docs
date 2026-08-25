# MVP Goal

Let a Traveler keep their basic contact/identity info current without needing support
intervention, while keeping their role fixed.

# Target User

Authenticated Traveler.

# Core User Problem

Personal details (name, phone) go stale over time; the Traveler needs to self-correct them.

# Core User Journey

Open profile settings → edit a field → save → see the update reflected immediately.

# Must Have

- Display current profile values. *(FR1)*
- Edit full name and phone number. *(FR2, scoped down — see Should/Could for avatar and email)*
- Validate updated values before saving. *(FR3)*
- Role is never changed by this flow. *(BR1, FR4)*
- Confirmation on success. *(FR5)*
- Phone uniqueness check reused from registration, if phone is edited. *(BR2)*

# Should Have

- Avatar upload — real value, but adds file-handling scope; ship shortly after the core text
  fields if not in the first release.

# Could Have

- Email address change — deferred due to its re-verification implications (open question), higher
  risk to get wrong than the other fields.
- Notification when contact info changes (security-conscious nicety).

# Out of Scope

- Any "other configurable profile fields" beyond full name, avatar, phone, email — not specified,
  not invented.
- Re-verification flow for a changed email — if email change ships, it should reuse whatever
  verification mechanism email/phone changes settle on (tracked as an open question, not solved
  here).

# MVP User Journey

1. Traveler opens Profile Settings.
2. System shows current values (full name, phone; avatar/email per Should/Could Have).
3. Traveler edits one or more fields.
4. Traveler saves.
5. System validates (format, uniqueness where relevant).
6. On success: profile updated, confirmation shown.
7. On failure: field-level error, profile unchanged.

# Dependencies

- Depends on UC-04 Sign In.
- Reuses email/phone uniqueness rules from register-traveler-account.md (BR2).

# Risks

- If email change ships without deciding on re-verification, a Traveler could silently take over
  the login identifier without proving ownership of the new email — a real security gap, not just
  a UX nuance, so it's reasonable to keep out of MVP until decided.

# Open Questions

- What are "other configurable profile fields" beyond the four named ones?
- Does changing email require re-verification?
- Avatar upload constraints (file type, size, dimensions).
- Is there a notification on contact-info changes?
