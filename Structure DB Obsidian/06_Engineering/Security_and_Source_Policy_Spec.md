# Security and source authorization specification
Security controls apply from P1 onward, with independent P4b review before agent automation. No runtime claims. Default deny is mandatory in all modes. Scope owner can authorize Git writes to this repository, not third-party scraping rights.

## Source lifecycle and approval
Source registry record: sourceId, owner, origins, allowed path/scope, terms URL and reviewed date, robots status where relevant, written grant/evidence ref, allowed actions (inspect/extract/navigate/store/AI-use/export/download), allowed fields, purpose, request/page/row limits, retention, redistribution conditions, expiresAt, reviewedBy, decision, grant revision. Status pending/approved/blocked/revoked/expired. Explicit owned synthetic fixture grant for tests only; no arbitrary localhost target in live mode. Grant revision immutable; amendment creates new record and invalidates stale approvals. Permission must cover source access and downstream use separately.
Fragrantica: previous terms review recorded in S1 Source_Registry; NO-GO for automated extraction or restricted data use absent written permission. Do not fetch or configure live site-specific selectors during development; reference screenshots inform data shape only. Scento/other dataset candidates require actual license/provenance and use verification. If permission unclear use owner-generated fixtures, licensed datasets, documented APIs or owner-provided authorized data. Stop on CAPTCHA, access denial, rate limit or source objection; no stealth, proxy rotation, fingerprint bypass, session evasion or circumventing access controls.

## Threat model
| Asset / boundary | Threat | Required control | Negative test |
|---|---|---|---|
| active tab | extension reads unrelated tab | activeTab, exact lease/document, user consent | SEC-01 |
| browser profile | cookie/credential disclosure | no cookies permission, hidden form/storage excluded | SEC-02 |
| page DOM | prompt injection | text as data, tool allowlist, approval independent | SEC-03 |
| page DOM | script injection into UI | text rendering, CSP, no untrusted innerHTML | SEC-04 |
| bridge | hostile webpage hits localhost | token, Origin and Host checks, no wildcard CORS | SEC-05 |
| bridge | replay / stolen token | scoped expiring/revocable token, single-use challenge | SEC-06 |
| bridge | spoof extension/client | bind pairing to verified origin/client, validate message sender | SEC-07 |
| runner | SSRF private networks | strict origins, request/redirect/DNS/IP checks and egress isolation | SEC-08 |
| runner | unapproved redirects/assets | check each destination and request, stop if prohibited | SEC-09 |
| runner | infinite loop or excessive load | budget/time/cycle/no-progress counters | SEC-10 |
| recipe | arbitrary code, malicious selector | strict schema/allowlisted transforms, bounded selectors, no eval | SEC-11 |
| AI | grant/scope escalation | owner-only approval, hash-bound budget, strict tools | SEC-12 |
| storage | path traversal/symlink | opaque IDs, internal paths, safe root resolution | SEC-13 |
| export | spreadsheet formula injection | neutralize dangerous prefix in CSV; JSON raw where allowed | SEC-14 |
| export | secret/PII/raw prohibited material | field allowlist, redaction, rights recheck | SEC-15 |
| job | concurrent controller conflict | tab lease and worker lease generation | SEC-16 |
| database | corruption / stale replay | transactions, backup, idempotent checkpoint, explicit resume | SEC-17 |
| grant | revoked/expired policy | check at admission and each action/write/export | SEC-18 |
| dependency | malicious install or drift | license/version/lockfile, inspect hooks, restricted setup | SEC-19 |
| parent | unintended database mutation | staging export only, separate parent gate | SEC-20 |
| logs | private URLs/paths/session details | redaction and bounded metadata only | SEC-21 |
| assets | dangerous download / oversized file | explicit grant, MIME/size/filename checks | SEC-22 |
| local model | silently sends to cloud | loopback default, no fallback, egress tests | SEC-23 |
| UI | false success or hidden partial | explicit state, counts, warnings and reason | SEC-24 |

