# Approved TM-212 scope correction

**Approved and recorded on 2026-11-15.** The Owner approved removing the Tour Favorite dependency. The approved title and scope are reflected in Jira. The original Report 3 and the implementation scope of the underlying use cases were preserved.

Jira title: **Reconcile Tour Recommendation, Booking and Payment dependencies**

Replace “Trace Favorite to TM-74, Booking to TM-73, and Recommendation/Match to TM-71” with:

> Trace Recommendation/Match to TM-71 (UC-25), Booking to TM-73 (UC-27), and Payment to TM-74 (UC-28). Document each contract and the booking-to-payment handoff. Tour Favorite is excluded from TM-212. Any future Tour Favorite capability requires its own approved requirement and owning ticket.

Retain the existing checks on typed components, usable callbacks, and the absence of simulated or unavailable actions. Apply the owning-ticket criterion only to supported controls. Keep the global navigation question separate from the capability mapping and preserve the existing owners and contracts.

The Jira scope correction is **IMPLEMENTATION_COMPLETE**. [TM-212](https://tripmate-capstone.atlassian.net/browse/TM-212) retains its In Progress status for its other acceptance criteria. The mappings agree with §§3.5.2, 3.5.4 and 3.5.5 of the [official original Report 3](https://docs.google.com/document/d/1VwPkqRyC6ZlbXBQC0ztdektt0vAyE3qt/edit).
