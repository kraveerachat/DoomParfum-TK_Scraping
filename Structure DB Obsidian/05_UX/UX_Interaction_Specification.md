# Visual workbench — interaction and state specification
Original UI using reference screenshots for interaction concepts only. Design for Google Chrome on Windows side panels approximately 320–480 CSS px; responsive at 280 px where possible, larger standalone data tab. Actual dimensions tested after install. Light/dark system theme, accessible contrast, restrained accent; color alone not sole status indicator.

## Navigation and persistent shell
| Screen | Entry | Contents | Primary action | Empty/error state |
|---|---|---|---|---|
| Home | Toolbar action / side panel | Logo, active origin, permission/connection status, tool cards | Choose List or Detail | No permitted tab: explain and disable picker |
| List | Home > List | Select group, fields, preview, load strategy, run | Preview then Extract | No group/matches: choose ancestor/manual |
| Detail | Home > Detail | Sections tree, field inspector, preview JSON | Add section / save draft | Missing required fields shown, no valid run |
| Recipes | Top navigation | Name, revision, type, source, validation, actions | New / open / import | No recipes: create from permitted fixture |
| Data | Top navigation / completion | Dataset list, row counts, provenance, warnings | Open standalone table | No rows: distinguish never run from zero matches |
| Jobs | Top navigation | Queue, state, budgets, counters, reason | Cancel or open result | Offline: last known stale state labelled |
| Settings | Gear | Pairing, source registry, local AI, storage, logs | Save validated settings | Invalid URL/token/model, no implicit network |
| Standalone table | Data > Open | Grid, filters, detail drawer, export, validation | Export selected/all | Missing dataset: recover or return to Data |

## Picker interaction
1. User clicks extension toolbar, selects tool. Extension checks tab identity and source policy before content script injection.
2. Start selection: show mode indicator and Esc instruction. Record tab ID, document identity, origin, selection generation; no background all-tabs scan.
3. Pointer enters page: candidate geometry computed relative to viewport; display solid parent border, dashed record borders, count and confidence. Do not modify host layout; overlay root separated from page style. Limit overlay rendering for huge groups; count still calculated within budget.
4. Candidate group list: user can move up/down ancestor with controls or keyboard, inspect counts, toggle singleton mode. Repeated group found only if matching scope passes threshold; otherwise propose manual selection.
5. Click locks candidate, prevents normal link/navigation only while picker active. On unsupported frame or changed document, stop and explain.
6. User may exclude irrelevant candidate or choose narrower group; recipe stores a reproducible filter, not ephemeral ordinal exclusion. If not reproducible mark draft invalid.
7. After selecting fields, show sample values and null/ambiguous warnings. Required fields and duplicate keys block approval; rename or change type without changing original acquisition evidence.
8. Preview is bounded; show sample count versus total matched count, truncation and rule revision.
9. Extract button enabled only for valid source grant, current document, valid recipe and explicit confirmation. Preview mismatch after navigation invalidates selection.
10. On complete, dataset summary shows rows, warnings, source, time, open data button; cancel returns to safe idle; release listeners and overlays.

## Picker state machine
| State | Event | Next | Behavior |
|---|---|---|---|
| idle | START with policy pass | selecting | inject once, register listeners |
| idle | START denied | error | no injection / clear reason |
| selecting | hover | selecting | update bounded candidates |
| selecting | confirm group | locked | freeze candidate and document ID |
| selecting | Esc/close/tab switch | idle | cleanup |
| locked | add field | field-select | scope to locked record or section |
| field-select | confirm | locked | validate field and sample |
| locked | preview | previewing | acquire bounded sample |
| previewing | valid response | preview-ready | show actual data and warnings |
| previewing | timeout/document change | error | discard stale response |
| preview-ready | edit | locked | invalidate old preview |
| preview-ready | extract approved | extracting | recheck source and generation |
| extracting | done | result | persist dataset, remove picker |
| extracting | cancel | result or idle | label partial, do not claim complete |
| any active | navigation/revoke | error | cleanup, clear authorization |
| error | dismiss/restart | idle | no retry without appropriate checks |

## List extractor layout
- Step 1 Select List: button, candidate count, scope breadcrumb, selector display, change/reselect, include/exclude.
- Step 2 Fields: editable name, extraction kind, relative scope, sample, coverage, required flag; detect common text/link/image URL; no hidden form data.
- Step 3 Preview: rows and columns, actual count, null highlights, invalid/duplicate badge, explicit consent to persist.
- Step 4 Load More: None in P1; Pagination, Load More, Auto-scroll enabled only when P4 installed/paired and permitted. Explain disabled state, select control via scoped picker.
- Footer: primary Start extraction, stop/cancel, status, open table, save recipe. No cloud paywall or advertised fake button.

## Detail extractor layout
- Section tree with Add section, Rename, Remove, reorder display, kind Scalar/Object/List.
- Each section specifies root selector, cardinality, required flag, nested fields, per-field preview.
- Example page fixture: Hero object, Accords list, Ratings object + distribution list, Wear list, Pyramid object with Top/Middle/Base lists, optional performance.
- Field inspector displays observed value, raw/normalized representation, evidence URL, missing reason, ambiguity count, and selector alternatives.
- Array previews show independent row groups; do not flatten every section by index. Root detail record ID connects related children.
- JSON preview rendered as text; export table tab with separate child table view; no arbitrary HTML rendering.
- Recipe version saved as draft, validation summary and differences required before approval.