## Control details
1. Extension permissions minimal and reviewable; user activates picker on permitted origin. Injection guarded before access, navigation invalidates document references, revoke clears channel/overlay. Restricted pages unsupported and explained. No background all-tabs capture.
2. Local API binds loopback, scoped bearer secret outside Git; Origin and Host validated against exact allowed client, no wildcard CORS. One-time pairing challenge initiated by owner, expiry and replay protection, revoke active leases. MCP stdio adapter has no admin grant scope. No unauthenticated privileged GET or mutation. Rate/size/timeout bounds. Secret storage OS-appropriate; exact storage choice reviewed before implementation; never store plaintext tokens in reports.
3. Network policy exact allowed scheme/host/port and path scope, reject userinfo/unsafe schemes/ambiguous host. Validate redirects and subresources, resolve IP and reject private/loopback/link-local/metadata in live mode; control DNS rebinding with validated egress strategy, test actual browser requests, not just URL strings. Owned fixture mode explicit narrow localhost exceptions, separate configuration; never enable fixture exception for live jobs. If robust egress cannot be established, remain fixture-only. Block unapproved third-party asset requests; not a method for evasion.
4. Job approval binds exact recipe graph hash, grant revision, allowed targets, limits and expiry. Recheck before each navigation/action/export; UI/model cannot increase budgets. One active worker initially; per-source concurrency and delay, stop conditions. No automatic retries on 401/403/429/CAPTCHA. Cancel closes owned context and prevents further actions.
5. Untrusted recipes, model outputs, DOM text, imported files validated strictly. No arbitrary JavaScript, arbitrary URLs or file paths from agent. Escaped text rendering and CSP; sanitized URLs; no HTML injection. CSV neutralization for leading = + - @ and control prefixes, test actual exported cells; raw JSON separate if permitted.
6. Evidence and logs: safe IDs, status, timestamp, source grant reference, counters, error code, not raw private values. Redact URL queries where secrets possible, avoid recording entire DOM. Private sensitive material only approved private storage; public Git docs/synthetic fixtures. No account data, credentials, raw captures, unlicensed assets. Retention and delete supported; source revocation stops future use and prompts retention review, not automatic destructive delete.
7. Export artifact file name internally assigned, safe root, path and symlink validation; atomic write, quota, hash manifest, explicit download authorization. No arbitrary user path via MCP. Optional asset download checks rights, declared and detected MIME/size, no executable content, no remote upload.
8. Independent review exact commit verifies fail-closed semantics, negative tests, scope, dependencies, logs and data flow; no self-certification. Security/source/Owner decisions remain separate. The sole early P1b/P2 manual-pilot gate is PG-MANUAL-1 with all ten conditions and the protected pilot-configuration route below. Every other automated/live-source security claim requires the full TKS-IMP-006 gate. Failed/unknown pilot conditions leave BLOCKED/NOT VERIFIED; accepting a reduced fixture-only scope needs explicit reviewed Owner decision. No006 prerequisite cycle is introduced.

## Security gate evidence
P1 fixture permission, no unauthorized DOM, safe UI/export; P3 pairing auth and expiry, malformed API and revocation; P4 runner egress, budgets, cancellation/recovery; P4b complete SEC matrix independent review; P5/P6 model injection and capability constraints; P7 export/parent no-write; P8 asset/scheduler and upgrade tests. All tests synthetic/owned, including denied domain tests without contacting prohibited site. Report command/version, exact SHA, pass/fail/skip, evidence and limitation. A code merge is not runtime source authorization or production deployment.

## Class C compatibility amendment — approval packet pending
This exact documentation payload is PROPOSED, not implementation or source authorization. S11 independent architecture/security review PASS applies to its reviewed design; LOW-5 protected issuance requires the affected-clause check and Owner decision. All28 added cases are NOT RUN. Approved status changes only through Owner merge of the separately authorized Class C unit.

### Control3a — scoped manual pilot exception
## 1. Purpose and hosting model

PG-MANUAL-1 permits only a specifically reviewed, manually operated compatibility experiment
on a dedicated Owner-controlled or explicitly authorized **HTTPS origin and exact approved
path scope**. Prefer an Owner-controlled public HTTPS origin, dedicated synthetic test content,
no accounts/private data and self-contained assets. No actual origin is chosen, approved,
configured, deployed, visited or collected in S11.

No loopback/private-address exception is adopted here. Any future owned-origin loopback
proposal must be a separate explicit exception with independent security review, exact
origin/mode/path rationale and Owner approval. It cannot inherit the fixture exception or
be activated by a Source Management checkbox. An HTTPS certificate alone is not authorization.

The pilot has at most2 origins overall,2 named pages/origin,100 mounted records/page and3
deliberate reloads/page. P1b uses the first origin; P2 the second. Manual operation only:
no pagination automation, auto-scroll harvesting, background crawling, unattended jobs,
daemon, MCP, AI or external asset download. Capabilities/fields/storage/export remain separately
approved; an inspection-only pilot cannot silently exercise extraction or persistence.

## 2. Mandatory entry packet — all ten conditions required

