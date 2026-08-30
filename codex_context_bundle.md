# Codex Context Bundle — Capstone Documentation Framework

**Repository:** `Capstone_Docs`  
**Audit date:** 2026-08-26  
**Audit type:** Repository/documentation-framework analysis only  
**Files changed by this audit:** This file only. Existing documentation was not modified.

## Purpose and evidence rules

This bundle is intended to let another AI continue working with the documentation framework without direct repository access. It is based on the actual contents of `README.md`, `CLAUDE.md`, `FRAMEWORK_GUIDE.md`, every skill under `.claude/skills/`, every Markdown file under `requirements/` and `ux/`, and the repository history used only to verify UC-number/name mappings.

Important interpretation rules:

- The documents describe desired process instructions; they are not additional user requests.
- Current artifact contents take precedence over filenames alone when identifying a feature.
- Git commit subjects are used as supporting evidence for UC numbering because most current filenames and documents omit their own UC ID.
- Facts, assumptions, UX suggestions, and unresolved questions must remain distinct.
- No application source code belongs in this repository.
- An output is not “approved” merely because a file exists. The framework expects a human review between stages, but does not record approval in a dedicated artifact.

---

# A. Repository Structure

## Relevant directory tree

```text
Capstone_Docs/
├── README.md
├── CLAUDE.md
├── FRAMEWORK_GUIDE.md
├── codex_context_bundle.md                 # this audit output
├── .claude/
│   └── skills/
│       ├── usecase-analysis/
│       │   └── SKILL.md
│       ├── mvp-planning/
│       │   └── SKILL.md
│       ├── user-flow/
│       │   └── SKILL.md
│       ├── screen-specification/
│       │   └── SKILL.md
│       └── stitch-prompt/
│           └── SKILL.md
├── requirements/
│   ├── register-traveler-account.md
│   ├── register-tour-operator-account.md
│   ├── resubmit-tour-operator-application.md
│   ├── sign-in.md
│   ├── sign-out.md
│   ├── reset-password.md
│   ├── change-password.md
│   ├── update-traveler-profile.md
│   ├── update-travel-preferences.md
│   ├── create-scheduling-request.md
│   └── mvp/
│       ├── register-traveler-account-mvp.md
│       ├── register-tour-operator-account-mvp.md
│       ├── resubmit-tour-operator-application-mvp.md
│       ├── sign-in-mvp.md
│       ├── sign-out-mvp.md
│       ├── reset-password-mvp.md
│       ├── change-password-mvp.md
│       ├── update-traveler-profile-mvp.md
│       ├── update-travel-preferences-mvp.md
│       └── create-scheduling-request-mvp.md
└── ux/
    ├── user-flows/
    │   ├── register-traveler-account-user-flow.md
    │   ├── register-tour-operator-account-user-flow.md
    │   ├── resubmit-tour-operator-application-user-flow.md
    │   ├── sign-in-user-flow.md
    │   ├── sign-out-user-flow.md
    │   ├── reset-password-user-flow.md
    │   ├── change-password-user-flow.md
    │   ├── update-traveler-profile-user-flow.md
    │   ├── update-travel-preferences-user-flow.md
    │   └── create-scheduling-request-user-flow.md
    ├── screen-specifications/
    │   ├── register-traveler-account-screen-spec.md
    │   ├── register-tour-operator-account-screen-spec.md
    │   ├── resubmit-tour-operator-application-screen-spec.md
    │   ├── sign-in-screen-spec.md
    │   ├── sign-out-screen-spec.md
    │   ├── reset-password-screen-spec.md
    │   ├── change-password-screen-spec.md
    │   ├── update-traveler-profile-screen-spec.md
    │   ├── update-travel-preferences-screen-spec.md
    │   └── create-scheduling-request-screen-spec.md
    └── stitch-prompts/
        ├── register-traveler-account-stitch-prompt.md
        ├── register-tour-operator-account-stitch-prompt.md
        ├── resubmit-tour-operator-application-stitch-prompt.md
        ├── sign-in-stitch-prompt.md
        ├── sign-out-stitch-prompt.md
        ├── reset-password-stitch-prompt.md
        ├── change-password-stitch-prompt.md
        ├── update-traveler-profile-stitch-prompt.md
        ├── update-travel-preferences-stitch-prompt.md
        └── create-scheduling-request-stitch-prompt.md
```

## Declared but absent locations

- `api/contracts/` is declared in `CLAUDE.md` and `FRAMEWORK_GUIDE.md`, but the directory does not currently exist.
- `architecture/` is declared in `CLAUDE.md`, but the directory does not currently exist.
- There is no raw-use-case directory separate from `requirements/`.
- There is no User Flow Review directory or review artifact naming convention.
- Google Stitch output and the resulting UI/UX MVP are explicitly outside this repository.
- Implementation is explicitly performed in other repositories.

## Repository role

