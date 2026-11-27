# MCP server and external agent integration
Design for local Codex and Claude Code clients using supported MCP transport. Custom MCP is narrow domain tooling; upstream Playwright MCP is optional separate interactive fixture-development tool, never unrestricted production bypass.

## Actor authority
Owner grants source rights, tab lease, recipe approval and job scope. Agent may inspect permitted data, propose selection, create draft recipe, run within already approved scope, view bounded results and cancel. It cannot approve its own recipe, alter source grants, acquire a new tab without consent, call arbitrary browser JavaScript, read local filesystem or send data to a third-party endpoint. Daemon independently validates every call. Page content and tool output are untrusted input, not instructions.
Agent client and extension are independently paired. Revocation of either invalidates relevant actions. Local stdio adapter uses scoped daemon credential outside Git; no access to owner browser cookies. Do not expose local daemon publicly or assume ChatGPT web can reach localhost.

## Tool catalog v1
All inputs JSON-schema validated, no unknown keys, IDs bounded strings. All outputs structured result or safe error. Client tool name, input, result and scope below are proposed stable contract.
| Tool | Input | Result | Side effect / permission |
|---|---|---|---|
| tk_list_sources | cursor, limit | allowed source summaries + capabilities | read, no secret grant material |
| tk_list_recipes | cursor, limit, status | recipe summaries + revisions | read |
| tk_get_recipe | recipeId, revision | exact allowed recipe | read |
| tk_pair_status | none | connection, lease availability, safe state | read |
| tk_inspect_tab | leaseId, scopeRef? | bounded DOM refs, generation, count, truncation | leased permitted tab read |
| tk_propose_selection | leaseId, generation, groupRef, fieldRefs | proposalId, predicted shape | visible proposal; no physical click |
| tk_validate_recipe | draft recipe | errors, hash, warnings, fixture requirements | offline validation, no site action |
| tk_save_recipe | draft recipe, expectedRevision | draft revision and hash | draft write only |
| tk_preview | recipeId/revision, targetRef, approvalRef? | sample, counts, warnings | explicitly permitted preview scope |
| tk_run | recipe/graph ref, approvalRef, idempotencyKey | jobId, state | bounded authorized job |
| tk_status | jobId | state, counters, reason | read |
| tk_results | jobId, cursor, limit | allowed bounded rows, next cursor, counts | read, no raw full dataset |
| tk_export | datasetId, format, selection? | artifactId, manifest summary, safe local ref | local export only |
| tk_cancel | jobId, reason? | state and reason | cancel own authorized job |
No generic click/navigate/eval/shell/get-cookies/download-url or arbitrary file-read tool. Approval UI and source registry administration are not model tools. Job limits enforced by daemon even when MCP schema accepts a value.

## Common tool result
```json
{"ok":true,"requestId":"req-01","data":{"jobId":"job-01","state":"queued"},"warnings":[]}
```
```json
{"ok":false,"requestId":"req-02","error":{"code":"STALE_DOCUMENT","message":"Select the current document again","retryable":false}}
```
Proposal input example: leaseId, generation, groupRef and array of fieldRefs from last permitted inspect. Refs are opaque, document scoped, expire on navigation, cannot be arbitrary selectors to hidden fields. Recipe creation validates selectors and policy. Preview returns max 10 records/64 KiB by default, results paginated with cursor (proposed max 100 records and 64 KiB per call); if oversized, truncate explicitly and return cursor. No entire dataset dumped into context. Export result returns opaque artifactId and safe relative location; actual user opens local data view, not arbitrary FS.
Error taxonomy: AUTH_REQUIRED, SCOPE_DENIED, POLICY_DENIED, STALE_DOCUMENT, LEASE_EXPIRED, VALIDATION_FAILED, APPROVAL_REQUIRED, CONFLICT, LIMIT_REACHED, JOB_NOT_FOUND, OFFLINE, INTERNAL. Retryable only transient offline/status; do not retry policy/approval failure.

## Agent-driven selection flow
1. Operator starts daemon, loads extension, pairs client, selects permitted tab and grants lease. Agent checks tk_pair_status.
2. Agent calls tk_list_sources and tk_inspect_tab with lease. Result contains bounded structural summary and stable refs, not arbitrary HTML or sensitive inputs.
3. Agent identifies candidate groups/sections and calls tk_propose_selection. Extension validates generation and shows highlighted proposal and field list; owner accepts/edits/rejects.
4. Accepted selection becomes draft recipe input. Agent calls validate/save draft, reports warnings. Deterministic preview verifies values; no model-invented record data.
5. Owner reviews recipe hash, source scope and job budget; approval created outside MCP by explicit local UI. Agent receives approval reference only.
6. Agent calls tk_run, polls tk_status, uses tk_results bounded sample, cancels if instructed. Owner can revoke/cancel directly; agent cannot bypass.
7. Agent requests export and summarizes counts/errors. Result stays local, only approved minimized samples can be sent to cloud-hosted agent. External model disclosure separate from extraction rights.