| Gate condition | Required evidence before access | Failure consequence |
|---|---|---|
| G01 exact implementation review | Independent review of the exact implementation SHA/build responsible for source authorization, content bootstrap/document binding and the claimed pilot-containment integration, including002.09 and applicable002.13/003.09 code; server/page/CSP configuration hashes separately bound | BLOCKED; an old generic source review is insufficient |
| G02 source rights | Ownership or written authorization reference, exact actions/fields, local storage/export rights, purpose/retention and expiry; current immutable grant revision and independent source-rights decision | BLOCKED before DOM/data access |
| G03 origin/path | Exact canonical HTTPS scheme/host/port and approved paths/query policy, with redirect destinations and permitted asset destinations explicitly enumerated | BLOCKED for unknown/ambiguous scope; no wildcard origin |
| G04 controlled page/assets | Dedicated controlled page and server build; self-contained approved assets or an independently reviewed restrictive CSP with explicit supported directives, no unreviewed third-party resources | BLOCKED if page/network behavior is uncontrolled |
| G05 observed network evidence | Browser request evidence for the exact pilot page/configuration and proposed operation in each target browser; top-level/subresource/connect/worker/redirect behavior and actual destinations recorded with sanitized hashes/references | BLOCKED if behavior is merely assumed from source/configuration |
| G06 no unexpected destinations | Observed trace has no unapproved redirects, external assets or unexpected destination requests; browser/server/CSP failure reports and deviations assessed | BLOCKED on any violation; stop further pilot actions and preserve failure evidence |
| G07 manual bounded operation | Exact manual procedure and budgets, no automatic page turning, navigation jobs, scraping loops or unattended execution | BLOCKED if automation is required or activated |
| G08 no later-system dependency | Pilot works without daemon, MCP or AI, using only reviewed manual extension and controlled page/environment | BLOCKED for that pilot; do not pull later phases forward |
| G09 explicit Owner pilot approval | Owner approval bound to implementation SHA, grant revision, page/server/CSP hashes, network evidence, exact scope/actions and budgets for this pilot | BLOCKED; plan approval, UI event or source ownership alone is not pilot approval |
| G10 continuous admission/stop rule | Current wall-clock validity, actual document/grant revision and all entry packet bindings checked before each applicable operation; stop procedure and retained audit evidence defined | BLOCKED on any missing/expired/changed binding; no new privileged work |

