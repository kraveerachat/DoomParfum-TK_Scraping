# Visual picker and deterministic extraction algorithms
Design-level algorithms for generic permitted DOM. No site-specific selectors, anti-bot mechanisms, or hidden data harvesting. Fixtures contain fictional catalogue layouts with known expected output.

## Input and record model
DOM adapter exposes bounded visible element snapshot: tag, approved stable attributes, parent/children, text sample, link/image reference when policy allows, viewport box, visibility, element reference scoped to current document generation. Avoid full document HTML and sensitive inputs. Element refs expire on document replacement or incompatible DOM mutation.
Record group is a scoped container plus record selector; one matched element is one record. Field selector relative to record/section root. Distinguish list group from singleton detail section. No global field query that can mix adjacent product data.

## Group detection
1. Capture hovered element via event path within permitted document; ignore overlay and extension nodes, script/style/input/form/control not eligible for data extraction by default.
2. Build bounded ancestor candidates (initial proposed max depth 8) and nearby child/sibling sets. For each candidate, compute structural signature for each visible child: tag, stable class/attribute tokens after removing clearly dynamic tokens, shallow descendant tag pattern, semantic roles. Never compare text equality alone.
3. Group similar signatures; score repeated count, structural similarity, field coverage, containment, visible item proportion and distractor penalty. Proposed initial weights: similarity 0.35, coverage 0.25, containment 0.20, repeat evidence 0.10, visibility 0.10; penalty up to 0.30 for unrelated siblings. Tune using fixture set, not assume perfect. Candidate must include hover target or explain why parent proposed.
4. Candidate count >=2 for repeated list. Table rows recognized by semantic table/tr structure, exclude header/footer by scope. Grid cards recognized structurally independent of CSS visual layout.
5. Rank bounded candidates, display top candidates and exact matched count, confidence category, scope breadcrumb. If close scores or missing structure, show manual selection rather than force a match.
6. Lock user-confirmed group; generate scoped selector, verify actual match set equals selected set on current document. Persist selection rules only if reproducible; ephemeral exclusion cannot become approved recipe.
7. Validate across multiple synthetic layouts: 20 similar cards, unrelated sidebar cards, class variations, missing fields, nested repeated components, table rows, list with ad-like distractor. Exact expected groups measured. No assertion generalizes to all sites.

## Selector strategy
Prefer unique stable IDs only when stable across page templates; semantic tags/roles, stable data attributes allowed by policy, stable class combination and scoped parent-child relation; avoid generated IDs, long absolute paths and nth-child unless documented and verified. Escape CSS tokens using standards-compatible escaping. CSS primary, bounded XPath optional only after compatibility review; do not execute arbitrary code or selectors outside authorized root. Store alternatives with priority and match expectation; if first fails, alternatives may be tried only within same approved scope and validation policy. Never broaden to whole page as silent repair.
Scoring proposal: uniqueness/cardinality correctness, cross-fixture stability, selector length and structural resilience; use deterministic tie-break. Report why chosen. For multiple matches expected singleton: AMBIGUOUS, not first match silently. For list field expected many: all matching within scoped root, bounded count. For required zero: MISSING_REQUIRED; optional zero: null/missing reason.

## Field discovery
- Sample up to 10 candidate records; enumerate visible leaf text, links and image URLs within each record, grouped by consistent relative path and semantic label.
- Infer suggested column name from nearby labels/semantic attributes; sanitize key and ensure unique stable key. User edits name/type. No inference of facts absent from DOM.
- For each candidate compute coverage (records with nonempty value / sampled records), cardinality and type consistency. Distinguish raw text and parsed numeric/date; no automatic coercion of unknown units.
- Repeated child structures become nested arrays only when grouping is explicit; do not cross-join unrelated children.
- Preview example raw/normalized values, warnings, selector, scope and extraction kind. User chooses required/optional.
- Detail mode uses separately selected scopes, each as singleton object or repeated list. A field outside section fails validation. A section missing on one page remains missing; no borrowing from other sections.