The repository is a documentation source of truth, not an application repository. It is intended to transform high-level business Use Cases into traceable product, UX, and technical documentation for later Google Stitch generation, API-contract design, and implementation in Next.js, Flutter, and .NET repositories.

---

# B. Framework Pipeline

## Literal pipeline versus implemented pipeline

`README.md` gives the fullest conceptual pipeline:

```text
Use Case
→ Use Case Analysis
→ Requirement Analysis
→ MVP Planning
→ MVP Scope
→ User Flow
→ User Flow Review
→ Screen Specification
→ Stitch Prompt
→ Google Stitch
→ UI/UX MVP
→ API Contract
→ Implementation
```

The repository does not implement each label as a separate stage. The operational pipeline in `FRAMEWORK_GUIDE.md` has five skills:

```text
Handwritten Use Case
→ /usecase-analysis (produces the combined analysis/requirements document)
→ /mvp-planning (produces the MVP scope document)
→ /user-flow
→ manual review/approval
→ /screen-specification
→ /stitch-prompt
→ manually paste into Google Stitch
→ UI/UX MVP outside the repository
→ API Contract (no skill yet)
→ Implementation in another repository
```

Therefore:

- “Use Case Analysis” and “Requirement Analysis” are conceptually separate in `README.md`, but are collapsed into one `/usecase-analysis` output in the actual framework.
- “MVP Planning” is the activity and “MVP Scope” is the output of the same `/mvp-planning` skill.
- “User Flow Review” is a manual approval checkpoint, not a skill or stored artifact.
- Google Stitch and UI/UX MVP are external/manual stages.
- API Contract and Implementation have no repository skills.

## Stage-by-stage contract

| Stage | Input | Output | Responsible skill/party | Invocation method | Expected location and naming | Dependency |
|---|---|---|---|---|---|---|
| Use Case | Business/use-case definition supplied by a human | Original Use Case source | Human/manual | No command is defined. `FRAMEWORK_GUIDE.md` says to create it manually. | `requirements/<feature-name>.md`; `<feature-name>` is kebab-case | None |
| Use Case Analysis | Original Use Case | Actors, preconditions, main/alternative/exception flows, postconditions, functional requirements, business rules, edge cases, missing information, and preliminary MVP scope | `/usecase-analysis` | `/usecase-analysis` followed by a natural-language request naming the source path | `requirements/<feature-name>.md` | Original Use Case must exist |
| Requirement Analysis | Conceptually the analyzed Use Case | There is no separate artifact. The `/usecase-analysis` document already contains functional requirements, business rules, gaps, assumptions, and questions and is called “Requirement Analysis” by `FRAMEWORK_GUIDE.md`. | No separate skill | No separate invocation is documented | Same combined `requirements/<feature-name>.md` file | Collapsed into Use Case Analysis |
| MVP Planning | Approved combined requirement/use-case analysis | Prioritization and planning: goal, target user, core problem/journey, Must/Should/Could/Out of Scope, dependencies, risks, questions | `/mvp-planning` | `/mvp-planning` followed by a request naming the approved requirement file | The generated document is also the MVP Scope: `requirements/mvp/<feature-name>-mvp.md` | Approved Requirement Analysis |
| MVP Scope | Result of MVP planning | The approved scope document used downstream | `/mvp-planning`; human approves it | No second command; this is the output of MVP Planning | `requirements/mvp/<feature-name>-mvp.md` | MVP Planning |
| User Flow | Approved MVP scope | Minimal end-to-end flow containing entry point, goal, user/system actions, alternatives, validation, errors, success, navigation, Mermaid diagram, and open questions | `/user-flow` | `/user-flow` followed by a request naming the MVP file | `ux/user-flows/<feature-name>-user-flow.md` | Approved MVP Scope |
| User Flow Review | Generated User Flow and its upstream sources | Human approval/corrections; no defined review report | Human/manual | No skill or command exists. The guide says to stop, read, and approve before continuing. | No directory or naming convention | User Flow |
| Screen Specification | Approved User Flow, MVP Scope, and Requirement Analysis | Minimal screen inventory and a specification for each required screen, including components, actions, validation, errors, states, navigation, business rules, open questions, and UX suggestions | `/screen-specification` | `/screen-specification` followed by a request naming the User Flow file | `ux/screen-specifications/<feature-name>-screen-spec.md` | Approved User Flow plus its approved MVP and requirements sources |
| Stitch Prompt | Approved Screen Specification, User Flow, and MVP Scope | A structured design brief and final English Google Stitch prompt | `/stitch-prompt` | `/stitch-prompt` followed by a request naming the Screen Specification | `ux/stitch-prompts/<feature-name>-stitch-prompt.md` | Approved Screen Specification plus User Flow and MVP Scope |
| Google Stitch | The `# Stitch Prompt` portion of the generated prompt document | Generated UI mockup(s) | Human + external Google Stitch service | Copy/paste manually; no repository command | Outside the repository | Approved Stitch Prompt |
| UI/UX MVP | Google Stitch result plus human design review | Approved UI/UX mockup/prototype | External/manual | No skill or command exists | Explicitly not stored in this repository according to the guide | Google Stitch generation and review |
| API Contract | Approved product/UX requirements; the exact input contract is not documented | API contract | No skill exists yet | No invocation documented | Intended: `api/contracts/`; no filename convention documented; directory absent | Comes after UI/UX MVP in the declared pipeline |
| Implementation | Approved docs, UI/UX MVP, and API contract; exact handoff contract is not documented | Next.js, Flutter, and/or .NET application code | Other repositories/implementation teams | No command in this repository | Never in `Capstone_Docs` | API Contract and approved prior artifacts |

