# TKS PR workstream index — proposed governance
Snapshot intake 2026-11-25. Verify live state before any action.
| Task | PR | Branch | Base/dependency | State | Next |
|---|---|---|---|---|---|
| TKS-PLAN-001 | [#1](https://github.com/kraveerachat/DoomParfum-TK_Scraping/pull/1) | plan/tks-001-system-blueprint | main 40166f338ed3cf5c198a8e9add1217633812ab6b | OPEN DRAFT, head 317e7b425f691d067c3b8aea60ef900611146a9a at intake | Owner keeps draft; revise when requested |
| TKS-GOV-001 | [PR #2](https://github.com/kraveerachat/DoomParfum-TK_Scraping/pull/2) | plan/tks-gov-001-agent-workflow | main 40166f338ed3cf5c198a8e9add1217633812ab6b | OPEN DRAFT; initial head 49bcd977492d8cab2ec725056a84a5bf50d26fae | Owner review governance |
| TKS-IMP-001 | NOT CREATED | feat/tks-imp-001 proposed | accepted plan + governance + root integration | NOT STARTED | blocked on both gates |
Both planning PRs intentionally independent and docs-only. No implementation on either. Root AGENTS/workflow and common vault files belong to PR #1 draft; governance root pointer and two folder paths belong to PR #2. After both accepted, reconcile links/status/logs in authorized docs checkpoint before coding. If #2 merged first, update #1 in its existing task when Owner authorizes; if #1 merged first, separate docs-only reconciliation from latest main after #2 merge. Owner alone merges, no direct main writes. PR index is living, update from verified remote state.

Publication verified; final HEAD in live PR. PR #1 unchanged; disjoint changed paths verified.

## TKS-PLAN-002 S1 / 2026-11-28 — verified successor workstream

Fresh readback: PR #1 merged at `ca385cb07d64df2e71303df7a003990d4d7bee0f`; PR #2 merged at
`ffb9468dad7e383232fa7144e9cbfdbde58835ac`. Earlier OPEN/DRAFT rows are historical snapshots.

| Task | PR | Branch | Base/dependency | State | Next |
|---|---|---|---|---|---|
| TKS-PLAN-002 | NOT CREATED | plan/tks-plan-002-chrome-generic-compatibility | main `ca385cb07d64df2e71303df7a003990d4d7bee0f`; independent of IMP-001 acceptance | Local preparation complete; publication BLOCKED | Resolve identity/protection; separate publishing authorization and Owner amendment review/merge |
| TKS-IMP-001 | No PR mutation in this task | feat/tks-imp-001 | Preserved checkpoint `901463aa11f2ac33f0339547927e85f672006845` | Unchanged; native acceptance pending | Separate history-preserving amendment integration authorization after Class C merge |

Exact 25 normative plus 11 supporting paths: [[2026-11-28_TKS-PLAN-002_S1_Report]]. No implementation commits copied,
push, PR, merge or settings change. Proposed successor ordering is non-circular; IMP-002 remains unauthorized.
