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
