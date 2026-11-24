# Visual workbench — interaction and state specification
Original UI using reference screenshots for interaction concepts only. Design for Chrome/Brave side panels approximately 320–480 CSS px; responsive at 280 px where possible, larger standalone data tab. Actual dimensions tested after install. Light/dark system theme, accessible contrast, restrained accent; color alone not sole status indicator.

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
