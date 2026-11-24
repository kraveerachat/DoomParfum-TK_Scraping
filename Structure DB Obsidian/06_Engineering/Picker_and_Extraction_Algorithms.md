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
