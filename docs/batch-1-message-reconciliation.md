# Batch 1 Application Message Reconciliation

## Purpose

This audit reconciles the `MSG` references in the detailed Report 3 UC sections with the locked Application Messages List in Report 3 section 5.3. The locked message text is not edited. A `Correct Match` is recorded only when an existing locked message unambiguously matches the required context.

## Status Definitions

- `VALID`: referenced code and locked content match the UI context.
- `WRONG_REFERENCE`: the detailed UC points to the wrong code and an existing locked message is a clear semantic match.
- `AMBIGUOUS`: an existing message may be reusable, but its wording or context does not establish an exact match.
- `MISSING_MESSAGE`: no locked message correctly represents the required user-facing situation.

## Reconciliation Matrix

| UC | Context | Referenced MSG | Locked Message Meaning | Correct Match | Status |
|---|---|---|---|---|---|
| UC-01 | Required registration field is empty | MSG01 | Required field is empty | MSG01 | VALID |
| UC-01 | Email format is invalid | MSG02 | Invalid email format | MSG02 | VALID |
| UC-01 | Password does not meet policy | MSG03 | Email already registered | MSG05 | WRONG_REFERENCE |
| UC-01 | Confirm Password mismatch | MSG04 | Invalid phone number | MSG06 | WRONG_REFERENCE |
| UC-01 | Email already registered | MSG05 | Password policy failure | MSG03 | WRONG_REFERENCE |
| UC-01 | Terms/Privacy not accepted | MSG06 | Password mismatch | None | MISSING_MESSAGE |
| UC-01 | Verification code incorrect or expired | MSG07 | Traveler registration success; asks user to verify | MSG14 | WRONG_REFERENCE |
| UC-01 | Google email already registered | MSG05 | Password policy failure | MSG03 | WRONG_REFERENCE |
| UC-01 | Standard registration succeeds | MSG08 | Tour Operator application submission success | MSG07 | WRONG_REFERENCE |
| UC-01 | Google registration succeeds and user is signed in | MSG08 | Tour Operator application submission success | MSG12 may cover the sign-in result, but not account creation | AMBIGUOUS |
| UC-01 | Verification code is resent | Not specified | Not applicable | MSG15 mentions a new six-digit code sent to email/phone; UC-01 does not define code length | AMBIGUOUS |
| UC-01 | Account/profile persistence or Google authorization fails | MSG127 | Generic system/network failure | MSG127 | VALID |
| UC-02 | Required account/company field is empty | MSG01 | Required field is empty | MSG01 | VALID |
| UC-02 | Email format is invalid | MSG02 | Invalid email format | MSG02 | VALID |
| UC-02 | Password does not meet policy | MSG03 | Email already registered | MSG05 | WRONG_REFERENCE |
| UC-02 | Confirm Password mismatch | MSG04 | Invalid phone number | MSG06 | WRONG_REFERENCE |
| UC-02 | Email already registered | MSG05 | Password policy failure | MSG03 | WRONG_REFERENCE |
| UC-02 | Mandatory business document is missing | MSG22 | Required travel-preference categories are missing | None | MISSING_MESSAGE |
| UC-02 | Uploaded document type/size is invalid | MSG19 | Traveler profile update success | None | MISSING_MESSAGE |
| UC-02 | Business Licence Number or Tax Code already registered | MSG26 | POI update success | None | MISSING_MESSAGE |
| UC-02 | Application already Pending Review for submitted identity | MSG23 | Unsaved travel-preference changes confirmation | None | MISSING_MESSAGE |
| UC-02 | Initial operator application submission succeeds | MSG21 | Travel preferences saved successfully | MSG08 | WRONG_REFERENCE |
| UC-02 | Storage/persistence failure | MSG127 | Generic system/network failure | MSG127 | VALID |
| UC-03 | No application exists or latest application is not Rejected | MSG25 | POI creation success | None | MISSING_MESSAGE |
| UC-03 | Required corrected company field is empty | MSG01 | Required field is empty | MSG01 | VALID |
| UC-03 | Mandatory business document is missing | MSG22 | Required travel-preference categories are missing | None | MISSING_MESSAGE |
| UC-03 | Replacement/additional document type/size is invalid | MSG19 | Traveler profile update success | None | MISSING_MESSAGE |
| UC-03 | Corrected Business Licence Number or Tax Code belongs to another account | MSG26 | POI update success | None | MISSING_MESSAGE |
| UC-03 | Resubmission succeeds | MSG24 | No POIs match search/filter | None | MISSING_MESSAGE |
| UC-03 | Application update fails | MSG127 | Generic system/network failure | MSG127 | VALID |
| UC-04 | Required sign-in field is empty | MSG01 | Required field is empty | MSG01 | VALID |
| UC-04 | Email format is invalid | MSG02 | Invalid email format | MSG02 | VALID |
| UC-04 | Email is unknown or password is incorrect | MSG09 | Incorrect email or password | MSG09 | VALID |
| UC-04 | Account is temporarily locked after five failed attempts | MSG10 | Tour Operator account pending verification/approval | None | MISSING_MESSAGE |
| UC-04 | Account is locked by Administrator | MSG10 | Tour Operator account pending verification/approval | MSG11 | WRONG_REFERENCE |
| UC-04 | Account is Pending Verification | MSG11 | Account locked for policy violations | None | MISSING_MESSAGE |
| UC-04 | Google identity is not linked | MSG09 | Incorrect email or password | None; generic non-disclosure may be intended but wording names email/password | AMBIGUOUS |
| UC-04 | Sign-in succeeds | MSG12 | Sign-in success | MSG12 | VALID |
| UC-04 | Pending Approval Tour Operator is authenticated but workspace remains locked | Not specified in detailed success flow | Not applicable | MSG10 | VALID |
| UC-04 | Token/session establishment fails | MSG127 | Generic system/network failure | MSG127 | VALID |
| UC-06 | Required reset field is empty | MSG01 | Required field is empty | MSG01 | VALID |
| UC-06 | Email format is invalid | MSG02 | Invalid email format | MSG02 | VALID |
| UC-06 | Reset request acknowledgement without account disclosure | MSG14 | Invalid/expired verification code | None | MISSING_MESSAGE |
| UC-06 | Reset code is incorrect, expired, or consumed | MSG15 | Verification code resent | MSG14 | WRONG_REFERENCE |
| UC-06 | Reset code resend succeeds | Not specified in detailed flow | Not applicable | MSG15 | VALID |
| UC-06 | New password does not meet policy | MSG03 | Email already registered | MSG05 | WRONG_REFERENCE |
| UC-06 | Confirm New Password mismatch | MSG04 | Invalid phone number | MSG06 | WRONG_REFERENCE |
| UC-06 | Password reset succeeds | MSG16 | Password reset success and sign-in instruction | MSG16 | VALID |
| UC-06 | Password persistence/system failure | MSG127 | Generic system/network failure | MSG127 | VALID |
| UC-07 | Required Change Password field is empty | MSG01 | Required field is empty | MSG01 | VALID |
| UC-07 | Current Password is incorrect | MSG17 | Current Password mismatch | MSG17 | VALID |
| UC-07 | New Password does not meet policy | MSG03 | Email already registered | MSG05 | WRONG_REFERENCE |
| UC-07 | Confirm New Password mismatch | MSG04 | Invalid phone number | MSG06 | WRONG_REFERENCE |
| UC-07 | New Password is identical to Current Password | MSG03 | Email already registered | None | MISSING_MESSAGE |
| UC-07 | Password change succeeds | MSG18 | Password change success | MSG18 | VALID |
| UC-07 | Password update fails | MSG127 | Generic system/network failure | MSG127 | VALID |