Approval records distinguish independent implementation/security review, source authorization,
network evidence review and Owner pilot approval. They are not one blanket authorization.
The entry packet consumes evidence from a separately authorized qualification run supplied by
the page Owner or independent reviewer. That run needs its own explicit Owner qualification
authorization, rights and reviewed code/configuration limits; it is not provisional permission
for an agent to visit a site. Its bounded procedure must cover the proposed manual operation
in each target browser. Final G09 pilot approval follows review of that evidence. If no such
authorized evidence exists, G05 is PENDING and pilot entry is BLOCKED. This proposal grants
neither qualification access nor pilot access, and no qualification run occurs in S11.
Material code/page/CSP/grant/scope change invalidates the packet; repeat only affected review/
evidence plus required regression and obtain a new explicit decision. No automatic approval
from a test report, imported recipe or agent request. Grant-administration restrictions in
[[Architecture_Implementation_Spec#6.1 Carried source-administration ban — MEDIUM-2]] still apply.

## 3. Control ownership — never attribute host-page containment to activeTab

| Control owner | Enforced/verified responsibility | Not a claimed capability |
|---|---|---|
| Extension | Deny unapproved DOM injection/inspection/commands, actual URL/path/capability/document/revision checks, bounded work, cleanup, no extension-initiated unapproved fetch/asset downloads | Cannot guarantee interception/prevention of all host-page or browser-initiated requests/redirects |
| Pilot page/application | Dedicated reviewed content/client code, explicit allowed links/actions and no unreviewed network integrations | A benign source-code claim is not a substitute for observed browser behavior |
| Pilot server | Approved page/assets/redirect configuration, origin/path ownership and controlled responses | Does not control arbitrary destinations outside that server |
| CSP | Independently reviewed, actually delivered/enforced restrictive resource/connect/frame/worker/form policies supported by the browser | Do not assume unsupported directives or universal top-level navigation prevention |
| Browser environment | Actual TLS/CSP enforcement and attributable bounded request evidence in each designated browser/profile | Not a general network sandbox merely because it is a test profile |
| Independent reviewer / Owner | Verify exact evidence/limits and approve the bounded packet; stop/reject unsupported claims | No agent/source UI may self-certify human approval or general live-job security |

Record the precise control that prevented a request, or that only observed it. An unexpected
request is a gate failure even if the extension later denies DOM access; do not relabel a
request that already occurred as prevented. Stopping extension work cannot retract an outbound
request or prove unobserved traffic never occurred. Only the reviewed pilot page/run is covered.
Unrelated browser-service traffic must be distinguished rather than hidden; if attribution
cannot establish the required page boundary, block the pilot.

## 4. Operation, expiry and incident behavior

Evaluate status/revision/expiry against wall clock at every applicable privileged command,
after asynchronous boundaries and before DOM access or storage/export effect. Cached approval,
scheduled alarm or an active MV3 worker is not sufficient. Sleeping/restarted worker, stale
document, expired grant or changed pilot packet denies before further work; content clears
unsafe selection on its next active check/resume. No promise that a suspended process executes
cleanup at a precise real-time deadline. See AUTH-03's lifecycle matrix in the architecture.

On any failed entry/runtime condition: stop, invalidate the pilot lease/confirmation, remove
overlays where the content context can run, suppress new extraction/write/export, record
BLOCKED with exact condition/evidence and seek fresh review. Preserve already-permitted
results according to retention policy; do not delete Owner data or backups automatically.
No fallback to permissive host rules, fixture-mode grants or unaudited external browsing.

## 5. Acceptance and dependency consequences

- PG-MANUAL-1 is a bounded early gate, **not completion of006**. It depends on exact002/003
  implementation/page/security review and source rights, not daemon/MCP/AI or all later tasks.
  No dependency edge002→006→002 is introduced.
- In the default proposed early-pilot path, a failed gate means the pilot parts of002.13 or
 003.09 and corresponding COMP-13 remain BLOCKED/NOT RUN; the task's full proposed pilot
  acceptance cannot be marked passed from fixture success.
- Fixture-only development may continue under its separately approved grant. If an early
  pilot cannot be established, Owner must explicitly choose a docs-reviewed offline-only
  scope/acceptance alternative before accepting/merging that reduced deliverable. Mark live
  compatibility NOT VERIFIED and defer its pilot cases to the appropriate later security
  gate. This is not an automatic waiver or a false PASS, and is not chosen by the agent now.
- The full automated/live-source gate remains TKS-IMP-006, including network/DNS/IP/redirect/
  subresource controls and independent review before later automation. No early-pilot evidence
  is substituted for that gate or for007's separate agent-approval boundary review.

## 6. Proposed verification

AUTH-02 verifies canonical app scope/denial plus the **observed controlled pilot model**, not
universal host-traffic prevention. COMP-13 cannot run on a real origin until PG-MANUAL-1
passes. New AUTH-09 independently checks all ten entry conditions, packet version/hash
bindings, expiry/change/violation stop behavior and the offline/BLOCKED acceptance branch.
Each missing condition is a negative control. All cases are PROPOSED/NOT RUN in S11.


### Protected pilot issuance addendum — LOW-5
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

## 4. Exact Class C integration and verification

This is an addendum to, not an in-place edit of, the independently reviewed S11 proposal:

- Security Control3a/8 and gate-evidence paragraph: protected pilot issuance with exact
  G01/G09 bindings and the authority/integrity fail-closed rule above.
- Source Registry: a proposed protected pilot-grant policy, no actual grant/origin; distinct
  from read-only Fixture v1/v2 and ordinary non-fixture pending requests.
- Architecture Sources step3/§6.1: retain blocked dynamic approval; permit only this reviewed
  protected issuance route after its own security/Owner conditions. Agents cannot administer it.
- R13.06/07/08 and R16.04,002.09/13 and003.09/11: clarify the protected packet and current checks.
- AUTH-04: protected seed/configuration cannot be edited/replaced/shadowed through the forbidden
  channels. AUTH-09: test missing/mismatched code/configuration/grant/rights/Owner bindings,
  revocation/expiry and all G conditions. AUTH-10: agent/API/import/page/UI attempts denied.

No new requirement/subtask/case ID. Totals remain75/91/210, ten major tasks,00213/00311;
all28 proposed cases NOT RUN. The future Class C reviewer verifies these affected clauses
against the completed review and actual proposed patch. That targeted review is PENDING;
do not label this author-proposed LOW-5 correction independently resolved. LOW-5 does not
reopen the seven closed findings or remove readiness for Owner amendment decision.


### Operation expiry and future agent exposure
### 6.1 Carried source-administration ban — MEDIUM-2

Source grant approval, capability expansion and expiry extension must not be callable through
MCP tools, daemon agent APIs/tokens, AI planners, recipe imports, page messages or automated
agent workflows. Indirect browser/Computer Use UI automation is included: no privileged UI
shortcut around API denial. Agents may prepare requests/proposals only, never approve them.
Do not issue agent credentials a grant-admin scope or expose an approval action in free-form tools.

A real trusted event is not proof of human Owner identity. An agent with unrestricted same-
user OS/browser control can potentially operate a naïve Sources UI; acknowledge that threat,
do not claim a human-only guarantee from a button or profile name. The eventual Owner-controlled
authority must be outside agent-reachable authority/channels under the reviewed threat model.
Possible mechanisms need separate security design; none is chosen, installed or implemented here.
Policy text or self-declared actor labels alone do not satisfy that review.

Manual-only002/003 does not gain a daemon/MCP/AI dependency. Protected fixture grants change
only by reviewed configuration/code revisions. Non-fixture dynamic approval remains unavailable
until its appropriate Owner-controlled mechanism is reviewed; a manual pilot still needs the
separate PG-MANUAL-1 packet and explicit Owner approval. Before007 exposes any agent/browser
workflow,006.03/006.06 and007.04/007.07/007.08 must independently test actual MCP/daemon/AI/
browser/computer-use capabilities against this boundary. If isolation is not demonstrated,
block affected approval/agent exposure. AUTH-10 plus SEC-18 and AGENT-05/07 carry this constraint.
Fixturev2 AI-ingestion denial is not implicitly broadened to make agent demos pass.

### 6.2 Wall-clock expiry at every operation — LOW-4

Evaluate current wall clock against valid expiresAt/status/revision for every applicable
privileged command and operation, not merely on alarms or lease creation. Reject expired,
revoked, unknown, invalid-date, stale-document/revision or untrusted cached grants. Check after
asynchronous boundaries and immediately before effects; cached successful checks confer no authority.

| Boundary | Required fail-closed behavior |
|---|---|
| START/inspect/preview/Lock/Confirm/filter/Undo/Reselect and STATUS that exposes protected selection data | Check current grant/document and wall-clock expiry before authorized DOM/state disclosure; content event guards use bounded current context and expiry, broker reloads authoritative grant/revision |
| Sleeping worker or worker restart | On wake/request re-read status/revision/expiry; reject old lease/cache before action; do not rely on an alarm having fired |
| Expiry during active selection/content resume | On active event/check/resume invalidate unsafe selection/confirmation and clean overlay; no new DOM operation under expired authority |
| Manual extraction | Check before field reads, bounded async chunks and result publication; stop with explicit partial/error, never fabricated success |
| Local storage | Check before starting writes and immediately before commit; abort pending atomic work where possible if authority expires/revokes |
| Export | Check current export rights/provenance and clock immediately before initiating download; prevent later exports under expired grant |
| Pending responses/queue | Recheck before publishing/updating lease/view; no old completion revives authority or Confirmed |

Alarms, heartbeat and watchdog are supplementary cleanup mechanisms. A suspended process
cannot promise exact-deadline execution; on resumption no privileged work proceeds before
validation. Clock-invalid/detected clock-anomaly conditions fail closed without fetching a
remote clock or adding a backend dependency. Do not promise recall of a download or reversal
of a commit already permissibly initiated/completed; preserve exact partial/effect evidence,
stop subsequent work and follow retention review without automatic data/backup deletion.
AUTH-03 expands across all rows; AUTH-05/06/07 and PG/AUTH-09 bind the relevant boundaries.



Security gate evidence: P1 inspection remains fixture-only; P1b/P2 local fixture extraction/store/export uses reviewed protected Fixture v2. Early external manual pilots use PG-MANUAL-1 plus independent source rights and Owner approval. Full006 security, later007 actor review, P6/P7/P8 gates and no-production/no-parent permissions remain.

## S13 browser-scope amendment — pending Owner approval/merge

Google Chrome on Windows is the proposed sole mandatory native browser. Brave obligations
are proposed OUT OF SCOPE, not PASS; prior observations remain historical. This is effective
only on Class C Owner merge. All applicable source/security/lifecycle/accessibility controls
remain. Shared Chromium-compatible runtime is preserved. See [[Browser_Support_Policy]].
The Class C unit is independent of001 acceptance and must become effective first; it is not
blocked on001 acceptance/merge. Later integration into001 requires separate explicit authority,
preserved history/source/build/evidence and then remaining mandatory Chrome acceptance.