## Approval semantics

The word “approved” appears throughout the skill inputs, but the skills do not define an approval field, status file, frontmatter key, review template, or approval command. The only operational instruction is manual: stop after each stage, read the output, resolve or explicitly retain open questions, and approve before running the next skill. Existence of a downstream artifact implies that someone proceeded, but it is not formal proof of approval.

## Source-of-truth rules

- Each stage must use the approved output of the previous stage as its primary truth.
- Screen Specification additionally consults MVP Scope and Requirement Analysis.
- Stitch Prompt additionally consults User Flow and MVP Scope.
- Requirements must not be invented.
- Assumptions, suggestions, and unresolved questions must be labeled.
- Only Must Have functionality should become required MVP screens.
- Source Use Cases must not be changed without explicit instruction.

## Important framework contradiction

The `/usecase-analysis` skill says “Do not modify the original Use Case,” while `FRAMEWORK_GUIDE.md` says the analysis may overwrite/augment the same `requirements/<feature-name>.md` file. Because the input and output naming collide, preservation is not structurally guaranteed. Until this is resolved, safest operation is to preserve the original text verbatim inside the combined file or create a separately named analysis file only after the repository owner explicitly approves a revised convention.

---

# C. Skills

There are exactly five skills under `.claude/skills/`. No other skill is present.

## 1. `usecase-analysis`

- **Purpose:** Analyze a raw Use Case while preserving its business meaning; identify actors, preconditions, postconditions, main/alternative/exception flows, functional requirements, business rules, ambiguity, edge cases, missing information, and preliminary MVP scope.
- **Required inputs:** The original Use Case. The skill does not define a fixed raw Use Case template.
- **Generated outputs:** Sections named `Overview`, `Actors`, `Preconditions`, `Main Flow`, `Alternative Flows`, `Exception Flows`, `Postconditions`, `Functional Requirements`, `Business Rules`, `Edge Cases`, `Missing Information`, and `MVP Scope`.
- **Output directory:** `requirements/`.
- **Expected filename:** `<feature-name>.md`.
- **Invocation:** `/usecase-analysis`, followed by a request such as `Phân tích use case tại requirements/register-traveler-account.md`.
- **Modifies existing files:** Ambiguous. The skill explicitly says not to modify the original Use Case, but the guide permits overwriting/augmenting the same path. Existing repository practice uses one combined file per feature; only the Traveler registration file retains a clearly labeled verbatim source block.
- **Must run before:** `mvp-planning`.
- **Must run after:** Manual creation/provision of the original Use Case.
- **Integrity constraints:** Do not generate code or invent business rules; label assumptions; separate explicit requirements, assumptions, suggestions, and questions/missing information.

## 2. `mvp-planning`

- **Purpose:** Define the smallest product scope that validates the primary user value.
- **Required inputs:** Approved Requirement Analysis, which in this repository means the combined `requirements/<feature-name>.md` produced by `usecase-analysis`.
- **Generated outputs:** `MVP Goal`, `Target User`, `Core User Problem`, `Core User Journey`, `Must Have`, `Should Have`, `Could Have`, `Out of Scope`, `MVP User Journey`, `Dependencies`, `Risks`, and `Open Questions`.
- **Output directory:** `requirements/mvp/`.
- **Expected filename:** `<feature-name>-mvp.md`.
- **Invocation:** `/mvp-planning`, followed by a request such as `Lập MVP scope từ requirements/register-traveler-account.md`.
- **Modifies existing files:** Not documented as modifying existing files; it generates the MVP file. Re-running against an existing output could overwrite it, but overwrite behavior is not specified.
- **Must run before:** `user-flow`.
- **Must run after:** `usecase-analysis` output is reviewed and approved.
- **Integrity constraints:** Complete one clear core journey, minimize screens and complexity, and classify features as Must/Should/Could/Out of Scope.

## 3. `user-flow`

