---
id: TKS-GOV-AGENT-001
project: DoomParfum-TK_Scraping
type: agent_instructions
status: proposed
updated: 2026-11-25
---
# Coding Agent instructions — Tk-Web-Scraper
Applies to Codex, Claude Code, other coding/testing/documentation agents operating on this repository once Owner approves this governance. This is the detailed policy; root AGENTS.md and DEVELOPMENT_WORKFLOW.md must link to it after both draft gates. Do not confuse this with instructions for DoomParfum-By-TK.

## 0. Operating contract
Repository: kraveerachat/DoomParfum-TK_Scraping. Use actual checked-out root, no assumed absolute path. Vault: Structure DB Obsidian/. Parent is read-only for this initiative. Independent local visual and agent-assisted scraper, no direct parent database writes, no mandatory paid scraping service. Source grants independent; no unauthorized live extraction, CAPTCHA bypass, secret/session disclosure, or fabricated facts.

## 1. Mandatory read and preflight
Read root AGENTS.md, DEVELOPMENT_WORKFLOW.md, GOVERNANCE.md, README.md, vault README/Project_Status/Index, accepted Master Specification, task map, traceability/test matrix, relevant UX/API/MCP/data/security specs and canonical task/receipts. Verify actual Git state: origin, visibility, worktree, branch, HEAD, base, open/Draft/merged PRs, tool permissions. Missing docs NOT FOUND. No runtime implementation from unmerged master draft. Verify both product plan and governance accepted, root pointers reconciled, predecessor merges and scope. If not, stop feature work and report BLOCKED. No direct edits to parent.
Code and actual test evidence establish implemented behavior; approved docs define intended scope. Conflicts flag, never quietly rewrite acceptance. Follow latest verified source.

## 2. Task intake and routing
A same task: reuse task ID, branch, PR, append session. B separate reviewable task: allocate ID, check dependencies, branch from current main, new Draft PR. C material scope/architecture/data/security change: stop affected work and request docs-only amendment before coding. D read-only review: no changes. Never duplicate PR for new session, new agent, test rerun or in-scope findings. Never combine independent tasks.
One task -> one branch -> one PR -> one immutable final receipt; sessions S1/S2/... share task branch. Use Task IDs from master plan; internal subtask IDs not separate PRs unless approved split. No simultaneous edits by multiple agents without explicit ownership/handoff.

## 3. Tool transparency and safe setup
Print TOOL DECISION before meaningful action where practical:
```text
TASK / WORKING_PATH / REQUESTED_TOOL / SELECTED_TOOLS (reason)
TOOL_STATUS: AVAILABLE / NOT INSTALLED / NEEDS SETUP / NOT AVAILABLE
EXECUTION_MODE: EXISTING / PROJECT-LOCAL INSTALL / MCP CONNECTION / MANUAL FALLBACK / NO TOOL
GIT_WRITE / OBSIDIAN_WRITE / PRODUCTION_MUTATION=NO
ROUTE_CLASS / PLAN_SHA / GOVERNANCE_SHA
```
Inventory current native tools, connected MCP, CLI/skills and project-local versions. Choose smallest effective set. A GitHub repo is not installed/connected. For explicit slash request verify actual harness supports it; otherwise report unavailable and fallback, do not claim it ran. Tool switch record reason. If external dependency missing, read official README/license/prerequisites/install hooks/permissions/cost/version and data flow. Project-local isolated checkout only in ignored .agent-tools/external when authorized; no blanket installs, remote scripts, global settings/hooks, credentials or fees without separate approval. Verify command/skill/MCP by safe smoke, record version/path/test/rollback. No secret values in logs. TKS tool registry follows Master Plan dependencies; parent tool catalog is inspiration, not automatic authorization.

## 4. Implementation lifecycle
- Verify merged Gate P and governance, exact main/base and predecessor evidence. Open or reuse correct branch, canonical task and session IN PROGRESS before meaningful edits.
- State ordered subtasks, exact planned files, acceptance, tests, risks, scope exclusions. Implement minimal approved change. Use RED -> minimal implementation -> GREEN -> focused -> related/full regression where meaningful. Do not fabricate RED or test pass.
- Run actual lint/type/build, unit/integration/e2e, fixture/security negative tests applicable to task. Record exact command, pass/fail/skip, exit, environment, SHA/evidence. If unavailable NOT RUN with reason. Classify failures introduced/pre-existing/environmental/dependency/not tested.
- Keep code and docs synchronized in same checkpoint; update affected specifications and requirements only if actual approved change. If material change, halt and request amendment.
- At every meaningful session end create immutable full report, separate summary, update canonical task overview and logs in vault. Include completed/current/remaining subtask IDs; see Obsidian_Protocol.md.
- Validate diff and secret/privacy/source rights, stage exact intended paths, commit coherent checkpoint and push with authorized identity. Open/update Draft PR; verify remote SHA and actual state; no local-only claim as remote.
- Request independent review of exact SHA, fix verified in-scope defects on same PR, retest/re-review. Review evidence not owner gate approval. Once acceptance ready, one immutable receipt. Owner marks ready/reviews/merges; no auto-merge or agent merge. Post-merge status updates are traceable and do not edit old receipts.

## 5. Git and publication
Use Git_PR_Lifecycle.md. No main push, force push, blind reset/clean, silent shared rebase, auto-merge, unauthorized production or branch protection bypass. Check actual remote and ruleset permissions; agent dedicated identity, least privilege and scoped authorized credential required normally. One-time Owner authorization is bounded exception, not reusable standing permission. If blocked report AGENT_CREDENTIAL_REQUIRED / LOCAL_ONLY / NOT_PUSHED / PR_NOT_CREATED as applicable. Do not request credential value in chat or commit it.

## 6. Security, data and parent
Follow current approved source registry; permission to use tool is not permission to access source. Fragrantica remains NO-GO absent explicit written grant; owned synthetic fixtures only until approved live source gate. No site-specific prohibited selectors or bypass. Least privilege, deterministic extraction, field provenance and validation, no fabricated values. No direct DB or parent repo changes. Production/destructive operation, secrets, paid calls, global configuration require separate Owner approval. No public raw data, private paths, cookies, tokens, security exploit detail or account info. Sensitive evidence follows separately approved private vault policy; otherwise stop sensitive part and report PRIVATE_RETENTION_NOT_CONFIGURED.

## 7. Final report and handoff (every meaningful session)
Report TOOL EXECUTION REPORT with actual repo/path, tools used, requested not used, install/config changes, versions/refs, exact changed paths, Obsidian files, branch/PR/base/head, checks/evidence, blockers and next action. Also explicitly state major topics done/open, subtask status, test results/not run, source rights, production NO, parent write NO, receipt status, next recommended agent and owner action.
Full Markdown report + short summary + canonical task update are mandatory for meaningful writable sessions, including blocked or partial work. Read-only reviewers exception: no vault writes; authorized reconciliation later. No duplicate Downloads report. See Session_Record_Templates.md.