## Codex integration
- P5 inspect current installed Codex CLI/IDE capabilities, supported MCP config mechanism, project skill discovery path and actual supported command invocation. Do not guess slash names.
- Create repo-local integrations/codex instructions and example config only after versions checked. User explicitly chooses to register custom stdio server; no global config mutation without authorization.
- Skill intent: "use Tk-Web-Scraper tools; check pairing/source before action; inspect/propose; validate and preview; ask owner for scope approval; run/status/export; summarize evidence".
- Test plain prompt invocation and supported short skill/command syntax. If slash unavailable document fallback. Run on synthetic owned fixture, compare exact rows with manual mode. Log client version, tool list, invocation, safe output and negative test.
- Existing Codex subscription/client is user-controlled; no assumption subscription gives external API key, no token harvesting or unofficial subscription proxy.
## Claude Code integration
- Verify installed Claude Code version and supported project-level MCP configuration, skill/custom command behavior and permission prompts.
- Use same stdio adapter/tools and separate integration docs; do not copy Codex config blindly. Configure only with explicit owner permission.
- Equivalent fixture workflow and negative tests; compare recipe hash, result schema and export with Codex/manual. Unsupported command documented as fallback.
- Prefer project-scoped configuration, no global hooks or arbitrary downloaded skills; inspect dependencies and consent.

## Optional Microsoft Playwright MCP
Use upstream github.com/microsoft/playwright-mcp to inspect local fixtures or debug browser behavior after version/license review. Existing browser connection may require separate upstream extension and explicit tab selection; verify actual Google Chrome on Windows support. Do not assume it shares custom runner profile or bypasses site policy. Never attach unrestricted MCP to authenticated everyday browser as default; prefer dedicated fixture context. Do not run both controllers on same tab concurrently. Upstream tool availability not required for Tk-Web-Scraper core.
## Acceptance
MCP protocol initialize/list tools/call typed tool; stdout protocol only, stderr sanitized. Reject malformed input, stale lease, unapproved run, excessive result page, untrusted page instruction, client disconnect; verify daemon job survives agent exit. Both Codex and Claude actual fixture demos, same contract; user can see proposal and revoke. If either client unavailable mark acceptance pending, not PASS. No paid API required.

## S3 packaging clarification
See [[Skill_Packaging_and_Usage_Modes]]. Deliver a portable installable tk-web-scraper skill package plus client-specific supported setup in P5 (subtasks 007.01–007.06, MCP-10/11/12 and AGENT-01–04). Skill is workflow instructions, not an MCP runtime, installer, API key or authorization. Standalone P1/P2 extension runs without skill, model or server. P4 daemon runs approved recipes without agent after submission. P6 extension Local AI panel uses local planner to call LM Studio and validated daemon API. Codex/Claude subscriptions remain in their official clients; no subscription-to-API proxy. Actual invocation syntax validated on installed clients.

## Class C compatibility amendment — approval packet pending
This exact documentation payload is PROPOSED, not implementation or source authorization. S11 independent architecture/security review PASS applies to its reviewed design; LOW-5 protected issuance requires the affected-clause check and Owner decision. All28 added cases are NOT RUN. Approved status changes only through Owner merge of the separately authorized Class C unit.

## Grant-administration prohibition and007 security gate
No MCP tool, daemon agent API/token, AI planner, import, page message or automated/Computer Use UI workflow may create/modify/approve/activate a source grant, expand capabilities or extend expiry. Agents may prepare pending requests only. isTrusted and actor=owner are not identity. Protected fixture/pilot config revisions require independently reviewed Owner-controlled authority/integrity outside agent channels; arbitrary files or overlapping grants cannot replace them. Before007 exposure independently verify actual capabilities in006.03/06 and007.04/07/08 using AUTH-10,SEC-18,AGENT-05/07. If isolation is unverified, affected approval/exposure BLOCKED. Current expiry/status/revision/scope/capability predicates apply at every privileged operation; alarms supplemental. No early daemon/MCP/AI prerequisite.

## S13 browser-scope amendment — pending Owner approval/merge

Google Chrome on Windows is the proposed sole mandatory native browser. Brave obligations
are proposed OUT OF SCOPE, not PASS; prior observations remain historical. This is effective
only on Class C Owner merge. All applicable source/security/lifecycle/accessibility controls
remain. Shared Chromium-compatible runtime is preserved. See [[Browser_Support_Policy]].
The Class C unit is independent of001 acceptance and must become effective first; it is not
blocked on001 acceptance/merge. Later integration into001 requires separate explicit authority,
preserved history/source/build/evidence and then remaining mandatory Chrome acceptance.
