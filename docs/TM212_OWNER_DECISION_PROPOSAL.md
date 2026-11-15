# Proposed TM-212 scope correction

**Decision requested from the Owner. This proposal has no effect until approved.**

Proposed title: **Reconcile Tour Recommendation, Booking and Payment dependencies**

Replace “Trace Favorite to TM-74, Booking to TM-73, and Recommendation/Match to TM-71” with:

> Trace Recommendation/Match to TM-71 (UC-25), Booking to TM-73 (UC-27), and Payment to TM-74 (UC-28). Document each contract and the booking-to-payment handoff. Tour Favorite is excluded from TM-212. Any future Tour Favorite capability requires its own approved requirement and owning ticket.

Retain the existing checks on typed components, usable callbacks, and the absence of simulated or unavailable actions. Apply the owning-ticket criterion only to supported controls. Keep the global navigation question separate from the capability mapping and preserve the existing owners and contracts.

Accept the correction when the sprint plan and Jira references agree with §§3.5.2, 3.5.4 and 3.5.5 of the [official original Report 3](https://docs.google.com/document/d/1VwPkqRyC6ZlbXBQC0ztdektt0vAyE3qt/edit). [TM-212](https://tripmate-capstone.atlassian.net/browse/TM-212) remains open pending the Owner’s decision.