## Locked Messages Used by Final Batch 1 Specs

The final Web specifications may require only these locked messages as authoritative visible copy:

| Code | Locked content |
|---|---|
| MSG01 | `This field is required.` |
| MSG02 | `Invalid email format. Please enter a valid email address (e.g., user@example.com).` |
| MSG03 | `An account with this email already exists. Please sign in or use another email.` |
| MSG04 | `Invalid phone number. Phone number must be 10 digits starting with 0.` |
| MSG05 | `Password must be at least 8 characters, containing uppercase, lowercase, number, and special character.` |
| MSG06 | `Passwords do not match. Please re-enter.` |
| MSG07 | `Account registered successfully! Please verify your email/OTP to activate your account.` |
| MSG08 | `Your business profile has been submitted for verification. Admin review takes 1-2 business days.` |
| MSG09 | `Incorrect email or password. Please try again.` |
| MSG10 | `Your Tour Operator account is pending verification. You will be notified once approved.` |
| MSG11 | `Your account has been locked due to policy violations. Please contact support@tripmate.com.` |
| MSG12 | `Welcome back to TripMate! Signed in successfully.` |
| MSG14 | `Invalid or expired verification code. Please request a new OTP.` |
| MSG15 | `A new 6-digit verification code has been sent to your email/phone.` |
| MSG16 | `Your password has been reset successfully. Please sign in with your new password.` |
| MSG17 | `Current password does not match our records.` |
| MSG18 | `Password updated successfully.` |
| MSG127 | `TripMate is temporarily unable to process your request. Please check your connection and try again.` |

## Rules for Screen Specifications and Stitch

1. Never display the locked content of a wrong `MSG` reference merely because the detailed UC names that code.
2. When `Correct Match` is unambiguous, the final Web spec may use that existing locked message and record the detailed-UC reference defect.
3. For `MISSING_MESSAGE`, the spec describes the required behavior and may use neutral placeholder wording only as an explicit UX suggestion.
4. `AMBIGUOUS` copy is not mandatory in Stitch. The control/state may still be designed using neutral placeholder copy when the visible behavior is otherwise determined.
5. Correcting Report 3 message identifiers or locked content requires human approval outside this task.

## Stitch Impact

The message defects do not change the required controls, validation placement, screen decomposition, or navigation determined for Batch 1. Therefore they do not block visual prompt generation after review. Stitch prompts must use exact locked text only for `VALID` or unambiguous `Correct Match` rows and neutral visibly-labeled placeholder copy for `MISSING_MESSAGE` or `AMBIGUOUS` rows.