## Data window
Header: dataset name, mode, source origin, recipe revision, job ID, observed time, row count, validation count, truncation status. Toolbar: search, filters, sort, visible column controls, selected row export, export all, validation/quarantine, delete confirmation. Row drawer: raw/normalized fields, field evidence, nested arrays, missing/ambiguous warnings, open source link only on explicit user action. Local-only preview; remote thumbnails default disabled unless allowed by policy and consent. CSV warnings distinguish formula-neutralized cells from raw JSON.
Editing column mapping creates derived view/export, never overwrites original observations. Deletion warns if dataset referenced by export manifest; delete local data only with user confirmation. Job output may outlive browser tab; no dependency on injected script.

## Agent UX
Show disconnected/paired/leased/running/revoked; active origin and lease expiry; Agent proposal panel with recipe diff, highlighted candidates and confidence; Accept/Reject/Edit. Display exact scope, approved revision, max pages/rows/time, source status before run. Cancel/revoke immediately disables further commands; do not hide agent mode or silently execute new scope. Agent-generated values never labelled verified until deterministic preview. LM Studio settings shows model ID, capability status, last error, budgets and manual fallback.

## Accessibility and usability acceptance
Keyboard navigation, visible focus, labelled controls, Esc cancel, focus restored after picker, reduced motion, no color-only alerts, semantic table headers, screen-reader count announcement throttled. Test browser zoom 80/100/125%, resize, scroll, multiple tabs, keyboard-only workflow, dark/light, UTF-8 labels. Empty states, denied source, daemon offline, storage full, invalid recipe, zero matches and partial export all have explicit messaging. No copied panda logo, copyrighted site imagery or original extension assets.

## Class C compatibility amendment — approval packet pending
This exact documentation payload is PROPOSED, not implementation or source authorization. S11 independent architecture/security review PASS applies to its reviewed design; LOW-5 protected issuance requires the affected-clause check and Owner decision. All28 added cases are NOT RUN. Approved status changes only through Owner merge of the separately authorized Class C unit.

## Authorization-state projection and protected Sources behavior
## 8. Proposed user-facing states

Use typed authorization/deny reasons, not substring matching of error messages. The following
is a proposed UI projection/contract change for002.12; it does not modify current P1a enums.

| State | Entry/meaning | Enabled behavior and exit |
|---|---|---|
| Idle | Source inspection and actual tab lease verified; no active selection | Start available; do not show readiness before STATUS establishes it |
| Awaiting Authorization | Typed reason=checking before authoritative source/tab status; or reason=browserGesture/leasePending when required authorization is absent | Checking has no privileged actions and is not an eighth state; explain gesture/pending step; validated status can lead Idle |
| Source Not Authorized | Current source/path/capability required for inspection is pending/denied/expired/revoked | Show reason and Source Management review path; no DOM access; approval still requires fresh toolbar activation |
| Selecting | Valid current context and reviewed candidate preview | Scope/filter/reselect/cancel allowed; Confirm disabled until trusted lock |
| Locked | Current exact member snapshot is frozen | Explicit review/Confirm; any accepted edit invalidates confirmation and advances revision |
| Confirmed | Explicit current-context confirmation succeeded | Confirmation is not permission for extraction/storage/export; document/grant/member/edit change clears it |
| Runtime Error | Actual transport/timeout/injection/protocol failure or unexpected safety failure | Specific safe recovery message, no stale success or implicit reauthorization |

Exactly one checking model: **Awaiting Authorization with reason=checking**. Initial pending
STATUS renders “Checking current source and tab authorization…” and enables no privileged
operation. It does not assert that the source is approved or the tab authorized. Resolution
is Idle only after valid readiness, Source Not Authorized for policy denial/expiry/revocation,
Awaiting Authorization with a specific gesture/lease reason for normal prerequisites, or
Runtime Error for actual transport/injection/protocol failure. No eighth Checking state and
no message-substring authority. UX-01/02/03 assert these exact transitions/semantics.

Expected DOM drift clears confirmation and unsafe state with a reselection notice; denial of
a storage/export capability is action-specific and does not fabricate inspection denial or
permission success. No old response/grant/selection survives expiry or revision changes.

Historical Brave authorization-label observations remain in immutable reports. Proposed002.12 fixes shared typed readiness presentation and tests UX-02 in Chrome on Windows; no Brave-specific investigation or new deferral. A demonstrated mandatory001 defect would require separate
scoped authorization; none is established by the existing message alone.



Sources UI shows pending requests and protected Fixture/pilot grants read-only. It cannot administer/approve/expand/extend protected grants, including through automated UI workflows. Event trust is not Owner identity. The protected pilot issuance route is reviewed configuration plus exact packet/Owner decisions, not an enabling checkbox. UX-01–04 and AUTH-04/09/10 remain NOT RUN.

## S13 browser-scope amendment — pending Owner approval/merge

Google Chrome on Windows is the proposed sole mandatory native browser. Brave obligations
are proposed OUT OF SCOPE, not PASS; prior observations remain historical. This is effective
only on Class C Owner merge. All applicable source/security/lifecycle/accessibility controls
remain. Shared Chromium-compatible runtime is preserved. See [[Browser_Support_Policy]].
The Class C unit is independent of001 acceptance and must become effective first; it is not
blocked on001 acceptance/merge. Later integration into001 requires separate explicit authority,
preserved history/source/build/evidence and then remaining mandatory Chrome acceptance.
