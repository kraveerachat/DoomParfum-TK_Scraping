# Canonical Obsidian reporting protocol
Shared by ChatGPT and coding/docs agents. Adapted from parent Workflow §4.2–4.7 and §10; latest parent PR33 governance supersedes earlier dual Downloads reporting. All paths relative to TKS repository, never parent vault. One canonical task note, not duplicate notes per session.

## Vault structure
```text
Structure DB Obsidian/
  README.md
  00_Home/Project_Index.md, Project_Status.md
  01_Product/      requirements, scope, priorities
  03_Data/         recipe, source rights, data and handoff contracts
  05_UX/           visual workbench, screens and flows
  06_Engineering/  architecture, algorithms, API, MCP, AI, security
    Governance/
      01_ChatGPT_Workflow/
      02_Coding_Agent_Workflow/
  07_Delivery/
    Roadmap_and_Backlog.md, Implementation_Master_Task_Map.md
    Requirement_Traceability_Matrix.md, Verification_and_Acceptance_Matrix.md
    Tasks/<TASK_ID>_<slug>.md
    Planning_PRs/TKS_PR_Workstream_Index.md
  08_Operations/
    Decision_Log.md, Change_Log.md, Agent_Run_Log.md
    Agent_Tool_Registry.md, Tool_Usage_Log.md
    Reports/<TOPIC_ID>/
    Summaries/<TOPIC_ID>/
    Receipts/
    Handoffs/
  09_References/   external tools and sources
  90_Templates/   reusable Markdown templates (as needed)
```
Paths marked not yet present are planned, not actual. Do not create irrelevant domain/matching/cart sections copied from parent. Product master docs are on PR #1; PR #2 adds governance paths and unique records. Shared Home/log links reconcile after both merges. YAML frontmatter for new primary notes: id, project, type, status, updated, tags/source refs as applicable. Wiki links and relative Markdown links; Mermaid where useful, verify renderer if available. No personal .obsidian workspace config or raw sensitive data.

## What is meaningful
A completed work assignment that performs or attempts planning, architecture, code, test, documentation, source rights/ETL, dependency/tool setup, config, review/remediation, bug fix, handoff, or acceptance checkpoint. Even partial/blocked meaningful writable sessions are recorded; no report per casual chat, trivial tool call or commit. One work prompt may have several coherent checkpoints, but session IDs track reviewable work, not each command.
At start update existing canonical task Current Task + Session Register IN PROGRESS with scope, IDs, starting SHA, plan, branch/PR, expected evidence, risks and do-not-touch. At end record actual changes/results/limits, done/current/remaining and next action. Never silently promote status.

## Required three records per meaningful writable session
| Record | Exact path | Required contents |
|---|---|---|
| Full report | 08_Operations/Reports/<TOPIC_ID>/YYYY-MM-DD_<TASK_ID>_S<n>[_r<rev>]_Report.md | complete work, tools, exact paths, decisions, tests and not run, errors, scope, Git and evidence |
| Short summary | 08_Operations/Summaries/<TOPIC_ID>/YYYY-MM-DD_<TASK_ID>_S<n>[_r<rev>]_Summary.md | concise done/current/remaining, result, SHA/PR, tests, blocker, next action |
| Canonical task | 07_Delivery/Tasks/<TASK_ID>_<slug>.md | current overview, session register, completed/current/remaining subtask IDs, test ledger, report/summary index |
TOPIC_ID major workstream; reuse existing folder. TASK_ID distinct review unit, S<n> increment within task. No new task note for new session. UTC timestamps for evidence, date in Asia/Bangkok filenames where relevant. Report/summary immutable once committed; corrections append a new record with correction reference. SHA-256 exact bytes, public-safe or sanitized classification, index in task note. Same checkpoint as code/docs where possible; own commit hash cannot be included in its own content, use next session or verified PR body. Never edit history to insert it.

## Update rules
Always: three records and Agent_Run_Log / Change_Log dated entry. Decision_Log when decision proposed/verified/changed; affected product/UX/data/engineering notes when approved scope changes; PR index on new PR/dependency; Project_Status on current truth change; Project_Index on new canonical note; Tool registry/log on meaningful tool use/setup/version/permission changes; Handoffs when changing agent/chat or ending major topic. Responsible agent ensures docs current even if another docs agent assists.
When docs stale, task is not CLOSED. Each session reports what was actually done and what remains, not entire master plan repeated. Use IDs to tie work to 65 requirement sub-IDs, 83 planned subtasks and 182 planned cases from PR #1 draft snapshot; verify current accepted revision later.

## State vocabulary
NOT STARTED, PLANNED, IN PROGRESS, BLOCKED, SOURCE IMPLEMENTED, LOCAL VERIFIED, ACCEPTANCE PENDING, READY FOR REVIEW, MERGED, CLOSED. Distinguish session result PASS/PARTIAL/BLOCKED from task state. PASS only for actually verified scope; READY not MERGED; MERGED not DEPLOYED. Tests NOT RUN or NOT AVAILABLE with reasons. Failure type introduced/pre-existing/environmental/dependency/not tested. No invented counts.

## Sensitive and read-only exceptions
Public-safe full report in vault only, no persistent Downloads copy. Sensitive original/raw review/secret/host details only in separately Owner-approved private vault outside every repo; canonical sanitized report references original SHA-256 and PRIVATE_EVIDENCE_NOT_PUBLISHED. No private path/custody details in Git; if private vault not configured, report PRIVATE_RETENTION_NOT_CONFIGURED and stop sensitive recording, publish safe summary only. Matching hash proves content identity, not independent custody. Read-only independent reviewer writes no repository/vault record; produces authorized separate report with hash, revision, findings, verification and provenance limitations; later authorized docs session reconciles without modifying original. Never turn review into approval.
## Receipt
One immutable receipt at acceptance readiness per task/PR, not per prompt/session. Filename 08_Operations/Receipts/YYYY-MM-DD_HHMMSS_<TASK_ID>_<slug>.md. Record reviewed SHA, scope, plan, files, tests, rights, risks, docs, owner action. If blocked/acceptance pending no final receipt. Actual merge event recorded separately; old receipt unchanged.