- **Purpose:** Turn the approved MVP scope into a minimal end-to-end user journey without designing UI.
- **Required inputs:** Approved MVP Planning/MVP Scope document.
- **Generated outputs:** `User Flow Overview`, `Actor`, `Entry Point`, `Primary User Goal`, stepwise `Main Flow` with User Action/System Action/Next State, `Alternative Flows`, `Validation Flow`, `Error Flow`, `Success State`, `Navigation Map`, Mermaid `Flow Diagram`, and `Open Questions`.
- **Output directory:** `ux/user-flows/`.
- **Expected filename:** `<feature-name>-user-flow.md`.
- **Invocation:** `/user-flow`, followed by a request such as `Tạo user flow từ requirements/mvp/register-traveler-account-mvp.md`.
- **Modifies existing files:** No modification behavior is documented; it generates a user-flow file.
- **Must run before:** Manual User Flow review, then `screen-specification`.
- **Must run after:** `mvp-planning` output is reviewed and approved.
- **Integrity constraints:** Use only approved MVP scope, minimize steps, separate user/system actions, include important validation and failure branches, and do not create application code or UI design.

## 4. `screen-specification`

- **Purpose:** Convert an approved User Flow into the minimum set of screen specifications needed for the Must Have MVP journey and later Stitch generation.
- **Required inputs:** (1) approved User Flow, (2) MVP Planning/MVP Scope, and (3) Requirement Analysis.
- **Generated outputs:** A `Screen Inventory`; then, per screen: `Purpose`, `User`, `Entry Conditions`, `Required Components` with Inputs/Information Display/Primary Actions/Secondary Actions, `Validation`, `Error Handling`, `States`, `Navigation`, `Business Rules`, `Open Questions`, and `UX Suggestions`.
- **Output directory:** `ux/screen-specifications/`.
- **Expected filename:** `<feature-name>-screen-spec.md`.
- **Invocation:** `/screen-specification`, followed by a request such as `Tạo screen spec từ ux/user-flows/register-traveler-account-user-flow.md`.
- **Modifies existing files:** No modification behavior is documented; it generates a screen-spec file.
- **Must run before:** `stitch-prompt`.
- **Must run after:** `user-flow` and manual User Flow approval, with the approved MVP and requirements available.
- **Integrity constraints:** Do not invent requirements/features/states or promote UX suggestions to requirements; focus on Must Have functionality, reuse screens, and clearly label assumptions and unresolved decisions.

## 5. `stitch-prompt`

- **Purpose:** Convert an approved Screen Specification into a concise Google Stitch design brief and final prompt.
- **Required inputs:** (1) Screen Specification, (2) User Flow, and (3) MVP Planning/MVP Scope.
- **Generated outputs:** `Product Context`, `Screen Objective`, `Target User`, `Screen Content`, `Required Components`, `Required States`, `Navigation Context`, `UX Constraints`, `Open Questions`, and `Stitch Prompt`. Multi-screen artifacts may use per-screen subsections inside one feature-level file.
- **Output directory:** `ux/stitch-prompts/`.
- **Expected filename:** `<feature-name>-stitch-prompt.md`.
- **Invocation:** `/stitch-prompt`, followed by a request such as `Tạo stitch prompt từ ux/screen-specifications/register-traveler-account-screen-spec.md`.
- **Modifies existing files:** No modification behavior is documented; it generates a Stitch-prompt file.
- **Must run before:** Manual copy/paste to Google Stitch.
- **Must run after:** `screen-specification` output is reviewed and approved.
- **Integrity constraints:** English prompt, mobile-first, MVP-focused, accessible, visually and interaction focused, no implementation details, no invented unresolved requirements, and a clear distinction between required elements and optional UX suggestions.

## Skills that do not exist

There is no repository skill for:

- standalone Requirement Analysis;
- standalone MVP Scope after MVP Planning;
- User Flow Review;
- Google Stitch execution;
- UI/UX MVP capture or review;
- API Contract generation;
- implementation.

Do not create fictional slash commands for these stages.

---

# D. Output Locations

| Artifact | Location | Naming convention | Created by |
|---|---|---|---|
| Original Use Case | `requirements/` | `<feature-name>.md` | Human/manual |
| Combined Use Case + Requirement Analysis | `requirements/` | `<feature-name>.md` | `/usecase-analysis` |
| MVP Planning / MVP Scope | `requirements/mvp/` | `<feature-name>-mvp.md` | `/mvp-planning` |
| User Flow | `ux/user-flows/` | `<feature-name>-user-flow.md` | `/user-flow` |
| User Flow Review | None | None | Manual checkpoint only |
| Screen Specification | `ux/screen-specifications/` | `<feature-name>-screen-spec.md` | `/screen-specification` |
| Stitch Prompt | `ux/stitch-prompts/` | `<feature-name>-stitch-prompt.md` | `/stitch-prompt` |
| Google Stitch mockup / UI/UX MVP | Outside repository | Not documented | Google Stitch + human |
| API Contract | Intended `api/contracts/` | Not documented | No skill yet |
| Architecture docs | Intended `architecture/` | Not documented | No skill defined here |
| Implementation | Other repositories | Not documented here | Implementation teams/agents |

