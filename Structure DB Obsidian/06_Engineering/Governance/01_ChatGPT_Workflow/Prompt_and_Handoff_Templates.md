# ChatGPT prompt and handoff templates
Fill from verified evidence; placeholders are not facts. Use English or Thai as appropriate. Never embed secrets. Every actionable handoff includes tool recommendation, mode, permissions and explicit stop conditions.

## A. Tool recommendation (required before agent assignment)
```text
TOOL RECOMMENDATION
TASK_ID / TOPIC_ID / SESSION_ID:
AGENT: CODEX / CLAUDE CODE / CHATGPT / DOCS / REVIEW
MODE: PLANNING_ONLY / IMPLEMENTATION / DOCS_ONLY / READ_ONLY
WHY:
ROUTE_CLASS: A/B/C/D
REPO: kraveerachat/DoomParfum-TK_Scraping
APPROVED_PLAN_PR / SHA:
GOVERNANCE_PR / SHA:
GIT_WRITE_ALLOWED: YES/NO
OBSIDIAN_WRITE_ALLOWED: YES/NO
PRODUCTION_MUTATION_ALLOWED: NO
PARENT_WRITE_ALLOWED: NO
HUMAN_MERGE_REQUIRED: YES
```

## B. Planning / planning revision prompt
```text
Read GOVERNANCE.md, root instructions, Master Specification, current task
and Obsidian status, actual main/head and PR state. Verify permissions.
TASK / OBJECTIVE / REQUIREMENT IDs / ACCEPTANCE:
MODE=PLANNING_ONLY; RUNTIME_SOURCE_EDIT=NO; MERGE=NO.
Class A: update existing task, branch and Draft PR.
Class B: new distinct plan from verified main.
Class C: material change to accepted scope, docs-only amendment.
Produce requirements, decisions, dependencies, numbered subtasks, contracts,
security/source rights, tests, unknowns and owner review criteria.
Update canonical task, report, summary, relevant notes/logs.
Validate docs/diff/secrets; authorized publish Draft; verify actual state.
Report completed/remaining, exact changed paths, SHA, tests NOT RUN if so,
blockers and Owner review action. Never start runtime implementation.
```

## C. Coding assignment template (only after verified gates)
```text
ROLE: <Codex or Claude Code>; TASK: <TKS-IMP-ID and title>
APPROVED_PLAN_PR/SHA: <verified merge>; GOVERNANCE_PR/SHA: <verified merge>
ROOT_GOVERNANCE_RECONCILED: <verified evidence>
BASE_MAIN_SHA: <verified>; PR_ROUTE: <A/B>; ACTIVE_PR: <number or NONE>
REQUIREMENTS: <Rxx>; SUBTASKS: <IDs>; TESTS: <IDs>
IN_SCOPE: <exact paths and behaviors>
OUT_OF_SCOPE / DO_NOT_TOUCH: <list>
DEPENDENCIES: <verified task merges and permissions>
SOURCE_POLICY: owned fixtures/explicit allowed scope only
GIT_WRITE / OBSIDIAN_WRITE: <granted>; PRODUCTION/PARENT/MERGE: NO
Read root instructions, GOVERNANCE.md, detailed coding AGENTS, accepted
master plan and relevant specs; verify branch/worktree and current PR.
STOP if gate, source rights, dependency, credentials or scope unverified.
Record TOOL DECISION and canonical task IN PROGRESS.
Implement ordered subtasks with TDD where meaningful; run exact applicable
tests, lint/type/build, regression and negative controls; no fake results.
At session end, write full report + summary in Obsidian and update same
canonical task, status/logs. Commit coherent checkpoint, push and create
or update same Draft PR if authorized. Independent review exact HEAD.
Report done/current/remaining by subtask, actual paths, tests and failures,
Obsidian refs/hashes, branch/PR/head, limitations and next exact action.
No merge, force push, live unauthorized scraping or parent/production edit.
```

## D. Correction / review prompt
For in-scope correction, same task/branch/PR: specify finding IDs, severity, exact reviewed SHA, path/line, expected behavior, focused regression, out-of-scope exclusions, required new evidence. Do not start another PR for a new review revision.
For read-only review: verify exact SHA, actual diff/source/test semantics and source rights; classify BLOCKER/HIGH/MEDIUM/LOW/NO ISSUE; report evidence, remaining unknowns and targeted correction instructions. No repository or vault write if read-only; separately authorized docs session reconciles immutable review evidence. No approval or merge claim.

## E. Continuity pack (vault Handoffs)
```markdown
# TKS Continuity Pack
- Identity: project, task/topic/session, agent, timestamp
- Requirements and accepted decisions / proposed decisions
- Git truth: repo, main/base, branch, HEAD, actual PR URLs and states
- Plan/governance gates: approved PRs and merge SHAs, dependencies
- Task map: done/current/remaining with subtask IDs
- Actual files/changes/checkpoint, test commands/results and not run
- Evidence, failure classification, open findings/risks
- Obsidian task, report and summary paths/hashes, receipt if applicable
- Source rights/privacy/security, production and parent state
- Tool availability/config (safe), limitations
- Next agent, exact action, abort conditions, Owner action
- Paste-ready next prompt (bounded task)
```
Quality gate: next agent can resume without old chat, knows what is approved and what remains. Chat history alone is not source of truth. Store public-safe only.
