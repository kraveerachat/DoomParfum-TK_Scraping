# Source registry — mandatory deny by default
| Source | Rights / evidence as of 2026-11-25 | Status |
|---|---|---|
| Owned synthetic fixtures | generated fictional data, no external network | ALLOWED for test |
| Fragrantica | https://www.fragrantica.com/Terms-of-Service.phtml §§5.2–5.7 (review exact clauses before permission decision); terms prohibit automated access without express written permission and restrict AI/data mining/dataset uses | NO-GO for automated extraction, AI ingestion, download or bulk catalogue reuse unless written grant explicitly covers it |
| Scento or other datasets | https://github.com/Scento-com/perfume-dataset ; license, provenance, storage and downstream use not verified | RESEARCH only |
| Official brand feeds / APIs | domain-specific terms, license and rate limits required | PENDING per source |
| User-provided data | only owned/authorized inputs; rights and private data still checked | PENDING per import |
Record owner, domains/origins, terms URL/date, robots applicable, authorization scope and expiry, allowed actions/fields, storage/AI/export/redistribution permissions, rate and retention limits, decision and evidence ID. Robots is not license. Grant must be separate from user permission to access repository. Default deny all modes including manual extension extraction for blocked domains; no site-specific selectors, live testing, crawling or anti-bot bypass for Fragrantica until approval. UI references may inform synthetic fixtures; ordinary personal reading not a grant to automate. Stop on CAPTCHA/access denial. Public Git only synthetic or explicitly shareable metadata. No personal contact harvesting. Policy negative tests include redirect, subresource, expiry and scope expansion.

## Class C compatibility amendment — approval packet pending
This exact documentation payload is PROPOSED, not implementation or source authorization. S11 independent architecture/security review PASS applies to its reviewed design; LOW-5 protected issuance requires the affected-clause check and Owner decision. All28 added cases are NOT RUN. Approved status changes only through Owner merge of the separately authorized Class C unit.

## P1a owned fixture grant v1 — preserved current implementation policy
Owner-generated fictional fixtures, authorized by TKS-IMP-001. Source ID `owned-fixtures-v1`;
origin exactly `http://127.0.0.1:4173`; paths `/fixtures/catalogue`, `/fixtures/table`,
`/fixtures/single`, `/fixtures/nested`, `/fixtures/mutation`, `/fixtures/navigation`,
`/fixtures/alternate`, `/fixtures/large`, `/fixtures/stress`. No userinfo, query or fragment.
Action: inspect visible DOM structure and stable synthetic record IDs only. No extraction,
AI use, export, external download or live navigation. Grant expires `2027-11-25T00:00:00Z`.
Revision requires reviewed code change; no runtime grant editor. Test evidence may be
retained in this public-safe vault. Generated profiles/logs remain ignored and disposable.
No third-party assets or network resources. Other localhost names/ports/paths are denied.

## Protected Fixture Grant v2 — reviewed future manual capabilities
## Identity, scope and protected configuration

- Preserve the registered source identity `owned-fixtures-v1` for provenance continuity;
  it is a legacy source identifier, not an authority to use v1 capabilities for v2.
- Proposed new immutable permission envelope: `grantId=owned-fixtures-v2`,
  `grantRevision=2`, `policyVersion=2`, status PROPOSED until separately approved and shipped.
  Requests bind the exact source/grant/revision tuple; v1 approvals do not authorize v2.
- Exact origin remains `http://127.0.0.1:4173`; only `/fixtures/catalogue`, `/fixtures/table`,
  `/fixtures/single`, `/fixtures/nested`, `/fixtures/mutation`, `/fixtures/navigation`,
  `/fixtures/alternate`, `/fixtures/large`, `/fixtures/stress`. No userinfo/query/fragment,
  other localhost names/ports, path prefixes or external origins. No live-mode exception.
- Proposed expiry is no later than the current `2027-11-25T00:00:00Z`; actual issued/approval
  metadata is set only by the later reviewed change. Any extension requires a new reviewed
  protected revision, never a Sources UI edit or an agent action.
- Protected packaged/reviewed configuration only. Ordinary Sources UI shows this grant
  read-only and cannot edit, replace, duplicate, shadow or widen it—even through another
  source ID whose normalized origin/path overlaps the reserved fixture scope. Imports cannot
  override it. A subsequent protected revision requires explicit code/configuration review.
- Non-fixture source records use the separately proposed authorization workflow, not this
  seed. The current “no runtime grant editor” statement remains true for protected fixtures;
  any future non-fixture workflow is a separately reviewed exception to that statement.

