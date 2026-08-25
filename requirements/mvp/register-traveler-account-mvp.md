# MVP Planning: Register Traveler Account

> Based on: `requirements/register-traveler-account.md` (Use Case Analysis — Register Traveler Account)
> Scope note: Only one use case has been analyzed to date. This MVP plan is scoped to that use case (account registration / onboarding entry point), not the full TripMate product. *(Assumption — no broader product requirement set exists yet to plan against)*

# MVP Goal

Let a Guest create a TripMate account with the minimum friction and fields necessary to become an authenticated Traveler, so the core "onboarding" journey can be validated end-to-end. *(Assumption — inferred from the use case; no explicit product goal statement exists in source)*

# Target User

Guest (unauthenticated visitor) who wants to start using TripMate as a Traveler. *(Explicit — actor from source use case)*

# Core User Problem

A new visitor has no way to access TripMate's Traveler features without an account, and currently cannot self-serve one. *(Assumption — reasonable restatement of the use case's purpose)*

# Core User Journey

1. Guest opens the registration screen.
2. Guest submits full name, email, phone number, and a password (all required). *(Decided — stakeholder confirmed both email and phone are required, 2026-08-26, see BR2/BR3)*
3. System validates input and checks uniqueness of both email and phone.
4. System creates the account, assigns role = Traveler.
5. Guest is redirected to the Login screen and logs in manually. *(Decided — stakeholder confirmed no auto-login, 2026-08-26)*

*(Steps 1–4 map to Main Flow in the use case; step 5 confirmed via stakeholder decision — see BR5 in source doc)*

# Must Have

- Registration form: full name, email, phone number, password (all four required). *(Decided — FR1, BR3; stakeholder confirmed 2026-08-26 both email and phone are mandatory, not either/or)*
- Input validation before account creation, including password policy: min 8 characters, at least 1 uppercase, 1 lowercase, 1 number, 1 special character. *(Explicit — FR2; policy decided by stakeholder 2026-08-26, see BR4)*
- Uniqueness check on both email and phone number, with rejection on duplicate of either. *(Decided — FR3, BR2; stakeholder confirmed 2026-08-26)*
- Email set as the login identifier; phone stored for contact/verification only, not usable for login. *(Decided — stakeholder confirmed 2026-08-26, see BR6)*
- Automatic assignment of Traveler role on success. *(Explicit — FR4, BR1)*
- Inline error messages on validation failure and duplicate-account rejection (e.g., "Password is not 8 characters long", "Email already exists"). *(Decided — stakeholder confirmed 2026-08-26, see E1/E2)*
- Redirect to Login screen after successful registration (no auto-login). *(Decided — stakeholder confirmed 2026-08-26, see BR5)*
- Terms of Service / Privacy Policy shown as a text annotation with a link below the Register button (e.g., "By registering, you agree to the Terms & Privacy Policy"); no separate checkbox. *(Decided — stakeholder confirmed 2026-08-26, see BR7)*

# Should Have

*(none currently — phone number moved to Must Have per stakeholder decision 2026-08-26)*

# Could Have

- Google Social Login (Alternative Flow A1). *(Explicit in source but conditional — "If Google Social Login is supported"; source's own MVP Scope suggestion defers this)*
- Password strength/complexity feedback beyond minimum validation. *(Source flags this as unspecified — Missing Information)*

# Out of Scope

- Email and phone (OTP) verification workflows. *(Decided — stakeholder confirmed Post-MVP, 2026-08-26)*
- Anti-abuse measures (rate limiting, CAPTCHA). *(Decided — stakeholder confirmed Post-MVP, 2026-08-26)*
- Conflict resolution between Google-authenticated and manually-registered identities (merge/block/link). *(Decided — stakeholder confirmed Post-MVP, 2026-08-26; also depends on Google Login, itself Could Have / deferred)*
- Concurrent-registration race-condition handling beyond a basic DB-level unique constraint. *(Edge case noted in source; acceptable risk for MVP validation)*
- Phone number format/country-code normalization beyond basic input validation. *(Still open — see Missing Information in source doc; acceptable risk for MVP)*

# MVP User Journey

Guest → Registration form (full name, email, phone number, password) → Client/server validation (incl. password policy) → Uniqueness check on email and phone → Account created with role = Traveler → Guest redirected to Login screen → Guest logs in manually.

On failure at validation or uniqueness check: Guest sees an inline error message (e.g., "Password is not 8 characters long", "Email already exists") and remains on the registration form to correct input.

# Dependencies

- Password storage/hashing and auth foundation must exist before this journey can be implemented (not covered by the source use case — implementation concern, noted here only as a planning dependency). *(Assumption)*
- A minimal account/user data model with a `role` field (defaulting to Traveler) must exist. *(Explicit — FR4, BR1)*
- A Login screen/flow must exist (or ship alongside Registration) since Registration now explicitly redirects there on success. *(Decided — BR5)*

# Risks

- Requiring both email and phone (rather than either/or) adds a field and a second uniqueness check to the registration form, slightly increasing signup friction — acceptable per stakeholder decision, but worth watching if drop-off is measured post-launch.
- Deferring email/phone verification means unverified contact info can be used to register accounts, which could affect data quality or enable abuse — acceptable for MVP but should be revisited before wider launch. *(Risk, following from Out of Scope decision)*
- No stated phone normalization rule means the same real-world number in different formats (e.g., with/without country code) could bypass the uniqueness check — acceptable risk for MVP, flagged as Out of Scope.
- Redirect-to-Login-on-success makes Login a hard dependency for the journey to be end-to-end usable; if Login isn't ready, Registration alone can't be demoed/validated.

# Open Questions

None outstanding for MVP. All prior open items are resolved via stakeholder decisions on 2026-08-26 (see source doc BR2–BR7 and Missing Information) — including explicit confirmation that email/phone verification, Google-identity conflict resolution, and anti-abuse measures stay Out of Scope / Post-MVP.
