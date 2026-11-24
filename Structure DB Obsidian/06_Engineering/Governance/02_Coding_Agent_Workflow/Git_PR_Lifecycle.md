# Git, task routing and PR lifecycle
Derived from parent DEVELOPMENT_WORKFLOW §§3, 5, 6. No repo administration action authorized by this proposal.

## Gate P
Planning PR is docs-only: requirements, design, task map, acceptance, risks, source policy, ADRs and Obsidian. Branch plan/<slug> from verified latest main. Draft for revisions; Owner decides readiness and merges. Record planning PR merged + approved plan SHA + accepted scope before assigning coding. PR #1 remains Draft on Owner instruction; PR #2 governance independent, also Draft. Do not code before both accepted and root instruction links reconciled.

## Gate I
Implementation branch feat/<slug>, fix/<slug>, test/<slug>, docs/<slug>, data/<slug> from updated main after verified dependency merge. New independently reviewable task = new Draft PR; continuation = same branch/PR. Stacked PR only explicit Owner approval with base/dependency recorded. No stacked work assumed. Check status before changing files and protect other uncommitted work.
Example sequence (commands to run only with actual local checkout and authority):
```bash
git fetch origin
git remote -v
git branch --show-current
git status --short
git rev-parse HEAD
git rev-parse origin/main
git diff --check
# inspect exact paths and diff before staging
git diff --cached --check
```
Inspect staged diff and secrets before commit; stage specific paths, not indiscriminate add. Never force push, blind clean/reset, silently rebase shared branch or auto-merge. Push coherent checkpoint not every chat line. Check branch/PR actual head after publishing; record exact SHA. If only connector access, local shell status/checks NOT RUN; verify remote paths/diff as available.
If main advances, inspect actual merge base and conflicts, preserve other work; follow approved non-destructive update policy. Do not silently merge/rebase across scope.

## PR description templates
Planning:
- Task ID, goal, proposed/accepted/deferred decisions, Rxx, scope/non-goals
- architecture/data/UX/security/source rights, task map and dependencies
- acceptance, open risks/questions, docs updated, verification, exact branch/base/head
- report/summary/task links, Owner review checklist
- IMPLEMENTATION_NOT_STARTED=YES, PRODUCTION_MUTATION_PERFORMED=NO
Implementation:
- task/subtask/requirement/test IDs, approved plan and governance PR/SHA, dependencies
- source/tests/docs changes and actual behavior; scope exclusions
- exact tests/commands/counts/exit/evidence, negative controls and NOT RUN
- security/data rights, migration/rollback, limitations/failures
- Obsidian task/session/report/summary/receipt links, branch/base/head
- review findings/status, owner action, PRODUCTION_MUTATION_PERFORMED=NO
Update existing PR body for meaningful changes in scope, evidence, limitations or head; do not duplicate PR. Never say merged because mergeable. Draft Ready status not automatic approval.

## Independent review and receipt
Reviewer checks exact SHA, changed paths, code semantics, tests genuinely asserting intended behavior, bypass/fail-open, security/data rights and docs claims. Findings BLOCKER/HIGH/MEDIUM/LOW/NO ISSUE, with evidence and remediation. Fix within scope on same branch/PR; material change needs planning amendment. Read-only reviewer does not modify repository/vault; separate authorized docs session reconciles. One receipt per reviewable task at acceptance readiness, not per session. Owner alone marks Ready/reviews/merges in this project proposal. Post-merge reconcile actual merge SHA and remaining work; merge not deployment.
## Agent publishing identity and protection
Normal agent publication uses a dedicated scoped identity distinct from Owner (prefer GitHub App restricted to this repo; alternative dedicated machine user with repo Write). Minimum Contents read/write, PR read/write, Metadata read. No Admin, Actions/Workflows, Secrets, Environments, Pages or account-wide permission unless separate purpose and approval. Token/key expires, stored outside repo, no logs. Availability is not authorization. Owner-login exception requires explicit bounded Owner authorization for exact task/PR, expires at task boundary, never authorizes merge. Existing connector permission is not proof of dedicated identity or branch protection.
Owner configures main rules: PR required, force push/deletion blocked, update/merge restricted to Owner, agent not bypass actor, auto-merge disabled. Verify using actual settings and authorized test branch/Draft PR, not attempted main merge. If unreadable mark NOT VERIFIED. No ruleset changes in docs-only task. Record identity/scope/expiry verification in tool registry without secrets. Blocked writes: keep local checkpoint if possible; report LOCAL_ONLY/NOT_PUSHED/PR_NOT_CREATED, AGENT_CREDENTIAL_REQUIRED where applicable. Owner may publish as explicit exception; do not make Owner routine branch/push operator.