`<feature-name>` must be kebab-case and remain identical across all five generated feature files. UC IDs are not part of the documented filename convention.

---

# E. UC Artifact Matrix

## UC-number mapping

The current files mostly omit UC IDs, so mapping was checked against content, cross-references, and commit history:

| UC ID | Feature | Evidence status |
|---|---|---|
| UC-01 | Register Traveler Account | Inferred from sequence: it is the first feature chain, but current file contents and its creation commit do not state `UC-01` explicitly. |
| UC-02 | Register Tour Operator Account | Inferred from sequence between UC-01 and explicitly identified UC-03; current file contents and creation commits do not state `UC-02` explicitly. |
| UC-03 | Resubmit Tour Operator Application | Directly supported by repository commit subjects and current cross-references. |
| UC-04 | Sign In | Directly supported by repository commit subjects and many current cross-references. |
| UC-05 | Sign Out | Directly supported by repository commit subjects. |
| UC-06 | Reset Password | Directly supported by repository commit subjects and current cross-references. |
| UC-07 | Change Password | Directly supported by repository commit subjects. |
| UC-08 | Update Traveler Profile | Directly supported by repository commit subjects. |
| UC-09 | Update Travel Preferences | Directly supported by repository commit subjects and current cross-references. |
| UC-10 | Create Scheduling Request | Directly supported by repository commit subjects and current cross-references. |

## Status legend

- ✅ Exists and is consistent enough with its upstream chain for the presence audit.
- ❌ Missing.
- ⚠️ Exists but is structurally combined, stale, or overlaps another canonical UC. Details follow the matrix.

## Matrix

| UC / Feature | Use Case Analysis | Requirement Analysis | MVP Scope | User Flow | User Flow Review | Screen Specification | Stitch Prompt |
|---|---:|---:|---:|---:|---:|---:|---:|
| UC-01* Register Traveler Account | ✅ | ⚠️ | ⚠️ | ⚠️ | ❌ | ⚠️ | ⚠️ |
| UC-02* Register Tour Operator Account | ⚠️ | ⚠️ | ⚠️ | ⚠️ | ❌ | ⚠️ | ⚠️ |
| UC-03 Resubmit Tour Operator Application | ✅ | ⚠️ | ✅ | ✅ | ❌ | ✅ | ✅ |
| UC-04 Sign In | ✅ | ⚠️ | ✅ | ✅ | ❌ | ✅ | ✅ |
| UC-05 Sign Out | ✅ | ⚠️ | ✅ | ✅ | ❌ | ✅ | ✅ |
| UC-06 Reset Password | ✅ | ⚠️ | ✅ | ✅ | ❌ | ✅ | ✅ |
| UC-07 Change Password | ✅ | ⚠️ | ✅ | ✅ | ❌ | ✅ | ✅ |
| UC-08 Update Traveler Profile | ✅ | ⚠️ | ✅ | ✅ | ❌ | ✅ | ✅ |
| UC-09 Update Travel Preferences | ✅ | ⚠️ | ✅ | ✅ | ❌ | ✅ | ✅ |
| UC-10 Create Scheduling Request | ✅ | ⚠️ | ✅ | ✅ | ❌ | ✅ | ✅ |

`*` UC-01 and UC-02 IDs are inferred, not directly asserted in the current artifacts.

## Matrix notes

### All Requirement Analysis cells are ⚠️

Every `requirements/<feature>.md` file contains functional requirements and business rules, so the substantive analysis exists. However, there is no separate Requirement Analysis artifact or skill; the same file doubles as Use Case Analysis and Requirement Analysis. This is a framework inconsistency, not ten missing files.

### All User Flow Review cells are ❌

No review skill, review directory, review report, or approval marker exists. Manual review is recommended in the guide, but there is no durable artifact proving that it happened.

### UC-01 Register Traveler Account

- The actual MVP file is `requirements/mvp/register-traveler-account-mvp.md`.
- The requirement analysis, User Flow, Screen Specification, and Stitch Prompt refer in prose/backticks to the nonexistent `requirements/mvp/register-traveler-account.md`. This is a stale path from before the file was renamed to match the convention.
- The Screen Specification and Stitch Prompt say no independent Login/Sign In use case has been analyzed, but a complete UC-04 Sign In chain now exists. Those statements are stale.
- The Register Traveler Screen Specification includes a minimal Login Screen stub, while the Stitch Prompt deliberately designs only Registration. This was defensible when generated but should now be reconciled with UC-04 before regeneration.
- The requirement file is the only feature analysis that preserves a clearly labeled verbatim source Use Case block. Other feature analyses do not provide equivalent raw-source provenance.

### UC-02 Register Tour Operator Account

