# MVP Goal

Let any authenticated user replace their password from account settings, using their current
password as proof of identity.

# Target User

Any authenticated user — Traveler, Tour Operator, or Administrator.

# Core User Problem

A user who wants to update their password proactively (not because it's forgotten) needs a
straightforward, secure, in-app way to do it.

# Core User Journey

Open account settings → enter current password + new password → system verifies and updates →
confirmation.

# Must Have

- Current password required before any change. *(BR1, FR1)*
- Current password verification. *(FR3)*
- New password validated against the standard password policy. *(BR2)*
- Confirmation on success. *(FR4)*
- Available identically to all three roles. *(BR3, FR5)*

# Should Have

- Reject a new password identical to the current one (EF3) — reasonable default in the absence of
  a stated rule, low implementation cost.

# Could Have

- Invalidate other active sessions/devices on password change.
- Security notification (e.g., email) when the password changes.
- Require re-authentication immediately after changing the password.

# Out of Scope

- Any session-management UI (viewing/revoking other active sessions) — bigger than what this use
  case describes.

# MVP User Journey

1. User opens account settings → Change Password.
2. User enters current password and new password.
3. System verifies the current password.
4. System validates the new password against policy.
5. On success: credential updated, confirmation shown.
6. On failure: error shown (wrong current password, or new password fails policy), credential
   unchanged.

# Dependencies

- Depends on UC-04 Sign In (must be authenticated to reach this).
- Reuses the password policy defined in register-traveler-account.md (BR4).

# Risks

- If "same as current password" isn't explicitly blocked (Should Have, not Must Have), a user
  could "change" their password to the same value with no real security benefit — low severity,
  acceptable to defer.
- Not invalidating other sessions on change (left as Could Have) is a minor security gap if a
  user is changing their password specifically because a session was compromised elsewhere.

# Open Questions

- Is re-authentication required after a password change, or does the current session remain
  valid?
- Are other active sessions/devices signed out on change?
- Is a same-as-current-password submission blocked or accepted?
- Is a security notification email sent on change?