## Proposed capability matrix

| Capability / destination | Fixture v1 now | Proposed protected v2 | Conditions |
|---|---|---|---|
| Inspect visible DOM/structure | Allowed | Allowed | Exact source/path, current browser gesture/document/grant and bounds; no secrets/hidden form capture |
| Manual field extraction | Not authorized | Allowed | Explicit user action, reviewed single-page recipe/field allowlist and current confirmed selection; synthetic fixture data only |
| Local dataset storage | Not authorized | Allowed | Local extension IndexedDB, bounded validated records, source/grant/recipe provenance, quota/atomicity checks |
| Local JSON/CSV export | Not authorized | Allowed | Explicit local user export, allowed fields/provenance, safe JSON/CSV quoting/formula neutralization; no upload |
| Automated navigation/pagination/scroll jobs | Denied | Denied | Owner manual browsing between named fixtures is not an automated job; new document still needs fresh authorization |
| AI ingestion/model disclosure | Denied | Denied | No model input of fixture records under v2; later agent/AI tests require a separately approved eligible grant/revision |
| External asset download | Denied | Denied unless separately approved | No such approval in v2; image URL references may be allowed recipe values but are not fetched/downloaded |
| Unapproved origin/path or unsafe scheme | Denied | Denied | No fallback to broad host permissions or synthetic grant injection |
| Cookies/passwords/tokens/private stores/raw scripts | Denied | Denied | Not fixture extraction fields; existing secret/privacy controls retained |

Inspection, extraction, storage and export are explicit separate permissions in v2. A generic
“approved” flag is insufficient. All allowed fixture fields are fictional visible values or
permitted href/image-reference attributes defined by the approved recipe; no asset/network
fetch or unrelated browser data. Budgets remain the lower of grant/global/recipe limits.

## Explicit transition and invalidation

1. After the amended plan and001 predecessor gates,002.09 obtains explicit Owner fixture-rights
   approval and independent review of the exact v2 implementation/metadata. Record grant hash,
   allowed actions/fields and expiry; do not silently alter v1's existing record/history.
2. Ship a strict versioned contract/configuration change. Revoke all old fixture leases,
   selections, queued commands and approvals; old source/grant tuples and old messages cannot
   auto-upgrade. Worker restart alone is not the sole revocation mechanism.
3. Existing recipes/imported records retain their original provenance. Any adaptation to v2
   enters draft, gets explicit capability validation and new approval/hash; old approved claims
   are never carried forward. Obtain a fresh toolbar gesture and current document selection.
4. Check wall-clock expiry/status/revision before every applicable command/operation and
   side effect. Expired/revoked/cached v2 cannot authorize new reads/writes/downloads. Abort
   pending atomic work where possible; preserve actual partial/error evidence and retention.
5. UI and ordinary source imports cannot edit/replace v1/v2, extend expiry or insert an
   overlapping alternate fixture source. All non-fixture approval obeys the agent-admin ban.

## How future002.04–002.07 operate without a bypass

| Existing subtask | Capability checks under reviewed v2 | Explicit denial/negative control |
|---|---|---|
| 002.04 deterministic single-page run | inspect + manual extraction, selected document/grant/recipe/fields, wall clock and budgets rechecked | v1, missing extract, expired grant, stale document/revision deny before extraction |
| 002.05 local dataset/recipe storage | store-records for derived values/keys; allowed local destination and atomic transaction; provenance retained | v1/missing store, expiry/revoke before commit, quota/error prevents success; no silent overwrite |
| 002.06 standalone data view | Display only already-authorized local data through safe rendering and its retained provenance; no fresh DOM read/network fetch | Opening data view does not grant extraction/export or disclose records to a model |
| 002.07 JSON/CSV/export/deletion | current export rights/provenance and explicit local action; existing safe-export/deletion controls | missing export, stale/revoked rights or agent request cannot download/upload; deletion still separate Owner confirmation |

AUTH-01 expands to compare v1 denial against each v2 allowed/denied capability; AUTH-03 covers
revision/expiry/migration invalidation; AUTH-04 covers protected-origin shadow/edit/import
attempts; AUTH-06 covers actual storage/export effects; AUTH-05/07 reject old context replay.
AGENT-07 and future AUTH-10 preserve denied AI ingestion and grant-administration isolation.
All are proposed tests, NOT RUN; no current runtime grant or approved Source_Registry is edited.


## Protected pilot configuration policy — no actual source granted
## 1. Proposed issuance route and authority