- The analysis says the use case covers registration/submission only, yet later includes rejection, editing, and resubmission in AF3, FR11, BR8, MVP scope, User Flow, Screen Specification, and Stitch Prompt.
- Resubmission is independently documented as UC-03 with a different actor/state model: an authenticated Tour Operator with a Rejected application.
- UC-02’s flow calls the returning actor a Guest and reuses the registration form; UC-03 explicitly excludes authentication fields and requires Sign In. The overlap makes both chains exist but inconsistent at the UC boundary.

### UC-05 Sign Out

- A Screen Specification and Stitch Prompt exist, but both correctly state that no dedicated Must Have screen is required.
- The only visual prompt is for an optional confirmation dialog classified as Could Have. The prompt itself says that if the dialog is not built, there is nothing to generate in Stitch for this UC. This is consistent, not missing.

### UC-09 Update Travel Preferences

- Downstream artifacts use concrete option lists and control types, but every relevant file clearly labels them as placeholder assumptions pending product confirmation.
- The artifacts therefore exist and are traceable, but should not be treated as final requirements until the option domains are approved.

### UC-10 Create Scheduling Request

- Result presentation as an ordered list of stops/times is a labeled MVP judgment because the source does not specify presentation format.
- “Mandatory locations” is treated as repeatable/multiple and possibly optional downstream, while the exact required/optional constraints remain open. These are explicitly labeled rather than silently asserted.

---

# F. Detected Conflicts

## UC numbering audit result

No direct contradictory pair such as “UC-07 = Feature A” in one current file and “UC-07 = Feature B” in another was found. UC-03 through UC-10 form a coherent sequence supported by commit history and cross-references. UC-01 and UC-02 are not explicitly numbered in current artifacts, so their IDs must be treated as inferred rather than proven.

The repository does contain a significant UC-02/UC-03 scope conflict and several cross-artifact staleness conflicts.

### Conflict 1 — UC-02 registration includes UC-03 resubmission

```text
CONFLICT:
Repository artifact: requirements/register-tour-operator-account.md and every downstream UC-02 artifact include edit-and-resubmit after rejection.
Canonical/use-case source: Repository history and the dedicated requirements/resubmit-tour-operator-application.md chain identify “Resubmit Tour Operator Application” as UC-03.
Reason: A stakeholder decision about resubmission was folded into the registration chain before/while a separate UC-03 chain was created, leaving duplicated ownership and different actor/entry assumptions.
Recommended action: Do not rename automatically. Decide the boundary explicitly. Prefer UC-02 ending at Pending Approval and referencing UC-03 for rejected-application resubmission; keep shared validation/file rules referenced rather than duplicated. Then regenerate/review only affected downstream artifacts.
```

### Conflict 2 — UC-01 documents claim UC-04 does not exist

```text
CONFLICT:
Repository artifact: ux/screen-specifications/register-traveler-account-screen-spec.md and ux/stitch-prompts/register-traveler-account-stitch-prompt.md state that no independent Login use case has been analyzed.
Canonical/use-case source: The repository now contains the complete UC-04 Sign In chain: requirements/sign-in.md, its MVP, User Flow, Screen Specification, and Stitch Prompt.
Reason: UC-01 downstream documents were generated earlier and were not refreshed after UC-04 was added.
Recommended action: Do not rename anything. Review UC-01’s post-registration handoff against the approved UC-04 User Flow/Screen Specification, remove the obsolete Login stub or clearly link to UC-04, and regenerate the UC-01 Stitch Prompt only if the approved screen scope changes.
```

### Conflict 3 — UC-01 references the wrong MVP filename

```text
CONFLICT:
Repository artifact: requirements/register-traveler-account.md, ux/user-flows/register-traveler-account-user-flow.md, ux/screen-specifications/register-traveler-account-screen-spec.md, and ux/stitch-prompts/register-traveler-account-stitch-prompt.md reference requirements/mvp/register-traveler-account.md.
Canonical/use-case source: The actual convention-compliant file is requirements/mvp/register-traveler-account-mvp.md.
Reason: The MVP file was renamed to the documented -mvp.md convention, but prose/backtick references were not updated.
Recommended action: Update references only after explicit permission to modify existing documentation; do not rename the correct existing MVP file.
```

### Conflict 4 — raw Use Case preservation versus same-path output

```text
CONFLICT:
Repository artifact: .claude/skills/usecase-analysis/SKILL.md says not to modify the original Use Case, while FRAMEWORK_GUIDE.md permits overwriting/augmenting requirements/<feature-name>.md with the analysis.
Canonical/use-case source: The framework has no separate raw-use-case directory or separate analysis naming convention. Only register-traveler-account.md visibly preserves a verbatim source block.
Reason: Input and output use the same path, so the skill’s preservation rule and the guide’s storage rule are structurally incompatible.
Recommended action: Decide and document one strategy before processing new UCs: either preserve a verbatim source section in the combined file, or introduce separate raw/analysis paths. Do not silently change the convention.
```

### Conflict 5 — conceptual stages do not match available skills

