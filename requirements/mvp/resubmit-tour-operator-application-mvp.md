# MVP Goal

Let a Tour Operator whose application was rejected fix the flagged problems and get back into
the review queue, without ever needing a second account.

# Target User

Tour Operator — an authenticated account holder whose most recent application is Rejected.

# Core User Problem

A rejected application is a dead end unless the operator has a clear, low-friction way to see why
it was rejected, fix it, and resubmit on the same identity.

# Core User Journey

Sign in → see the rejection reason → edit the flagged business/legal info and/or re-upload the
license → resubmit → application returns to Pending Approval.

# Must Have

- Sign-in gate before accessing the resubmission flow. *(depends on UC-04)*
- Display of the Administrator's rejection reason.
- Ability to edit previously submitted business/legal fields.
- Ability to re-upload the Travel Business License file.
- Reuse of the original registration validation rules on resubmission. *(EF2)*
- Setting application status back to Pending Approval on success. *(FR6)*
- Resubmission updates the same application record — never a new account/application. *(BR2)*

# Should Have

- Enforcing that resubmission is only reachable when the application is actually Rejected (BR1)
  — important for correctness, but a simple "no rejected application found" fallback screen can
  stand in if this isn't fully gated at MVP.

# Could Have

- Highlighting which specific fields were flagged in the rejection reason (vs. just showing the
  reason as text).
- Viewing prior rejection history (not just the latest reason).

# Out of Scope

- A cap/limit on the number of resubmission cycles.
- Diff/change-highlighting for the Administrator between rejected and resubmitted versions (that
  belongs to the downstream Admin Approval use case, not this one).
- Partial-correction rules (must-fix-all vs. fix-some) — ship assuming any successful validation
  pass is enough to resubmit, since the requirement analysis leaves this open.

# MVP User Journey

1. Tour Operator signs in.
2. System detects the account has a Rejected application and shows the rejection reason.
3. Tour Operator edits the flagged fields and/or re-uploads the license file.
4. Tour Operator resubmits.
5. System validates (same rules as initial registration).
6. On success: same application record updated, status set to Pending Approval, confirmation
   shown.
7. On failure: field-level errors shown, application stays Rejected.

# Dependencies

- UC-04 Sign In must exist and work for a Tour Operator account before this flow is reachable.
- The original registration's validation rules and file-upload mechanism (from
  register-tour-operator-account) are reused here rather than redefined.
- Depends on the downstream Admin Approval use case actually producing a rejection reason to
  display.

# Risks

- If Sign In (UC-04) isn't ready, this entire use case is unreachable — sequencing risk.
- Reusing validation rules "as-is" assumes register-tour-operator-account's still-open questions
  (Tax Code/License format, file size/format limits) get resolved before either flow is built,
  since both depend on the same rules.
- No defined behavior for a Tour Operator with no Rejected application reaching this flow risks
  an inconsistent/undefined UI state if not handled.

# Open Questions

- What happens if a Tour Operator without a Rejected application accesses this flow?
- Must all flagged issues be corrected before resubmission, or is partial correction accepted?
- Is there a limit on resubmission cycles?
- Is full rejection history visible, or only the latest reason?