## DOM reading rules
Use rendered visible text (normalize whitespace only as configured), permitted href/src/currentSrc attributes, optionally explicit title/alt/aria-label as approved display metadata. Resolve relative URLs against actual document URL and validate schemes; never expose javascript/data/file or private query secrets. Images references only by default. Ignore password inputs, hidden form fields, cookies, localStorage, sessionStorage, browser tokens, arbitrary script content. Script JSON extraction not part of baseline; separate reviewed feature if rights allow.
Number parse stores raw display, normalized numeric when unambiguous, unit, locale/approximate flag. Example '2.3k' raw + approximate 2300 only under explicit transform; no exact vote count fabricated. Width of accord bar is visual ratio only, not a claimed percentage. Date parsing must preserve unknown precision. Text truncation recorded.

## Extraction execution
1. Check policy, grant revision, tab/document generation, recipe validation and limits.
2. Resolve root and record group; cardinality checks. Compute match count and budget before extracting.
3. For each record, build stable source key from approved identifier/URL or documented deterministic fallback; collisions quarantine rather than overwrite.
4. Walk fields tree depth-first with scoped queries; enforce node/array/text limits and timeout. Add per-field evidence and missing status.
5. Validate record types/cardinalities; preserve raw acquisition separately from normalized value. Invalid records go to quarantine with reason.
6. Deduplicate by source key and recipe revision; report repeated records and conflicting observations separately.
7. Persist atomic dataset or checkpoint; emit counts and reason. No claim success if required fields invalid or limit reached (partial when appropriate).
8. UI renders data as text; model receives only minimized, allowed preview.

## Failure handling
STALE_DOCUMENT: clear proposal, user reselects. SELECTOR_DRIFT: pause and require review; do not automatically re-run across larger scope. AMBIGUOUS_MATCH: require field correction. MISSING_REQUIRED: record quarantine, no fabricated value. LIMIT_REACHED: partial with counters. POLICY_DENIED: stop before DOM/network action. CANCELLED: cleanup and preserve committed observations if permitted. Storage quota: report error and allow local export of valid retained data where possible.
Performance: cap snapshot nodes and candidate comparisons; throttle hover events with animation frame; no mutation observer full-page scan on every mutation; invalidate relevant subtree only and disconnect on cleanup. Benchmark fixture 100/1000 cards with measured latency and memory; tuning targets proposed in acceptance matrix. Frame and open shadow DOM later explicit capability; closed shadow DOM inaccessible, no workaround.

## Class C compatibility amendment — approval packet pending
This exact documentation payload is PROPOSED, not implementation or source authorization. S11 independent architecture/security review PASS applies to its reviewed design; LOW-5 protected issuance requires the affected-clause check and Owner decision. All28 added cases are NOT RUN. Approved status changes only through Owner merge of the separately authorized Class C unit.

## 2. Source-agnostic group strategy

1. Owner chooses a visible scope on an authorized document. Inspect only permitted top-frame
   DOM and approved preview metadata. No full-page secret/hidden-field capture.
2. Generate bounded candidates from repeated structural relationships: tag/role, immediate
   field shapes, semantic attributes explicitly allowed as anchors, parent/child containment
   and visible geometry. Do not depend on #catalogue, .card, perfume text, p01 or data-record-id.
3. Standard tables use tbody rows with header/footer separation. Grid and Flexbox are visual
   arrangements of records, not record identity. Support repeated div/article/section/li and
   other approved record tags when they form a coherent scoped set. Preserve DOM order as the
   canonical order; CSS visual reordering is disclosed, not silently converted to identity.
4. Optional fields and inconsistent child shapes influence confidence/coverage, not invented
   data or an unreviewed wider root. Heterogeneous templates/ambiguous roots require explicit
   scope/variant selection. Do not promise arbitrary non-sibling content as one group.