```text
CONFLICT:
Repository artifact: README.md lists separate Use Case Analysis, Requirement Analysis, MVP Planning, MVP Scope, and User Flow Review stages.
Canonical/use-case source: FRAMEWORK_GUIDE.md and .claude/skills provide one combined usecase-analysis output, one MVP output, and no review skill/artifact.
Reason: The conceptual lifecycle is more granular than the implemented document-generation framework.
Recommended action: Treat the stages as conceptual checkpoints, not fictional commands. If separate artifacts are desired, define skills, directories, filenames, and migration rules before changing existing chains.
```

### Conflict 6 — declared API/architecture outputs are not implemented

```text
CONFLICT:
Repository artifact: CLAUDE.md declares api/contracts/ and architecture/ output locations; README.md includes API Contract in the pipeline.
Canonical/use-case source: Neither directory exists and FRAMEWORK_GUIDE.md explicitly says no API Contract skill exists yet.
Reason: Future-state documentation was declared before tooling and directory structure were implemented.
Recommended action: Do not invoke a fictional API skill. Define the API-contract source-of-truth inputs, template, naming convention, and skill before beginning that stage.
```

---

# G. Stitch Workflow

## Exact repository-supported path from Use Case to Google Stitch

1. Start with the original Use Case in `requirements/<feature-name>.md`.
2. Run `/usecase-analysis` against that file.
3. Review the combined analysis, especially `Missing Information`, and record decisions clearly. Do not treat unresolved assumptions as requirements.
4. Run `/mvp-planning` against the approved requirement file to create `requirements/mvp/<feature-name>-mvp.md`.
5. Review and approve Must/Should/Could/Out of Scope and the core journey.
6. Run `/user-flow` against the approved MVP file to create `ux/user-flows/<feature-name>-user-flow.md`.
7. Manually review the flow, validation/error branches, Mermaid diagram, and navigation. There is no `/user-flow-review` skill.
8. Run `/screen-specification` against the approved User Flow. The skill must also consult the MVP Scope and Requirement Analysis. It creates `ux/screen-specifications/<feature-name>-screen-spec.md`.
9. Review the screen inventory, required components, states, navigation, open questions, and separation of requirements from UX suggestions.
10. Run `/stitch-prompt` against the approved Screen Specification. The skill must also consult the User Flow and MVP Scope. It creates `ux/stitch-prompts/<feature-name>-stitch-prompt.md`.
11. Copy the final `# Stitch Prompt` section from that file and paste it into Google Stitch manually.
12. Review the resulting mockup outside the repository. The guide states that the UI/UX MVP is not stored here.

## Which skill creates what

- Screen specifications are created by `/screen-specification`.
- Google Stitch prompts are created by `/stitch-prompt`.

## Required prerequisite files

For Screen Specification:

```text
requirements/<feature-name>.md
requirements/mvp/<feature-name>-mvp.md
ux/user-flows/<feature-name>-user-flow.md
```

For Stitch Prompt:

```text
requirements/mvp/<feature-name>-mvp.md
ux/user-flows/<feature-name>-user-flow.md
ux/screen-specifications/<feature-name>-screen-spec.md
```

The Stitch skill does not list Requirement Analysis as a direct source, but Requirement Analysis influences the prompt through the approved MVP, User Flow, and Screen Specification. The Screen Specification skill directly lists it as a source.

## Mobile, web, or both

The current Stitch skill is explicitly **mobile-first**. Existing prompts consistently request mobile viewports and single-column mobile layouts. The repository does not define a web-specific Stitch skill, responsive breakpoint rules, desktop variants, or a “both mobile and web” prompt mode.

Although `README.md` says the documentation may eventually support Next.js and Flutter implementation, that does not change the current Stitch generation contract. Treat current Stitch output as mobile-first UI design. A web/desktop brief would require an explicitly approved extension to the framework; do not infer it from the existence of Next.js as a later implementation target.

## Multiple screens for one UC

Multiple screens are represented inside one feature-level Screen Specification and one feature-level Stitch Prompt file, not as one file per screen. The screen spec starts with `Screen Inventory` and repeats `# Screen: <Screen Name>`. The Stitch file uses per-screen sections such as `Screen 1` and `Screen 2`, then a single final prompt that requests connected screens.

Current examples:

- UC-06 Reset Password: Request Reset + Set New Password.
- UC-10 Create Scheduling Request: Request Form + Itinerary Result.
- UC-02 Register Tour Operator: Registration Form + Pending Approval Confirmation.
- UC-01 Screen Specification lists Registration + a minimal Login stub, but its Stitch Prompt generates Registration only because Login was considered outside that brief at generation time.
- UC-05 Sign Out has no required screen; its prompt covers only an optional confirmation dialog.

## Navigation and states

