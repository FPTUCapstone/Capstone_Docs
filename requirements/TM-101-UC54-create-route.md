# TM-101 UC-54 Create Route

## Authority and approved clarifications

[Official live Report 3](https://docs.google.com/document/d/1VwPkqRyC6ZlbXBQC0ztdektt0vAyE3qt/edit), §3.9.4.1 and Table 5, governs this delivery. Drive revision 0B5_dfpH4IC_wQks1SFVRSmlzeFhsWmxtUjRZZzhMK3hJdDNBPQ was modified 2026-11-12T12:32:53.377Z. The verified DOCX SHA-256 is 093D69D721831608F462DE0AC450E4BBF3A8A96477B991622FDEAD1EFA3D25BA. The current Drive metadata was rechecked before publication.

The Owner approved these decisions in the implementation chat on 13 November 2026:

- Route Note is optional, blank becomes null, with a 2,000-character limit.
- MSG139: “Route created successfully.”
- MSG141: “Select two different Active points of interest.”
- MSG143: “Distance and estimated travel time must be positive values.”

The live SRS contains Figure 84 and all three message entries. The 13 November Owner approval supplies the implementation wording for MSG141 and MSG143, which differs from the live catalog. MSG139 agrees.

## Implemented boundary

Only an authenticated, currently Active Staff account may maintain routes. Administrator, Traveler, Tour Operator and unauthenticated accounts are refused. Backend checks authoritative account state independently of Web sessions. Create Route is UC-54; Update Route is UC-55 / TM-102 and excluded.

Screen 21 contains Active POI selectors, distance in kilometres, estimated duration in whole minutes, the existing four transport modes, optional Route Note, Mapbox preview, Create and Cancel. Screen 20 retrieves the server list after creation. Missing fields use inline MSG01; invalid endpoints MSG141; nonpositive numbers MSG143; forbidden access MSG126; applicable failure MSG127. Success is reported only after 201 and MSG139 from the Backend.

Distance uses the existing decimal(6,2) database representation. Inputs beyond its range/precision are rejected rather than rounded. Duration uses the existing positive SQL int. The authoritative existing `TransportMode` enum and SQL constraint agree on Walking, Motorbike, Car and PublicTransit. No options were invented.

## API contract

All management endpoints use `/api/v1/admin/routes`, existing bearer authentication and Result/ProblemDetails conventions.

| Method and path | Contract |
| --- | --- |
| GET `/api/v1/admin/routes?page=1&pageSize=20` | `{items, page, pageSize, totalCount}`; Staff-only paginated list |
| GET `/api/v1/admin/routes/options` | `{pois:[{id,name,latitude,longitude}],transportModes:[...]}`; only Active POIs |
| POST `/api/v1/admin/routes` | `Idempotency-Key` UUID header; `{fromPoiId,toPoiId,distanceKm,estimatedDurationMinutes,transportMode,routeNote}` |

201 returns the persisted Route DTO with identifier, endpoint names/identifiers, metrics, canonical mode, Active status, optional note, UTC creation time, MSG139 identifier and message. Repeated actor/operation identity and the existing directed endpoint/mode uniqueness constraint return conflict without creating another row. The SRS calls idempotency `CCR-13`; the task calls it CR-13.

The Web same-origin BFF forwards only fixed application paths, checks sealed Staff sessions, requires same-origin mutations, forwards the operation identifier and suppresses raw upstream server errors. Browser code does not receive Backend bearer tokens.

## Persistence and concurrency

The additive SQL migration adds status, note, creator and operation identity to existing `catalog.RouteSegments`. Existing scoring/terrain attributes are preserved and remain outside Create Route input. Original positive numeric, FK, no-self-loop, mode and unique constraints remain.

Legacy routes are backfilled Active only when both endpoints are Active. Unsafe legacy data or incompatible column definitions abort the transaction with no partial migration. A guarded rollback refuses to discard new note, operation or lifecycle data. It is not an instruction to run rollback on a deployed database.

Route creation rechecks Staff account and endpoint states under SERIALIZABLE and holds those reads through commit. The route and immutable `RouteCreate` audit event commit in one transaction. Audit/persistence failure rolls back both; bounded deadlock retry rechecks authorization and idempotency. A SQL route-write trigger rejects Active rows whose endpoints are inactive. Concurrent POI lifecycle changes are outside this task; consumer queries also require Active endpoints so unsafe routes are never used.

## Planning navigation and maps

`RoutePoint` carries optional POI identity. Scheduling and itinerary evaluation populate it for known POIs, allowing distinct POIs at the same coordinates to consume their own stored metrics. `CatalogRouteDurationProvider` selects Active routes by directed POI identity and mode, requiring both endpoints Active. Free GPS points may resolve an unambiguous Active POI by coordinates; unknown or ambiguous GPS pairs use the existing Mapbox provider for supported modes. A complete stored matrix needs no external routing call. Repeated instances of the same location become zero matrix cells; different known POIs retain their stored edge even when colocated. Provider capabilities allow complete catalog-backed PublicTransit matrices through planning and itinerary evaluation; incomplete transit graphs return the existing unavailable-routing result without inventing external transit support. No CSP objective or algorithm was replaced.

Navigation responses add `routeLegs` with current Active stored route identifiers, endpoints, metrics and modes. Reads do not modify historical itinerary times or versions. Guidance remains Mapbox. Existing Mobile parsing ignores unknown additive JSON fields and continues using its established per-leg transport modes; no Mobile screen was added.

Screen 21 reuses installed Mapbox GL and the existing Directions client. Walking uses the walking road profile. Other modes use the existing Web driving road geometry convention with an explicit notice for Motorbike/PublicTransit; the preview does not claim transit timetables or vehicle-specific restrictions. PublicTransit requires complete stored metrics because the existing Mapbox matrix provider has no transit profile. Stored polyline persistence is unnecessary.

## Traceability

| Requirement | Implementation | Verification target |
| --- | --- | --- |
| §3.9.4.1 / BR-115 / Screen 21 | Staff-only controller, authoritative account query, Web session and BFF gates | Role/active-account unit and endpoint tests |
| BR-122 / PC-01 | Domain factory, serialized endpoint reads, SQL constraints/trigger | Endpoint validation and real SQL tests |
| BR-24 / PC-02 | Catalog matrix provider and navigation route projection filter Active routes and endpoints | Provider, navigation and real SQL integration tests |
| BR-35 / Screen 21 | Stored metrics separate from Mapbox preview/guidance | Mapbox component tests and browser evidence |
| BR-130 / PC-03 | Existing immutable AuditLog model, atomic creation event | Audit content and induced audit-failure SQL tests |
| PC-04 | Validation before insert; transaction rollback | Invalid-input, audit/save-failure integration tests |
| CCR-13 | Disabled/ref-guarded submit; durable actor/UUID uniqueness | Duplicate and concurrent-operation tests |
| Screen 20 / MSG139 | Fresh server list after acknowledged creation | Web tests and browser evidence |

Use the final delivery report for actual command totals, PASS versus NOT_VERIFIED status, external-provider limits and publication state. A test target in this table is not itself a passing-test claim.
