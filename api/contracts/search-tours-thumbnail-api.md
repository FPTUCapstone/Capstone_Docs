# TM-208 — Public Tour search thumbnail API contract

Status: **TM-208 implementation contract — 2026-09-27**

Endpoint: `GET /api/v1/tours`

Source: approved [Tour Catalogue Enrichment addendum](../../requirements/tour-catalogue-enrichment.md), Jira [TM-208](https://tripmate-capstone.atlassian.net/browse/TM-208)

TM-208 extends the existing TM-70 public Tour search response by adding one
always-present, nullable property to every `items[]` element:

| Field | Type | Meaning |
| --- | --- | --- |
| `thumbnailUrl` | `string \| null` | HTTPS Cloudinary delivery URL of the Tour's active primary image, or `null` when there is no eligible primary image. |

The property is `null` if no active primary Tour image exists. An active image
with the smallest `sort_order` is **not** an implicit fallback when it is not
primary. Deleted images, POI photos, and media attached to Tours that do not
pass the existing public search predicate are not eligible. TM-206 permits
zero or one active primary image per Tour, so the result is unambiguous.

Jira's phrase "first ordered Tour photo" is interpreted in light of the
approved addendum: **primary-only**, with null when no public primary image
is available. The Tour owner confirmed that rule for TM-208 on 2026-09-27.
There is no independent per-image approval flag in the current data model;
the existing TM-70 Tour publication predicate and TM-207 media lifecycle
govern public visibility. This contract does not create another approval or
publication workflow.

All existing TM-70 response fields, filters, validation, page size, total
count, stable ordering, availability calculation, anonymous access, `no-store`
header, and RFC 7807 error responses remain unchanged. Success still returns
the direct paged DTO, not a synthetic envelope. Older clients may ignore the
new field; they need not display an image to continue searching.

The search endpoint returns only this nullable URL, not an ordered gallery,
Cloudinary public ID, upload key, operator-only metadata, POI image, or Tour
Detail payload. It does not call Cloudinary during the public read; the URL is
read from SQL Server metadata for Tour IDs on the selected page only.

Web and Mobile adoption can be scheduled separately. Both clients must allow
`null` and use their honest no-image presentation in that case; they must not
invent a POI-photo fallback.
