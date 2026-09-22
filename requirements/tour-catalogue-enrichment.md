# Tour Catalogue Enrichment — Source-of-Truth Addendum

Status: **APPROVED FOR IMPLEMENTATION PLANNING — 2026-09-23**  
Jira: TM-204 (Epic), TM-205  
Owner: Tour catalogue product owner  
Last updated: 2026-09-23

## 1. Purpose and change boundary

This addendum closes a documented catalogue-content gap between the public Tour
experience and the currently implemented TM-70 search contract. It defines the
business decision boundary for Tour-owned media and the catalogue information
needed by future public Tour Detail work.

It does **not** reopen TM-70 scope. TM-70 remains a public, paginated Tour
search contract with no Tour image, rating, favourite, recommendation, booking,
or detail payload. Those capabilities require their own approved contracts and
delivery tasks under TM-204.

## 2. Authoritative inputs and traceability

| Source | Established fact | Effect of this addendum |
| --- | --- | --- |
| `requirements/mvp/search-tours-mvp.md` | A Tour card needs identity, price and availability; the screen specification additionally calls for a Tour image. | Defines how a future card may receive an approved Tour thumbnail. |
| `ux/screen-specifications/search-tours-screen-spec.md` | UC-24 requires Tour identity/image and operator/availability summary. | Does not alter the existing TM-70 API; a later contract may expose `thumbnailUrl`. |
| `requirements/mvp/view-tour-details-mvp.md` and UC-26 screen specification | Public Tour Detail requires complete Tour content and is image-led. | Establishes the media/content foundation for a later UC-26 contract. |
| `Report3_Screens_All.html` operator mockups | The visual mockup shows cover-photo upload and a submission checklist saying “at least 3 cover photos”. | Treated as a **proposal to decide**, not a production rule until approved below. |
| Current database/API implementation | `commerce.Tours` has no Tour-owned media relation; `catalog.POIPhotos` belongs to POIs. | Tour media must be modelled independently; POI photos must never be substituted as Tour photos. |

When this addendum is approved, it supersedes only the explicit TM-70
limitations concerning absent Tour media. It does not supersede Report 3
business rules or add a feature not recorded here.

## 3. Decisions requested for approval

The following choices are intentionally not inferred from UI mockups. Record a
decision, owner, and date in this section before TM-206 starts implementation.

| ID | Decision needed | Proposed default | Why it matters |
| --- | --- | --- | --- |
| D1 | Media storage | **Approved 2026-09-23:** Use Cloudinary as the dedicated media storage and delivery service for Tour-owned images. SQL Server stores only image metadata, Cloudinary public identifier, delivery URL, display order, primary-image state, and lifecycle information. Raw image binaries must not be stored in SQL Server. | Avoids database BLOB growth and makes Cloudinary ownership, delivery, and deletion traceable. Cloudinary upload presets, folder convention, transformation policy, retention, and moderation still need to be defined in the operator-media contract. |
| D2 | Publish completeness | **Approved 2026-09-23:** Require at least **3** active Tour images, including exactly one primary/cover image, before a Tour can be submitted for approval or published. | Aligns with the approved product decision and supplied operator mockup. |
| D3 | Capacity and formats | **Approved 2026-09-23:** Maximum **10** active images per Tour; JPEG, PNG, or WebP. Maximum file size and dimensions must be set by the Cloudinary/security policy before upload implementation. | Sets a bounded first release without inventing unsafe upload limits. |
| D4 | Editing a published Tour | **Approved 2026-09-23:** Only material changes that significantly affect the Tour's commercial offering, itinerary, or public presentation require a new approval cycle. Minor edits that do not materially change the Tour may be updated without re-approval. | Preserves approval control for commercially meaningful changes while allowing routine correction. The operator-media contract must classify material versus minor edits and retain an audit record for both. |
| D5 | Scope of this epic | **Approved 2026-09-23:** Tour media now; category, language, inclusions, cancellation policy, and rich detail content are separate decisions/contracts. | Prevents TM-204 from silently becoming all Tour-detail work. |

**Approval record:** D1–D5 approved by product owner on 2026-09-23.

## 4. Approved target behaviour

### 4.1 Tour-owned media

- A Tour owns an ordered collection of image metadata stored in SQL Server; the
  original and derived image assets are stored and delivered by Cloudinary.
- Exactly one active image is the primary/cover image for a Tour.
- The primary image is the only image a public list/search endpoint may expose
  as `thumbnailUrl`; an endpoint must return `null` when no public primary
  image is available.
