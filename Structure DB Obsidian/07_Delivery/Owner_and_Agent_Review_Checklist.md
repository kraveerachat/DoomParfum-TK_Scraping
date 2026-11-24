# Owner and agent review checklist — master plan gate
Use exact merged plan SHA after Owner approval. This is a review checklist, not evidence of completed checks.

## Owner Gate P (this Draft PR)
- [ ] Product modes and screenshots-derived interactions represented (manual, external agent, local AI).
- [ ] List and detail extraction both first-class; multi-section/nested arrays supported.
- [ ] Independent repo and parent staging-only interface accepted.
- [ ] Architecture choices and deferred compatibility items accepted.
- [ ] Recipe/API/MCP/data contracts, job lifecycle, limits and approval boundaries reviewed.
- [ ] Source rights policy accepted; Fragrantica remains blocked without written grant.
- [ ] All tasks P1–P8, dependencies, subtask evidence and test suite reviewed.
- [ ] P8 optional scope and no paid mandatory dependency accepted.
- [ ] Obsidian/session/PR workflow accepted, no runtime before Gate P.
- [ ] No unresolved material plan conflict; owner merges personally.
## Every implementation PR reviewer
- Verify actual base/current head, merged plan and predecessor gate; compare exact diff and task scope.
- Verify Rxx/subtask/test mapping; inspect source behavior, not merely agent summary.
- Read actual tests and run evidence; distinguish RED/GREEN, regression, not run and skipped; verify negative control actually tests boundary.
- Check security/permissions, source rights, sensitive logs, dependency versions and no external unapproved network.
- Check docs/code and report/summary/task index consistency, known limitations and rollback.
- Classify BLOCKER/HIGH/MEDIUM/LOW with path/line/evidence; targeted correction same PR if in scope; amendment if material change.
- Reviewer must be independent of author for security acceptance. Owner alone merges; no auto-merge.
## Agent kickoff handoff template
TASK_ID / TOPIC_ID / SESSION_ID / AGENT / ROUTE_CLASS
APPROVED_PLAN_PR and SHA / BASE_SHA / BRANCH / existing PR
GOAL / REQUIREMENTS / SUBTASKS / IN_SCOPE / OUT_OF_SCOPE / DEPENDENCIES
TOOL_DECISION / requested and actually available tools / installation permissions
SOURCE_POLICY / PRODUCTION_MUTATION=NO / PARENT_WRITE=NO / MERGE_ALLOWED=NO
TEST_IDS / expected evidence / exact commands after implementation
OBSIDIAN task note / report / summary / logs / immutable receipt when ready
FINAL: actual changed paths, tests counts/failures/not run, source rights, PR URL/state/head, blockers, next action.
If plan not merged or permissions/dependencies missing STOP affected implementation and report BLOCKED. No fake SHA, tests or PR.

## First implementation instruction (not authorization to start now)
After verifying PR #1 is Owner-merged: Codex TKS-IMP-001, P1a. Read master and linked detailed docs, then exact task subtasks 001.01–001.09 and EXT/ALG/SEC cases. Build workspace and owned fixture server, minimal MV3 shell/picker only. Do not implement list export, detail, daemon, AI or live scraping yet. Verify Chrome and Brave actual support, exact 20-card selection, safe cancellation and source denial. Publish authorized Draft implementation PR, full report and separate summary, update task overview, request independent review. Owner merges. If Gate P not verified, do not code.
