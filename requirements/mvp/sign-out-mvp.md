# MVP Goal

Let any authenticated user end their session with one action and be safely locked out of
protected functions until they sign in again.

# Target User

Any authenticated user — Traveler, Tour Operator, or Administrator.

# Core User Problem

A user needs a reliable, immediate way to end their session, especially on a shared or
untrusted device.

# Core User Journey

Tap Sign Out → client authentication information is cleared → user is redirected to Sign In.

# Must Have

- Sign Out action available to any authenticated user, regardless of role. *(BR1)*
- Removal/invalidation of client-held authentication information on sign-out. *(FR1)*
- Protected functions become inaccessible immediately after sign-out. *(FR2)*
- Re-authentication required to regain access. *(FR3)*

# Should Have

- Redirect to the Sign In screen immediately after sign-out, so the user isn't left on a now-stale
  protected screen.

# Could Have

- A confirmation step before signing out ("Are you sure?").
- Multi-device/session visibility or management (e.g., "sign out of all devices").

# Out of Scope

- Server-side invalidation semantics beyond what's needed for this client to lose access (the
  exact mechanism — stateless JWT expiry vs. a server-side revocation list — is an
  implementation detail, not a UX requirement).

# MVP User Journey

1. User taps Sign Out from wherever the action is available.
2. Client clears local authentication information.
3. User is redirected to Sign In.
4. Any attempt to access a protected function without signing in again is blocked.

# Dependencies

- Depends on UC-04 Sign In existing (nothing to sign out of otherwise).

# Risks

- If server-side session/token invalidation isn't actually implemented (Missing Information),
  "sign out" may only be cosmetic on the client while the token remains technically valid until
  natural expiry — a security-relevant gap worth resolving before build, not just a UX nuance.

# Open Questions

- Does Sign Out invalidate the session/token server-side, or only clear it client-side?
- Does Sign Out on one device affect other active sessions?
- Is a confirmation step required, or is it immediate?
- Exact redirect destination after sign-out.