- A public Tour Detail endpoint may expose the ordered image gallery only after
  TM-210 defines and approves its contract.
- Images must have an operator-managed caption that may be absent; clients must
  tolerate a null caption.
- A disabled, deleted, rejected, or non-public image must not be returned from
  public endpoints.
- `catalog.POIPhotos` remains POI content. It must not become a fallback image
  source for a Tour card or Tour Detail hero.

### 4.2 Operator and publication rules

- Only the owning authorised Tour Operator may create, order, replace, or
  remove that operator's Tour images.
- Media actions must be auditable with actor, Tour, action, and timestamp.
- Uploading a file is not itself publication. An image becomes publicly visible
  only through the approved Tour/publication state.
- A material change to commercial offering, itinerary, or public presentation
  requires a new approval cycle before its new public version is released.
- A minor edit that does not materially change the Tour may be released without
  re-approval, but must retain the same audit record as a material edit.
- TM-207 must define a deterministic material/minor classification. Until that
  contract is approved, an operator must not self-classify an ambiguous change
  as minor.
- The public search API must remain anonymous and must expose no upload key,
  private object URL, internal storage path, or operator-only metadata.

### 4.3 Deliberate exclusions

This addendum does not add or imply:

- rating or review summaries (TM-211);
- favourites, bookings, recommendations, or persona-match scores (TM-212);
- a Tour Detail endpoint or UI (TM-210);
- a generic asset library shared with POIs;
- arbitrary rich content fields outside a separately approved detail contract.

## 5. Contract and data invariants for downstream tasks

These are acceptance conditions for TM-206 through TM-210, not an API schema
implemented by this document.

1. The database migration is upgrade-safe and idempotent; upgrading the baseline
   schema and creating a fresh schema produce the same Tour-media inventory.
2. The data model preserves deterministic media order and has a database-level
   guarantee that there cannot be more than one active primary image per Tour.
3. Deleting or disabling the primary image has defined behaviour: select an
   explicitly promoted replacement, or return `thumbnailUrl: null`; never pick
   an arbitrary image silently.
4. Public thumbnails and galleries use stable HTTPS Cloudinary delivery URLs,
   not temporary upload credentials or database storage paths.
5. SQL Server stores the Cloudinary public identifier and lifecycle metadata so
   an authorised media-management workflow can safely replace or delete the
   matching Cloudinary asset without deriving identity from a delivery URL.
6. List/search returns only the nullable `thumbnailUrl` required by its contract;
   it does not transfer the full gallery.
7. Operator media write endpoints enforce ownership, validation, audit logging,
   and publication-state rules.
8. No field described here is added to the TM-70 response until TM-208 has an
   approved contract and compatibility plan.

## 6. Required follow-on documentation

After D1–D5 are approved, update the following in this order:

1. `requirements/mvp/search-tours-mvp.md` — explicitly distinguish current
   TM-70 card data from the planned thumbnail-capable contract.
2. `ux/screen-specifications/search-tours-screen-spec.md` — define the null
   thumbnail presentation and remove any visual assumption that has no data
   source.
3. `requirements/mvp/view-tour-details-mvp.md`, its user flow, and screen
   specification — make gallery/content requirements traceable to this
   addendum without expanding booking or review scope.
4. `api/contracts/` — create separate approved contracts for operator media
   management (TM-207), search thumbnail (TM-208), and Tour Detail (TM-210).
5. The relevant BE, Mobile, and Web specs — reference the approved API contracts
   rather than duplicating storage or publication rules.

## 7. TM-205 acceptance criteria

- [x] The current gap between UC-24/UC-26 visuals and TM-70's actual data
      contract is stated without claiming fake data is available.
- [x] Tour media is explicitly separated from POI media.
- [x] Media, ownership, visibility, and publication rules are traceable and
      approved by the product owner.
- [x] Downstream task boundaries and compatibility invariants are documented.
- [x] D1 is approved and recorded with storage responsibility boundaries.
- [x] D4 is approved with material/minor change boundaries.
- [x] D2, D3, and D5 are approved and the approval record is complete.
- [ ] The documentation change is reviewed and committed in `Capstone_Docs`.

## 8. Implementation inputs to define in TM-207

- Which Cloudinary upload preset, folder convention, delivery transformation,
  retention/deletion, and moderation workflow does TripMate approve?
- Which concrete image/content edits are material enough to require a new
  approval cycle, so TM-207 can apply the rule consistently?
