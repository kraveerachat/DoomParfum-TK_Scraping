---
id: TKS-PLAN-002
project: DoomParfum-TK_Scraping
type: class-c-planning-amendment
status: local-preparation-complete-publication-blocked
classification: PUBLIC_SAFE
updated: 2026-11-28
requirements: [R01.01, R01.05, R02.06, R02.07, R02.08, R03.05, R06.04, R13.06, R13.07, R13.08, R16.04]
planning_pr: null
implementation_authorized: false
publication_authorized: false
---
# TKS-PLAN-002 — Chrome-only and generic compatibility amendment

## Current Task

Codex / Class C / LOCAL_ONLY / S1 / 2026-11-28 Asia/Bangkok.
Branch `plan/tks-plan-002-chrome-generic-compatibility`; separate repository-relative worktree `.runtime/worktrees/tks-plan-002`.
Base and starting HEAD `ca385cb07d64df2e71303df7a003990d4d7bee0f`; no PR. Owner merged planning PR #1 at this base and
governance PR #2 at `ffb9468dad7e383232fa7144e9cbfdbde58835ac`.
Owner authorization covers local amendment preparation and commit only. Current result:
local preparation complete; publication BLOCKED; amendment approval PENDING.
Production/parent/source-grant/implementation authorization NO.

## Goal / In Scope / Out of Scope

Apply exact independently reviewed S14 patch SHA-256 `d6c192a0849923592046321d3889966cf99db35b723129dfcf4d698a6d17d42a` from main.
Keep all 25 normative after-contents byte-for-byte; create only four necessary supporting
records and update required existing governance/status/index/log files. Prepare future Draft PR
information locally. No implementation commits or historical proposal files imported.
No runtime edit, Brave investigation, installation, new independent review, real-source access,
GitHub mutation, source grant, acceptance receipt, IMP-002 start or backup deletion.

## Dependencies / Do-not-touch

Original clean implementation checkout `feat/tks-imp-001` at `901463aa11f2ac33f0339547927e85f672006845` remains unchanged.
Reviewed runtime `1496b73cf063806c4d728bac1fa696f9feb46c53` and seven production build
hashes preserved. Main-based amendment is independent of unfinished IMP-001 acceptance.
Future order: Class C review/Owner approval/merge; separately authorized history-preserving
IMP-001 integration; remaining Chrome native acceptance; Owner IMP-001 acceptance/merge;
separate IMP-002 authorization. No full006 prerequisite cycle or live authorization inferred.

## Acceptance Criteria and Evidence Needed

Preparation checks are complete in [[2026-11-28_TKS-PLAN-002_S1_Report]]. Proposed registry: 10 major tasks,
75 detailed requirements, 91 subtasks, 210 cases, IMP-002 13 and IMP-003 11 subtasks.
All 28 new cases NOT RUN. EXT-02 historical/proposed OUT OF SCOPE, never PASS.
Chrome Windows sole mandatory target only after amendment merge. Deny-by-default,
Fragrantica NO-GO, operation-specific authorization, protected Fixture v2/pilot configuration,
explicit Owner security packet and full006 gate remain mandatory. No actual grant issued.

## Task Map

| Checkpoint | Scope | State | Evidence |
|---|---|---|---|
| Preflight | Repository/main/patch/reviewer provenance | DONE | Report and preserved final review |
| Independent branch | Verified-main worktree, exact 25 normative paths | DONE | Branch ancestry and byte hashes |
| Validation | Counts, IDs, mappings, dependencies, links/anchors, stage/privacy | DONE | Report validation ledger |
| Local checkpoint | Required records, explicit staging and coherent local commit | DONE | Containing commit / final handoff |
| Publication | Scoped identity, protection, explicit authorization | BLOCKED | Read-only governance evidence |
| Owner amendment decision | Independent actual-commit review, Owner approval/merge | PENDING | No PR created |

## Planned / Completed / Current / Remaining

Completed local preparation and provenance capture. Final S14 review F1/F2/F3 resolved;
Chrome-only/LOW-5/standalone/Class C PASS; mandatory planning findings remaining 0.
No repeat review. Current: preserve local checkpoint. Remaining: publication authorization,
identity/protection gates, actual-commit review and Owner amendment decision. IMP-001 native
acceptance remains separate; accepted D02/D03 deferrals and genuine Owner observations retained.

## Session Register

| Session | Purpose | State | Result | Checkpoint | Evidence |
|---|---|---|---|---|---|
| S1 | Exact S14 Class C local preparation | COMPLETE | LOCAL_ONLY / publication BLOCKED | Commit containing S1 records | [[2026-11-28_TKS-PLAN-002_S1_Report]] / [[2026-11-28_TKS-PLAN-002_S1_Summary]] |

## Test and Evidence Ledger

25 normative after hashes match, 75 requirement/traceability sets match, 91 unique subtask IDs,
210 unique case IDs, 28 new NOT RUN, 44 explicit dependency edges acyclic, local links and four
normative anchors resolve. Explicit 36-path staging, whitespace and privacy scans in report.
182 implementation tracked file hashes and seven build hashes preserved; original checkout clean.
Runtime/browser/security execution NOT RUN in this docs-only task; no claims of native completion.

## Issues / Risks / Known Limitations

Publication BLOCKED by explicit local-only instruction and unmet normal publishing gates:
Owner `kraveerachat` is authenticated in connector/CLI, dedicated scoped identity or bounded
publishing exception not established; main unprotected, disabled/incomplete ruleset with admin
always-bypass. Auto-merge false. No credentials/settings changed. Private raw review retained
only in existing local reviewer history; sanitized copy and exact original hash in provenance.

## Reports index

| Session | Record | SHA-256 | Classification |
|---|---|---|---|
| S1 | [[2026-11-28_TKS-PLAN-002_S1_Report]] | `043008e3f0b7ebb0b92e3a77b52a1f3614e7356974cad205d32fd8045fca03bc` | SANITIZED |
| S1 | [[2026-11-28_TKS-PLAN-002_S1_Summary]] | `f20dba0003251dfaffef3296c35f5e1e10385af37d808df28b83d9c9503d5668` | PUBLIC_SAFE |
| S1 | [[TKS_PLAN_002_S1_Review_Provenance]] | Original review `4600cd573ffa5e0600f2a0acf6845386d42b69a79ca4334d6e85ea5b3d983cf6`; sanitized text `90661a6d9ad2c4ed575a8ee397d69868490f2be449c59e61a6fa167334fc90ff` | SANITIZED |

## Latest Git Checkpoint / Handoff / Next Action

Local checkpoint is the commit containing S1 records; use `git log -1` on this planning branch
or the final handoff for its exact SHA. No PR URL; no push/PR/merge. Preserve implementation
checkpoint `901463aa11f2ac33f0339547927e85f672006845`. Owner resolves publishing gates and separately authorizes future publication,
then reviews and merges the amendment before any authorized IMP-001 integration or scope acceptance.
