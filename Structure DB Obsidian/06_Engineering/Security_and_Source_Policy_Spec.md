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
8. Independent review exact commit: verify fail-closed semantics, negative tests, scope, dependencies, logs and data flow. Blocker/high findings resolved or explicitly scoped out with owner review; no self-certification. No external source pilot until security and source gates passed separately.

## Security gate evidence
P1 fixture permission, no unauthorized DOM, safe UI/export; P3 pairing auth and expiry, malformed API and revocation; P4 runner egress, budgets, cancellation/recovery; P4b complete SEC matrix independent review; P5/P6 model injection and capability constraints; P7 export/parent no-write; P8 asset/scheduler and upgrade tests. All tests synthetic/owned, including denied domain tests without contacting prohibited site. Report command/version, exact SHA, pass/fail/skip, evidence and limitation. A code merge is not runtime source authorization or production deployment.
