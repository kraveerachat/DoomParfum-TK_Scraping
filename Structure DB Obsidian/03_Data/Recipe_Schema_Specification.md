# Versioned recipe and result contract
Design specification v1.0 proposed. JSON examples are documentation, not runtime files. Generic rules only, fictional data. Canonical fields and names below refine S1 summary. All tools validate the same contract; no arbitrary executable input.

## Recipe entity
| Field | Type | Required | Validation |
|---|---|---|---|
| schemaVersion | string | yes | supported major 1, reject unknown |
| recipeId | opaque string | yes | generated local, <=128 chars |
| revision | integer | yes | >=1, immutable per saved revision |
| name | string | yes | 1–120 chars, safe display |
| sourceId | string | yes | exact registered source |
| grantRevision | integer | yes | match current active grant |
| status | enum | yes | draft/validated/approved/retired |
| pageType | enum | yes | list/detail |
| allowedOrigins | string[] | yes | subset of grant, exact scheme/host/port |
| startUrls | string[] | yes | validated, no secrets, within grant and limits |
| rootSelector | selector object | yes | bounded, scoped |
| recordSelector | selector object/null | conditional | list requires, detail singleton |
| recordKey | field reference | yes | stable approved key or documented fallback |
| fields | Field[] | yes | unique keys at each level, nonempty |
| navigation | strategy object | yes | none/next/load-more/scroll |
| edges | Edge[] | no | bounded acyclic workflow |
| limits | Limits | yes | <= global and grant limits |
| validation | object | yes | expected samples, fixture evidence, rule hash |
| provenancePolicy | object | yes | raw value/URL retention restrictions |
| createdAt/updatedAt | UTC ISO8601 | yes | system-generated |
| approvalRef | string/null | conditional | required for approved revision, bound hash |

## Field and selector
| Field | Type / meaning |
|---|---|
| key | stable machine key, unique sibling, no prototype keys |
| label | human-readable display name |
| kind | scalar/object/list |
| selector | scoped CSS primary, alternatives bounded; XPath only later supported |
| scope | record or explicit section; never implicit document fallback |
| extract | text/attribute/url/number/object/list (allowlisted enum) |
| attribute | permitted attribute only for attr extraction |
| cardinality | one/optional/many; kind compatible |
| required | boolean; one required not silently null |
| valueType | string/number/boolean/url/object/array |
| transform | allowlisted normalization enum; no free code |
| children | field array for object/list, bounded depth |
| maxMatches | positive integer <= configured cap |
| evidence | raw value retention and selector reference within grant |
Selectors: engine css; expression 1–1024 chars proposed; alternatives max 3; scoped root; expected match count; no arbitrary JavaScript. Validate syntax and bounded query execution. Unknown keys reject rather than ignore for security-critical fields.

## Navigation and workflow
Navigation type none, next, load-more, scroll; selector only for next/load-more; maxPages, maxActions, maxNoProgress, delayMs and timeout bounded by grant. Only approved next/load button, not arbitrary click. Scroll target explicit; stop on repeated keys, disabled control, no-progress, limits, challenge, or permission change. Edges connect list field of permitted detail links to approved detail recipe revision, with max fanout and depth. No cycle; each edge origin and field validated. Job approval binds recipe graph hash and source grant(s); changes invalidate approval.
Limits initial fixture pilot proposed: maxPages 3, maxRows 50, maxActions 10, maxDurationMs 60000, concurrency 1, maxDetailDepth 1, preview 10 rows and 64 KiB. Conservative starting values for owned fixture tests, not universal website rate. Grant can lower; actual live pilot separate approval. Hard cap proposals 100 pages/10000 rows/30 min, still subject to grant and review; no UI override without approved policy change.

## Example synthetic list recipe
```json
{
  "schemaVersion": "1.0",
  "recipeId": "fixture-brand-list",
  "revision": 1,
  "name": "Fictional brand list",
  "sourceId": "owned-fixture",
  "grantRevision": 1,
  "status": "draft",
  "pageType": "list",
  "allowedOrigins": ["https://fixture.example"],
  "startUrls": ["https://fixture.example/catalogue"],
  "rootSelector": {"engine":"css","expression":"main .catalogue"},
  "recordSelector": {"engine":"css","expression":".brand-card"},
  "recordKey": "brand_url",
  "fields": [
    {"key":"brand_name","label":"Brand","kind":"scalar","selector":{"engine":"css","expression":".name"},"scope":"record","extract":"text","cardinality":"one","required":true,"valueType":"string"},
    {"key":"brand_url","label":"Detail","kind":"scalar","selector":{"engine":"css","expression":"a.detail"},"scope":"record","extract":"url","attribute":"href","cardinality":"one","required":true,"valueType":"url"},
    {"key":"display_count","label":"Count","kind":"scalar","selector":{"engine":"css","expression":".count"},"scope":"record","extract":"number","cardinality":"optional","required":false,"valueType":"number"}
  ],
  "navigation":{"type":"none"},
  "limits":{"maxPages":1,"maxRows":50,"maxDurationMs":60000,"concurrency":1},
  "validation":{"fixtureSet":"brand-list-v1","expectedRecordCount":20,"state":"not-run"},
  "provenancePolicy":{"retainRaw":true,"retainUrl":true},
  "createdAt":"2026-11-25T00:00:00Z",
  "updatedAt":"2026-11-25T00:00:00Z",
  "approvalRef":null
}
```
Reserved fixture.example is illustrative; actual owned local fixture server is explicitly approved in test policy. Never contact this sample URL as a real source.

