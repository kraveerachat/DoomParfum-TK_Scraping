# Mandatory Markdown templates
Fill all fields; use NONE, NOT RUN, NOT AVAILABLE, NOT COMMITTED, UNKNOWN as truthful. All examples are templates, not actual test claims.

## A. Canonical task overview
```markdown
---
id: TASK-ID
project: DoomParfum-TK_Scraping
type: task
status: in-progress
updated: YYYY-MM-DD
requirements: []
planning_pr: null
implementation_pr: null
---
# TASK-ID — title
## Current Task
Type / topic / agent / class / branch / PR / base / start SHA
Approved plan PR and SHA / governance PR and SHA
Current state / last checkpoint / production and parent mutation NO
## Goal / In Scope / Out of Scope
## Dependencies / Do-not-touch
## Acceptance Criteria and Evidence Needed
## Task Map
| Subtask | Requirement | Planned test IDs | State | Evidence |
## Planned / Completed / Current / Remaining
## Session Register
| Session | Purpose | State | Result | Checkpoint SHA | Evidence |
## Test and Evidence Ledger
## Issues / Risks / Known Limitations
## Reports index
| Session | Report or Summary path | SHA-256 | PUBLIC_SAFE/SANITIZED |
## Latest Git Checkpoint / Handoff / Next Action
```

## B. Full session execution report
```markdown
# TASK-ID S<n> — execution report
Classification: PUBLIC_SAFE / SANITIZED
Date / timezone / agent / role / route class
## TOOL DECISION
Task, working path, requested tool, selected tools and reason, availability,
execution mode, Git/Obsidian write, production/parent NO
## Scope and initial state
Plan and governance PR/SHA, main/base, branch, starting HEAD, active PR
Requirements, subtask IDs, dependencies, do-not-touch, expected evidence
## Work performed
Ordered actions, actual implementation details, commands and exact paths
## Changes and decisions
Exact changed files, proposed vs Owner-verified choices
## Progress
Completed subtask IDs / current / remaining / deferred
## Tests and evidence
Test ID / exact command / expected / actual / pass fail skip / exit /
environment / SHA / evidence path or URL
Tests NOT RUN + reason
## Findings and fixes
Introduced / pre-existing / environmental / dependency / not tested
## Source rights, security, privacy
Permissions, negative controls, no production/parent mutation
## TOOL EXECUTION REPORT
Tools actually used, requested not used, installation/config changes,
versions, file paths, actual Git branch/base/head/PR state,
Obsidian paths and evidence identity
## Final state
Result PASS/PARTIAL/BLOCKED, task state, limitations, blockers, receipt status
Next recommended agent, exact next action, Owner action
```

## C. Short session summary (separate Markdown file)
```markdown
# TASK-ID S<n> — session summary
Date / Agent / Class / PUBLIC_SAFE or SANITIZED
- Goal:
- Completed subtask IDs:
- Current:
- Remaining:
- Files / scope:
- Tests / evidence / NOT RUN:
- Git branch, checkpoint SHA, PR URL/state:
- Blocker / limitations:
- Result and task state:
- Full report link:
- Next action / Owner action:
```

## D. Immutable receipt (one per accepted task)
```markdown
# TASK-ID — review readiness receipt
Task type/purpose, approved plan/governance PR/SHA,
branch/base/final reviewed SHA, PR URL/state at receipt,
scope/exact paths, decisions, tests/results/evidence, negative controls,
data rights/security/production NO, limitations/deferred, Obsidian refs,
review readiness and next Owner action.
```
Never create if acceptance evidence incomplete, never overwrite. Post-merge event goes in living canonical note.
## E. Evidence and review ledger
| Field | Meaning |
|---|---|
| Test ID | mapped requirement/subtask |
| Command / tool | exact invoked action |
| Environment/version | verified version or UNKNOWN |
| Result | pass/fail/skip/not run, count and exit |
| Evidence | actual path/URL and SHA |
| Limitation | what was not proven |
| Finding | severity, exact path/line, observed behavior, expected, fix/retest |
## F. Tool inventory / usage entry
Tool ID, purpose/type, actual availability, source URL, version/commit, license, install path/config, authorization scope, test, cost/permission implications, rollback, date, task/PR. No credentials or private paths published.