- **Navigation:** User Flow contains a `Navigation Map`; Screen Specification has a `Navigation` section per screen; Stitch Prompt converts this into `Navigation Context` with entry, success destination, failure/stay behavior, and connections between screens.
- **Validation:** User Flow has `Validation Flow`; Screen Specification defines field/business validation; Stitch Prompt lists validation/error visuals.
- **Error:** User Flow separates error branches; Screen Specification labels validation, business, and system errors; Stitch Prompt describes error states and placement/tone.
- **Loading:** Screen Specification includes Loading only where supported or clearly labeled as a UX suggestion; Stitch Prompt represents it under `Required States` or `States to show`.
- **Empty/default:** Represented as `Initial`, `Input`, or `Empty / Default` when needed. UC-09 explicitly has a first-time empty/default preferences state.
- **Success:** May be an in-place confirmation, a dedicated screen, or navigation away. The prompt should not invent a success screen when the approved flow only redirects.
- **Business/expected non-success:** UC-10 models infeasible constraints as an expected non-alarming state distinct from system error.
- **Open questions:** Preserved in dedicated sections. The Stitch prompt must not convert them into mandatory behavior; optional UX suggestions must remain labeled.

## Existing Stitch coverage

All ten identified UCs already have a Stitch Prompt file. “Generate screens” for an existing UC should normally mean review/approve the existing chain, resolve material open questions, then paste the existing final prompt into Google Stitch. Regeneration is appropriate only when upstream approved content changed or the artifact is one of the inconsistent chains identified above.

---

# H. Recommended Next Action

## Exact workflow for a new UC that currently has only a Use Case definition

The slash commands below are the exact skill names defined by this repository. The text following each command is a natural-language request, not a shell command.

```text
UC-XX / <feature-name>

0. Manually place the original Use Case at:
   requirements/<feature-name>.md

1. Run:
   /usecase-analysis
   Phân tích use case tại requirements/<feature-name>.md

2. Manually review requirements/<feature-name>.md.
   Resolve or explicitly retain Missing Information.
   Confirm facts versus assumptions/suggestions.

3. Run:
   /mvp-planning
   Lập MVP scope từ requirements/<feature-name>.md

4. Manually review and approve:
   requirements/mvp/<feature-name>-mvp.md

5. Run:
   /user-flow
   Tạo user flow từ requirements/mvp/<feature-name>-mvp.md

6. Manually review and approve:
   ux/user-flows/<feature-name>-user-flow.md
   There is no User Flow Review command or file.

7. Run:
   /screen-specification
   Tạo screen spec từ ux/user-flows/<feature-name>-user-flow.md

8. Manually review and approve:
   ux/screen-specifications/<feature-name>-screen-spec.md

9. Run:
   /stitch-prompt
   Tạo stitch prompt từ ux/screen-specifications/<feature-name>-screen-spec.md

10. Review:
    ux/stitch-prompts/<feature-name>-stitch-prompt.md

11. Copy its final “Stitch Prompt” section into Google Stitch manually.
```

Do not run a separate Requirement Analysis command, MVP Scope command, User Flow Review command, API Contract command, or implementation command; none exists in this repository.

## If the selected UC is one of the ten existing UCs

1. Select the UC by both ID and name, using the mapping in this bundle rather than filename alone.
2. Read its chain in order: combined analysis → MVP → User Flow → Screen Specification → Stitch Prompt.
3. Resolve or explicitly accept material open questions that affect visible UI. Do not resolve implementation-only questions just to generate a mockup.
4. For UC-01, first reconcile the stale MVP references and the obsolete claim that UC-04 Sign In does not exist.
5. For UC-02, first decide whether rejected-application resubmission belongs only to UC-03 or remains duplicated in UC-02.
6. For UC-09, confirm the actual option domains before treating the placeholder choices as final UI requirements.
7. For UC-10, confirm the required/optional constraint fields and whether the list-only itinerary result is acceptable for MVP.
8. For UC-05, decide whether the optional confirmation dialog is desired; without it, there is no dedicated UI screen to generate.
9. Once the selected chain is approved, use the existing `ux/stitch-prompts/<feature-name>-stitch-prompt.md` rather than regenerating it without cause.
10. Paste only the final Stitch Prompt section into Google Stitch and review the output externally.

## Highest-value repository cleanup before broad screen generation

Without changing business requirements, the most useful next documentation actions would be:

1. Decide whether Use Case Analysis and Requirement Analysis are intentionally one artifact or should become separate artifacts.
2. Define how raw Use Cases are preserved so `/usecase-analysis` cannot overwrite the only source.
3. Define a durable User Flow Review/approval marker if traceable approval matters.
4. Resolve the UC-02/UC-03 resubmission boundary.
5. Refresh UC-01’s stale path references and Sign In assumptions.
6. Add an explicit UC ID and UC name metadata convention to every feature chain without renaming existing files automatically.
7. Define API-contract tooling and naming only when that pipeline stage is ready to begin.

Until those decisions are made, another AI should preserve current files, avoid silent renames, avoid inventing missing commands, and clearly label any inferred UC numbering or UX behavior.