## Result envelope
| Field | Meaning |
|---|---|
| schemaVersion | result version |
| datasetId/jobId | local opaque IDs |
| recipeId/revision/hash | exact rule used |
| sourceId/grantRevision | permission evidence reference |
| mode | manual/agent/local-ai (execution deterministic) |
| acquiredAt | UTC time |
| status | succeeded/partial/failed/cancelled |
| counts | pages, matched, valid, missing, ambiguous, duplicates, quarantined |
| records | bounded acquisition records or cursor |
| warnings | stable code, record/field ref, safe message |
| terminationReason | explicit finish reason |
| validationRef | validation report ID |
Record: sourceKey, sourceUrl (sanitized), observedAt, field map, evidence map, validationStatus. Evidence: field path, raw value allowed by grant, normalized value or null, unit/approximate flag, source URL, rule selector ID and revision, observedAt, status observed/missing/ambiguous/invalid/truncated. No AI guess recorded as observed.
Example:
```json
{
  "sourceKey":"fictional-001",
  "fields":{"name":"Example Perfume","accords":[{"label":"citrus","rank":1}],"pyramid":{"top":[{"name":"orange"}],"middle":[],"base":[]}},
  "fieldEvidence":{"pyramid.middle":{"status":"missing","reason":"SECTION_ABSENT","raw":null},"name":{"status":"observed","ruleRevision":1,"raw":"Example Perfume"}},
  "validationStatus":"partial"
}
```
Empty list observed vs missing distinguish in evidence. Export JSON canonical; CSV scalar table plus child tables keyed by sourceKey and ordinal; arrays not zipped. Formula-safe CSV separate from raw JSON.

## Revision lifecycle and validation
draft -> validated after fixture contract tests -> approved only by Owner-bound approval for exact recipe hash/grant/limits -> retired. Any edit creates next revision draft; old jobs keep immutable revision. Policy revocation invalidates use even if approved. Unknown schema version reject; migration explicit conversion with backup and validation report; never silently mutate old job history. Validation errors include field path/code; no remote content in logs. Recipe import treats data untrusted, checks graph acyclic, size/depth limits, allowed origins, selectors, transforms and source policy. Approval and job execution are different from saving draft.

## Contract clarification: draft and normalized forms
The examples are draft input, not final validated or approved objects. Validation fills explicit documented defaults only; normalized output is hashed after canonical serialization. For draft input, validation evidence/hash, approvalRef, optional transform/attribute/children/maxMatches/evidence, navigation-specific limits and nonessential global limits may be absent; defaults must be restrictive and shown in preview. Approved normalized form requires explicit resolved limits, rule hash, fixture evidence and approvalRef. No required field may be implicitly invented. Status values are server-controlled; imported or model-generated recipes always enter draft regardless of claimed status. Reject absent required sourceId, grantRevision, pageType, root selector, fields, recordKey or list recordSelector. The sample uses a fictional draft and is never an executable live-source recipe. Exact default normalization and JSON Schema conformance must be implemented/tested in P2/P3, including rejection of claims of approval from input. See RECIPE and API negative cases.

## Class C compatibility amendment — approval packet pending
This exact documentation payload is PROPOSED, not implementation or source authorization. S11 independent architecture/security review PASS applies to its reviewed design; LOW-5 protected issuance requires the affected-clause check and Owner decision. All28 added cases are NOT RUN. Approved status changes only through Owner merge of the separately authorized Class C unit.

## 3. Two identity layers — do not conflate them

### 3.1 Live document-bound identity

Content maintains a per-document WeakMap from actual Element objects to opaque random tokens.
No token is written into page DOM/data attributes. A scope has its own opaque token and root
node reference. A live selection context contains:

`sourceId, grantRevision, tabId, documentId, documentGeneration, selectionSessionNonce,
scopeToken, candidateRevision, orderedMemberTokens, renderRevision`.

Only the trusted content/broker path issues/resolves tokens. A command must match the current
source/grant, browser tab/document, generation, scope/session and current candidate revision.
Resolve every requested token back to a member Element; recheck root, ordered membership,
visibility/eligibility and allowed operation. IDs copied across tabs/scopes/documents/revisions
are rejected even if their display labels or business keys match. Never authorize by a page-
provided ID, selector string, ordinal or sourceKey alone. Display labels may be safe tag/ordinal
labels; approved preview values require the corresponding inspection field permission.
Retain extension-panel sender validation, the panel's containing-window/current-tab binding,
per-tab serialized command queues and request/response identity matching. Recheck grant and
document context after asynchronous boundaries before publishing success; a stale completion
cannot restore a lease or confirmed view. Existing timeouts, panel heartbeat, worker recovery,
closed-overlay integrity and cancellation cleanup remain mandatory regression controls.

