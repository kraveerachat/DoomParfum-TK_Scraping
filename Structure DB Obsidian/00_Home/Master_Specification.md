# Tk-Web-Scraper — Master Specification v0.2
Date: 2026-11-25 | Task: TKS-PLAN-001 S2 | State: PROPOSED / Gate P pending
This is the normative proposed master plan for the independent DoomParfum-TK_Scraping repository. The S1 blueprint documents remain summaries; where additional precision is needed the detailed S2 specification controls once merged. Nothing here is evidence of implemented code or permission to scrape. Decisions marked proposed require Owner merge; later material changes require a planning amendment.

## 1. Product objective
Deliver a locally installed no-subscription-required visual browser extraction workbench with independent manual mode, agent-assisted mode (Codex and Claude Code), and optional LM Studio local AI. Build reusable rules, not a single hardcoded website crawler. Enable permissioned structured data acquisition and reviewed handoff to DoomParfum By TK; no direct coupling to parent database.
The first useful deliverable is a working visual Chrome/Brave extension with standalone data table. The end-to-end deliverable is a bounded, auditable list-to-detail pipeline that supports multiple page sections and exports a normalized package. Every stage must be testable independently without accessing a prohibited site.

## 2. Owner-confirmed intent and proposed choices
Confirmed from user: Chrome/Brave, visual mouse selection, list and multi-section detail extraction, separate data view, manual and agent workflows, local AI integration, independent companion repository, reuse of upstream tools, staged development and inherited Obsidian/PR workflow.
Proposed by planner: TypeScript monorepo, Node 24 LTS, MV3, Vite, HTML/CSS/TS, Fastify, SQLite, Zod/JSON Schema, Playwright library, custom MCP stdio, local provider adapter; dedicated runner profiles; original UI design; source policy default deny; versioned recipes and JSON-first results; conservative job limits. Exact dependency versions and SDK API must be verified in P1/P3 before installation. No paid service or Docker required.
Fragrantica screenshots are a reference for information shape, not an authorization grant. The previously reviewed terms restrict automated access/data uses; live extraction and restricted reuse remain NO-GO absent explicit written authorization. Build fictional fixtures and approved source adapters only. Manual user research and owned input are separate from extension automation.

## 3. How to read and review
| Review area | Canonical detailed specification |
|---|---|
| Functional product scope and requirement acceptance | [[Functional_Requirements_Detail]] |
| Human flows, screens and UI states | [[UX_Interaction_Specification]] |
| Runtime components, interfaces, platform lifecycle | [[Architecture_Implementation_Spec]] |
| Visual algorithm and extraction semantics | [[Picker_and_Extraction_Algorithms]] |
| Recipe schema, workflow DAG, versioning | [[Recipe_Schema_Specification]] |
| Local storage, job orchestration and HTTP API | [[Daemon_Storage_and_Job_Spec]] |
| MCP tools, real client integration and agent permissions | [[MCP_and_Agent_Integration_Spec]] |
| Local AI model and evaluation | [[Local_AI_Provider_Spec]] |
| Domain fields, data quality and parent staging | [[Perfume_Data_and_Parent_Handoff_Spec]] |
| Source policy, threat model and failure behavior | [[Security_and_Source_Policy_Spec]] |
| Tool selection and compatibility | [[Dependency_and_Platform_Plan]] |
| Phases and acceptance | [[Implementation_Master_Task_Map]] |
| Fixtures, traceability, release tests | [[Verification_and_Acceptance_Matrix]] |
| Installation, operational lifecycle, support | [[Release_and_Operations_Plan]] |
| Design decisions and review checkpoints | [[Architecture_Decisions_and_Open_Items]] |

## 4. End-to-end data flow
1. Owner records a verified source grant or selects an owned synthetic fixture. No grant means no extraction.
2. Manual user chooses List or Detail, enables picker on a permitted tab, selects records/fields, previews and corrects schema.
3. Extension extracts one page deterministically with no model or daemon; local table shows values, nulls, counts, provenance, and export options.
4. Owner saves a draft recipe. Validation tests on representative permitted page fixtures verify scope and expected fields.
5. For multi-page work, user explicitly pairs extension and daemon and approves the exact recipe revision, source grant and bounded job budget.
6. Agent may inspect an explicitly leased tab and suggest selection/recipe; alternatively LM Studio proposes the same typed contract. Neither can grant source rights or approve expanded scope.
7. Job engine validates policy, creates durable job, runs isolated Playwright context (or explicit tab workflow), tracks unique records and page progress, cancels or stops on limits, failures or challenges.
8. Normalizer preserves observed and missing values separately, reconciles candidate duplicates, stages uncertain records and records field-level evidence.
9. User reviews standalone data table, validation counts and quarantine, exports JSON/CSV and manifest with hashes. No automatic upload.
10. Parent adapter produces versioned import candidates. Parent team reviews live schema compatibility and source rights, dry-runs staging, owner approves separate parent change. Parent production remains untouched by TKS.
11. Operational lifecycle: retention, backups, deletion confirmation, recipe revision/retest, upgrade/rollback and audit. Scheduling is opt-in late phase only.

## 5. Separation of concerns
Manual extension does not depend on local daemon or model. Agent and local AI share policy and execution contracts but differ in planner/provider. Playwright MCP is optional interactive development tooling, not the production batch runner or the source authorization gate. External subscriptions are used through supported clients only; no proxying subscription credentials. All exported source material remains local unless explicitly approved.

## 6. Release increments
P0 docs only. P1a shell/picker; P1b list extraction and data view; P2 multiple independent detail fields and nested arrays; P3 daemon and authenticated bridge/MCP; P4a automation; P4b security review; P5 Codex and Claude integration; P6 LM Studio; P7 handoff; P8 optional advanced tools and release. Each is its own reviewable implementation task in [[Implementation_Master_Task_Map]]. All require Gate P and dependency merges; runtime not started.

## 7. Definition of a complete plan
- Every R01–R17 has detailed behaviors, exclusions, a task and verification reference.
- All interfaces, data shapes, state transitions and failure semantics are specified at design level.
- Every proposed implementation task has dependencies, ordered work, test evidence, acceptance and out-of-scope boundaries.
- Security and source rights are enforced at every mode, not relegated to final release.
- Dependencies and platform assumptions are explicitly unverified where applicable.
- Report and summary are immutable per session; task status and PR link reflect real remote state.
- Reviewer must verify actual code against the merged revision, not just rely on count of lines or a checklist claim.
No runtime test passes are claimed. Owner acceptance, compatibility tests, source grants and parent schema verification are distinct gates.

## 8. Change control
S1 baseline retained; S2 expands same task and PR (class A). No source implementation or source-specific selectors. On Gate P merge, freeze accepted plan SHA. If an agent discovers scope, security, API semantics, data shape or dependency decisions materially differ, stop that portion and open/reuse a docs-only amendment. Small in-scope implementation refinements documented with actual evidence. Owner alone merges; no auto-merge.