5. Root-scoped CSS selectors may use bounded semantic/structural alternatives. Generated CSS
   classes and unstable HTML IDs are excluded from default durable anchors. A source-specific
   stable attribute may be opted into with validation; no arbitrary JS, XPath expansion,
   unbounded alternatives or implicit document fallback.
6. Show exact current member count, scope and ambiguity before Lock/Confirm. Verify resolved
   membership equals the reviewed ordered node set. A selector string is a locator, not proof
   that the same records still exist. Keep existing bounds until a separate measured decision.

Missing HTML IDs or generated classes do not prevent live selection: node-bound tokens below
identify actual elements. Persistent selector reproducibility still needs a unique valid
scope and identity contract; inability to find one is reported explicitly.

### 2.1 Picker membership versus extraction rows — LOW-3

R02.06 owns repeating-member identity/count/scope and a validated confirmed selection. Existing
R03.01 owns one extraction row per confirmed member and the separate field/schema/value contract.
Picker success never proves field values or coverage. A strict SelectionRef carries source/grant,
tab/document/generation, scope/session, candidate/render/confirmed revision and ordered member
tokens. It is a validated reference, not permission to extract. An explicit extraction request
adds current recipe/field schema and inspect+extract capabilities; the engine revalidates all
bindings and resolves only confirmed member roots through the authorized DOM adapter. Storage/
export remain separate gates. No global fallback, row association inference or raw DOM handles
crossing untrusted messages. Dynamic/replaced membership invalidates the handoff before use.

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



### Optional compatibility variants — not universal support requirements

| Structure | Classification | Bounded proposed expectation / case |
|---|---|---|
| display:contents wrappers | OPTIONAL | COMP-01/02 diagnostic variant; absent wrapper geometry yields explicit supported descendant-scope behavior or declared unsupported/ambiguous result, never fabricated outline accuracy |
| HTML table with omitted authored tbody but parser-created tbody | MANDATORY within existing standard-table case | COMP-03 inspects the actual normalized DOM; ordinary syntax omission is not an excuse to miss body rows |
| DOM-script-created direct tr children of table without tbody | OPTIONAL | COMP-03 negative/unsupported variant; do not insert wrappers or silently include headers to claim compatibility |
| rowspan/colspan | OPTIONAL | COMP-03 bounded membership/coverage variant; generalized merged-cell expansion/value propagation is DEFERRED pending explicit extraction design |
| dl/dt/dd multi-node logical records | DEFERRED | Requires association/range-record design beyond one repeated record Element; no automatic dt/dd pairing or cross-join claim |
| Heading-sibling record ranges | DEFERRED | Multi-node boundaries need separate reviewed scope contract, not ordinal grouping hidden in identity |
| Frames/deep or closed shadow DOM | LIMITED/DEFERRED | Existing top-frame/ordinary-DOM contract unchanged; no closed-root or cross-frame bypass |

Variants add no standalone case IDs or mandatory universal-website contract. Record applicability,
unsupported results and future decisions honestly; optional support is not a fabricated PASS.



Extraction rechecks a validated current SelectionRef and separate field/recipe/grant capabilities before reading fields. R02.06 membership and R03.01 rows/values have independent gold. No broad locator fallback or invented full-dataset count.

## S13 browser-scope amendment — pending Owner approval/merge

Google Chrome on Windows is the proposed sole mandatory native browser. Brave obligations
are proposed OUT OF SCOPE, not PASS; prior observations remain historical. This is effective
only on Class C Owner merge. All applicable source/security/lifecycle/accessibility controls
remain. Shared Chromium-compatible runtime is preserved. See [[Browser_Support_Policy]].
The Class C unit is independent of001 acceptance and must become effective first; it is not
blocked on001 acceptance/merge. Later integration into001 requires separate explicit authority,
preserved history/source/build/evidence and then remaining mandatory Chrome acceptance.