Tokens can fit the current bounded recordId size, but the context fields require a reviewed
versioned message-contract change. Atomically upgrade sender/broker/content validators; reject
legacy grant-less messages and unknown versions/fields. Do not default missing security fields.

### 3.2 Reproducible durable key and locator

Store a versioned identity specification in a recipe, not a live node token. Prefer, in order:

1. A unique approved semantic/business key attribute or field, with documented normalization.
2. A unique canonical approved href/key combination, within the source URL/field policy.
   Unknown/sensitive queries, userinfo and unsafe schemes are rejected, not silently stripped
   in a way that creates collisions.
3. An explicitly reviewed fingerprint of approved key fields plus structural descriptors.
   It is a candidate locator only. Generated classes and positional indexes are not stable
   keys; hashing values does not anonymize them or create read/storage permission.

Namespace a durable key by source identity and identity-spec/recipe revision. Canonical JSON
and a specified hash/normalization version can make representation reproducible, but uniqueness
must be checked on the actual reviewed scope. Preserve collision/ambiguity results; do not
append an ordinal and call an identical record stable. Saved key/locator data is untrusted
input, size-bounded and subject to the recipe/source policy.

Across reloads, resolve the saved scope/locator under a new toolbar authorization and current
grant/recipe revision. Check cardinality, approved key values and unique membership, then issue
new live tokens. The Owner reviews the new selection before any saved intent is applied.
Any required recipe identity fields need an explicitly approved schema/version migration; do not silently widen the existing strict recipe major. Imports/migrations enter draft, require validation and fresh hash/grant-bound approval, and never carry old approval forward.
No silent cross-document rebinding or selector broadening. Changed grant/recipe/identity specs
invalidate old approvals even if the logical business key remains equal.

### 3.3 Identical records and DOM replacement

- Identical records without a unique approved key are distinct only by live node identity in
  the current document. They can be excluded within that live scope, but an individual one
  cannot be reliably replayed after reload/reorder. Mark non-replayable/ambiguous and require
  fresh selection. Never promise semantic distinction not present in authorized data.
- A cloned/replaced node always gets a new live token, even if markup and key text match.
  Current Lock/Confirm and incompatible history are invalidated; explicit new review is required.
- Virtualizers may recycle the same Element for another record. Relevant text/attribute/key
  mutations advance renderRevision and invalidate the active reviewed snapshot. WeakMap node
  identity alone is insufficient to permit old actions. Keep the existing conservative
  subtree mutation stop until any more selective reconciliation is approved and tested.
- New/removed/reordered members are not silently merged into a locked set. Current count is
  mounted DOM count only; no full-dataset/pagination total is inferred.

## 4. Exclusions and Undo

Live exclusion is a set of opaque member tokens bound to the selection context. Include and
Exclude use the current revision and a resolved current member; they do not use array index,
CSS class or a synthetic data-record-id. Enforce the existing minimum-valid-selection rule.
Every accepted edit advances the monotonic candidate revision and clears Confirmed.

Undo snapshots contain context, scope/root token, ordered member tokens, selected scope,
excluded tokens and render revision, with a bounded stack (retain current30-entry bound unless
separately changed). Undo is an explicit current-context action. It may restore a prior valid
Parent/Child scope only if that snapshot's actual nodes/root remain valid in the same current
document/grant/session. Restore selection data, **not** an old document generation, permission,
grant revision or candidate revision; issue a fresh revision. Incompatible snapshots are
discarded/rejected with a clear message. Navigation, revocation, replacement and stale
membership cannot be undone into authorization or confirmation.

Persistent exclusions are opt-in recipe intent keyed by a validated durable key specification.
On a later document, resolve uniquely and show a preview, then require an explicit Apply saved
exclusions action. Missing/duplicate/changed keys stop replay; no positional fallback. Storing
source-derived key values requires storage permission, not merely permission to save local
configuration. No generic persistent exclusion replay for indistinguishable records.



Recipes bind exact source/grant revision, document/selection and approved recipe hash. Imported approval is never authority. v1→v2 grant/protocol migration returns incompatible recipes to draft and invalidates pending approvals; fresh browser/document review required. Before extraction/storage/export revalidate rights, current wall clock and context after awaits and before effects.

## S13 browser-scope amendment — pending Owner approval/merge

Google Chrome on Windows is the proposed sole mandatory native browser. Brave obligations
are proposed OUT OF SCOPE, not PASS; prior observations remain historical. This is effective
only on Class C Owner merge. All applicable source/security/lifecycle/accessibility controls
remain. Shared Chromium-compatible runtime is preserved. See [[Browser_Support_Policy]].
The Class C unit is independent of001 acceptance and must become effective first; it is not
blocked on001 acceptance/merge. Later integration into001 requires separate explicit authority,
preserved history/source/build/evidence and then remaining mandatory Chrome acceptance.
