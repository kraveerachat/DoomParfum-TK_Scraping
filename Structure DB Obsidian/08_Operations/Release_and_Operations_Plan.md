# Local installation, operation and release plan
Proposed documentation and operational acceptance; commands below are design targets, not existing scripts. No deployment or install in planning. Scope Windows first, Google Chrome on Windows; local-only; owner manually chooses to start each component.

## Installation sequence
1. Verify supported Windows, Node/npm and browser versions; check disk space and dependency versions; clone only TKS repo. Do not overwrite parent folder or existing browser profiles.
2. Check approved plan/release tag and lockfile; inspect install hooks, run documented project-local npm ci only after authorization. Build extension locally; no remote JS and no mandatory account.
3. In Google Chrome at chrome://extensions, enable Developer mode, Load unpacked built extension folder; record actual path and browser version. Pin toolbar action. Grant only requested narrow permissions. If unsupported sidebar API, use documented standalone fallback subject to acceptance.
4. Manual smoke on owned fixture: launch local fixture server, explicit fixture grant, select list, preview, extract, open standalone table, export JSON/CSV; verify no daemon/model necessary. Detail smoke after P2.
5. Optional daemon: run documented local start command (proposed npm run daemon:start), bind 127.0.0.1, choose local runtime directory and port, verify readiness. Pair extension through explicit owner consent. Do not expose port to network.
6. Optional MCP: register project-scoped stdio adapter in each supported client using actual tested instructions. Start with synthetic fixture; check tool list and pairing. No global config changes or subscription credential copying.
7. Optional local AI: start LM Studio local server, choose model, configure loopback endpoint, run capability check and fixture proposal. Manual fallback always available.
8. Export review: inspect counts, warnings, source rights, manifest/hash. Parent staging separate; no automatic upload or import.

## Day-to-day operation
Manual user chooses tool -> permitted page -> selection/fields -> preview -> extract -> data table -> export. Agent user explicitly leases permitted tab, requests recipe proposal, reviews and approves bounded job -> status -> result/export. Local AI identical approval path. Job progress displays actual counters; cancel/revoke available. Source grant changes stop further actions and require review. No ordinary account browser attachment by default.
Start/stop daemon only when needed. Close runner contexts gracefully; no unrelated browser process termination. Local schedule opt-in P8, requires daemon running and valid approval at run time; missed schedule reported, not replayed automatically. No service at system startup without explicit owner decision.

## Local data ownership
Extension IndexedDB holds manual recipes/datasets; daemon SQLite holds imported/saved recipes/jobs/checkpoints and local outputs. No automatic sync. User may export and explicitly import with manifest. Retention policy per source and local settings, configurable after review; no silent indefinite retention. Delete confirmation and status; source revocation review retention requirements. Output, runtime, browser profile, .env and caches ignored. Never commit live data or tokens to public repository. Back up only permitted local data to owner-approved location; encrypt sensitive backups if required, no claim private storage configured.
## Backup, upgrade and rollback
- Before upgrade: record exact installed version, schema/recipe version, release SHA, backup of allowed local DB/recipes and export manifest; stop jobs and daemon, close extension if necessary.
- Verify release notes, lockfile and migration instructions; build new version from reviewed tag/commit. Reload unpacked extension; pair again if permission model changed.
- Migration transaction with backup and integrity check. Unsupported downgrade refuses safely; restore supported prior backup rather than overwrite data. Test with fixtures first.
- Re-run manual list/detail, daemon/MCP and model smoke for installed modes. If failure, stop affected component, restore approved previous version and compatible DB backup, record data loss risk and actual result.
- Uninstall extension removes browser extension storage according to browser behavior; export/backup before remove. Unregister project-local MCP config only with authorization. Stop daemon, remove specifically confirmed local runtime folder only; never delete parent repo or personal browser profile.
## Troubleshooting
| Symptom | Diagnose | Safe action |
|---|---|---|
| Side panel unavailable | actual browser/API version | standalone fallback if supported; report unsupported |
| Picker not injected | active tab/permission/source policy | choose approved tab, grant explicitly; do not bypass |
| Wrong list | candidate scope/DOM drift | reselect narrower group, validate recipe |
| Missing field | scope/cardinality/actual DOM | correct rule or keep missing status |
| Daemon offline | process, port, readiness | start local daemon or manual mode |
| Pairing expired | token/lease state | owner re-pairs; no token logging |
| MCP missing | actual client config/version | supported setup or manual prompt; no invented tool |
| Job paused | reason/approval/lease | review and explicitly resume if allowed |
| Challenge/rate limit | source response | stop and review rights/limits; no evasion |
| LM Studio invalid response | capability/context/model | bounded repair or manual fallback |
| Export invalid | manifest/rights/hash/quota | correct source or regenerate allowed export |
| Parent schema mismatch | latest accepted contract | update mapping plan; no direct DB write |
## Release checklist
- [ ] Accepted plan and implementation PRs traceable; exact release SHA.
- [ ] Actual install/upgrade/uninstall smoke for Google Chrome on Windows.
- [ ] Manual list/detail, deterministic automation, both agents and LM Studio verified within scope.
- [ ] Security and source policy review; no outstanding released-scope blocker/high.
- [ ] Synthetic parent handoff package validated; no parent mutation.
- [ ] Versioned docs, supported dependencies, known limitations and safe troubleshooting.
- [ ] Obsidian status, reports, summaries, receipts and reviewer evidence reconcile.
- [ ] Owner reviews and merges; local release distribution only unless separately approved.
Optional P8 tools separately feature-flagged; bulk contact harvesting excluded; no browser store publication, public hosting or automatic production deployment.

## S13 browser-scope amendment — pending Owner approval/merge

Google Chrome on Windows is the proposed sole mandatory native browser. Brave obligations
are proposed OUT OF SCOPE, not PASS; prior observations remain historical. This is effective
only on Class C Owner merge. All applicable source/security/lifecycle/accessibility controls
remain. Shared Chromium-compatible runtime is preserved. See [[Browser_Support_Policy]].
The Class C unit is independent of001 acceptance and must become effective first; it is not
blocked on001 acceptance/merge. Later integration into001 requires separate explicit authority,
preserved history/source/build/evidence and then remaining mandatory Chrome acceptance.
