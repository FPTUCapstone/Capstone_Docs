# Staff and Administrator account management

Report 3 §3.9.1 and §5.1 govern account operations. Staff can manage Traveler, Tour Operator and Service Provider accounts. Administrators can manage Staff accounts. The signed-in actor must have an active account and their database role must match the signed role.

| Method | Path | Response |
|---|---|---|
| GET | `/api/v1/admin/users` | Page of account summaries filtered to the caller's scope |
| GET | `/api/v1/admin/users/{userId}` | Account summary, associated traveler booking count and moderation history |
| POST | `/api/v1/admin/users/{userId}/lock` | Updated account summary |
| POST | `/api/v1/admin/users/{userId}/unlock` | Updated account summary |

List filters are `page`, `pageSize`, `role`, `status`, `search`, `registeredFrom`, and `registeredTo`. Pagination defaults to page 1 with 20 entries and supports up to 100 entries per page. Results are sorted by descending registration time and ID. Account summaries expose decimal string IDs, name, email, role, status, registration time and last sign-in time.

Lock and unlock accept a JSON object containing only a nonblank `reason` of up to 200 characters. A lock preserves account state for restoration, records the reason, and revokes access and refresh sessions. Unlock restores the prior state. Existing debt safeguards apply to Tour Operator unlocks. An older Service Provider lock remains in effect when commission settlement cannot be verified. Successful mutations record audit history and queue a notice.

Errors are 400 for invalid inputs, 401 for missing or unusable authentication, 403 for actors without permission, 404 for unavailable or out-of-scope targets, and 409 for conflicting account state or unresolved settlement. The 404 response does not reveal whether the target exists.

Deploy the additive `20261115_tm228_user_session_boundaries.sql` migration before the API. The Web account directory and moderation operations require the updated API.