In proposed002.09, allow a dedicated pilot source only through a protected, independently
reviewed configuration revision, analogous to protected Fixture v2. It is a separate exact
HTTPS source grant, never the fixture seed or a runtime Sources form. Agents may draft
pending request metadata for review; that draft cannot replace or modify an active grant.
Runtime UI/commands/imports/page messages/MCP/daemon APIs/AI planners/automated-agent and
Computer Use workflows cannot create, modify, approve, activate, expand or extend it.

Owner explicitly authorizes the exact protected revision through the reviewed release/
configuration approval process outside agent-reachable approval authority. A local author
commit, an `approved=true` claim, `isTrusted`, an imported receipt or an agent-operated button
is not that authorization. Protected configuration loading accepts only independently
reviewed, Owner-approved revisions under the actual reviewed integrity/authority boundary;
arbitrary local files/duplicate records/overlapping scopes cannot shadow them.

The configuration/release integrity and Owner-decision mechanism must itself be independently
verified before activation at the exact implementation SHA. This proposal does not select
an unreviewed signature scheme or claim file immutability against unrestricted same-user OS
control. If the effective threat model permits an agent to substitute configuration or approval,
the gate stays BLOCKED until a suitable Owner-controlled mechanism is established. General
dynamic source administration remains BLOCKED; no early daemon/MCP/AI dependency is introduced.

## 2. Exact approval packet bindings

G01/G09 jointly bind the following, with no self-referential hash embedded into its own payload:

| Binding | Required content |
|---|---|
| Implementation | Exact reviewed code commit, production build hashes and protocol/policy version; source authorization and protected-loader/invalidation behavior reviewed |
| Pilot grant | Unique grantId/sourceId, immutable revision, canonical configuration byte hash, status, validity interval and revocation/version reference |
| Rights | Ownership/written authorization evidence reference and hash; permitted actions/fields, storage/export/retention/redistribution rights and expiry |
| Scope | Exact HTTPS scheme/host/port and named paths/query policy; no inherited loopback exception, wildcard or permissive fallback |
| Controlled environment | Page/server/CSP configuration and asset hashes; exact browser/profile qualification procedure and sanitized attributable request evidence |
| Decisions | Independent affected-clause/implementation security review, independent source-rights decision and explicit Owner qualification/pilot approvals, each bound to the tuple above |
| Limits | Manual-only budgets, no automated navigation/crawling/unattended jobs, no daemon/MCP/AI, denied unapproved destinations/assets and exact stop procedure |

Store review/approval receipt references outside the hashed grant payload; the final packet
binds actual code/configuration hashes. Keep private rights documents/credentials/raw captures
outside public Git. No actual source, origin, grant record or approval value is selected in S12.

## 3. Admission, revision and fail-closed behavior

All PG-MANUAL-1 G01–G10 remain mandatory. Protected issuance solves only the missing issuance
route; it does not waive source rights, approved network evidence, explicit Owner decisions
or the full006 gate. Qualification evidence needs its own authorization as already specified.

Before every applicable privileged operation, after awaits and before DOM/state/storage/export
effects, recheck the authoritative current grant revision/configuration integrity, wall-clock
expiry/status/revocation, scope, capability/field predicates, actual tab/document/generation
and reviewed packet bindings. Browser activeTab is additionally required. Cached grant data,
worker restart or scheduled alarms alone confer no authority.

A change in code, grant revision, scope/rights, page/server/CSP/configuration or revocation
invalidates affected leases, confirmations, pending operations and pilot receipts. Repeat
affected independent review/evidence and explicit Owner approval; no automatic reactivation,
expiry extension or replay. Unknown/failed predicates keep pilot BLOCKED. Stop further work,
invalidate unsafe state and clean overlays where execution is available. Do not promise cleanup
while suspended, recall of completed exports or automatic deletion of retained data/backups.

## S13 browser-scope amendment — pending Owner approval/merge

Google Chrome on Windows is the proposed sole mandatory native browser. Brave obligations
are proposed OUT OF SCOPE, not PASS; prior observations remain historical. This is effective
only on Class C Owner merge. All applicable source/security/lifecycle/accessibility controls
remain. Shared Chromium-compatible runtime is preserved. See [[Browser_Support_Policy]].
The Class C unit is independent of001 acceptance and must become effective first; it is not
blocked on001 acceptance/merge. Later integration into001 requires separate explicit authority,
preserved history/source/build/evidence and then remaining mandatory Chrome acceptance.
