# Verification and acceptance matrix
All test cases are PLANNED / NOT RUN. No runtime exists at P0. Tests below are planned obligations, not scripts or claimed passes. Every case has stable ID, linked requirement and task. Security tests are repeated in applicable phases. Independent reviewer checks actual test semantics and code at exact HEAD. Use owned synthetic fixtures only; deny-policy tests must not contact prohibited sites.

## Fixture inventory and gold outputs
| Fixture | Purpose | Expected oracle |
|---|---|---|
| F01 brand grid | 20 fictional cards + unrelated sidebar | exact 20 source keys and fields |
| F02 table | header/footer, data rows, missing field | exact body rows and missing reason |
| F03 nested list | repeated child inside cards | correct group scope; no cross-join |
| F04 singleton | only one visible record | no false repeat group |
| F05 mutation | dynamic class and DOM replacement | stale selection invalidation |
| F06 perfume detail | hero, accords, votes, wear, pyramid, optional performance | exact nested JSON |
| F07 variant detail | missing optional, duplicate singleton, changed layout | missing/ambiguous/drift statuses |
| F08 pagination | 3 pages with duplicate key and disabled next | exact unique count and terminal reason |
| F09 load/scroll | append and no-progress cycle | bounded stop and exact rows |
| F10 list-to-detail | permitted edges + denied edge + one error | exact relationships and partial status |
| F11 security | hostile input/recipe/DOM/CSV/redirect mock | denial before unauthorized effect |
| F12 storage | crash/restart, idempotency, quota, tampered manifest | atomicity and validation |
| F13 model | gold mapping, malformed/ref hallucination/injection | safe rejection and proposal score |
| F14 performance | 100 and 1000 generated fictional records | measured latency/memory, exact rows |
| F15 operations | clean profile, migration/rollback, uninstall | reproducible supported setup |
All fixture contents generated independently; no third-party source capture, real personal information, credentials or copyrighted assets. Expected values authored independently of implementation to prevent tests simply echoing output. Fixture server explicit narrow local grant; tests never resolve live blocked domain.

## Test design and execution
Unit pure contracts/picker/normalization, integration stores/API/MCP, browser E2E synthetic fixture, actual Windows Chrome/Brave smoke, actual Codex/Claude/LM Studio smoke separately, security negative controls. Test data and clock where possible deterministic. Use fake time for expiry and bounded retries, actual browser navigation for egress controls, actual DB restart for recovery. Do not mock away the security boundary under test. CI fixture-only. Exact scripts created in relevant implementation tasks; P0 no runtime.

## EXT — task 001/002
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| EXT-01 | R01.01 | Load unpacked in clean Chrome Windows profile | Toolbar/side panel opens; version recorded |
| EXT-02 | R01.01 | Load unpacked in clean Brave Windows profile | Actual side panel or documented approved fallback; no assumed support |
| EXT-03 | R01.02 | Daemon/model not running | Home, manual tool usable; connection marked offline |
| EXT-04 | R02.01 | Hover fixture card; scroll and resize | Box tracks geometry; host layout unchanged |
| EXT-05 | R02.02 | 20-card grid plus unrelated sidebar | Exactly 20 gold keys; unrelated group excluded |
| EXT-06 | R02.03 | Nested child and ancestor candidates | Candidate count/confidence shown; explicit choice |
| EXT-07 | R02.04 | Esc/cancel after start | Overlay and listeners removed; normal click restored |
| EXT-08 | R02.04 | Repeat start/stop and reselect | No duplicate listener or stale overlay |
| EXT-09 | R02.05 | Navigate/tab switch during selection | Stale proposal denied; no next-tab injection |
| EXT-10 | R01.04 | Suspend/restart service worker, reopen panel | Restored safe state or explicit reset |
| EXT-11 | R13.01 | Denied origin/revoked permission | No DOM access; user sees reason |
| EXT-12 | R02.04 | Keyboard selection, focus, dark/light and zoom | Reachable controls, Esc and focus restore, readable labels |
| EXT-13 | R04.01 | Open data view then close source tab | Persisted data still available |
| EXT-14 | R04.02 | Search/sort/filter and drilldown | Correct display, original evidence unchanged |
| EXT-15 | R04.05 | Delete dataset confirmation/abort | No deletion until confirmation; local only |
| EXT-16 | R03.03 | Zero matches, quota failure and offline state | Explicit error/no false success |

## ALG — task 001/002
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| ALG-01 | R02.02 | Grid with same child tags but sidebar distractors | Selected root and exact record IDs match gold |
| ALG-02 | R02.03 | Single record page | No false repeat confidence; manual alternative |
| ALG-03 | R02.02 | Semantic table with header/footer | Only body rows included |
| ALG-04 | R02.03 | Nested list within record | Outer/inner candidates distinguished |
| ALG-05 | R02.05 | DOM mutations after selection | Generation invalidation or explicit revalidation |
| ALG-06 | R03.02 | Stable relative class/attr and dynamic classes | Stable selector preferred, verified match set |
| ALG-07 | R03.02 | Field labels and coverage across records | Correct inferred columns and missing flags |
| ALG-08 | R03.03 | Two matches for singleton field | AMBIGUOUS; no silent first value |
| ALG-09 | R03.03 | Missing required/optional | Quarantine required; optional null reason |
| ALG-10 | R03.04 | URL relative and unsafe schemes | Approved URL normalized; invalid scheme rejected |
| ALG-11 | R03.04 | Hidden form/password and script data | No extraction or disclosure |
| ALG-12 | R03.03 | Oversized DOM and field text | Bounded processing; truncation reported |

## DATA — task 002/003/009
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| DATA-01 | R04.04 | UTF-8 Thai and Unicode names | JSON/CSV round-trip values |
| DATA-02 | R04.04 | Comma, quote, newline in cells | CSV correctly quoted |
| DATA-03 | R13.04 | Spreadsheet formula prefixes | CSV neutralized; raw JSON separate if allowed |
| DATA-04 | R04.02 | Rename/sort/filter view | Original raw observations unchanged |
| DATA-05 | R04.05 | IndexedDB restart and quota | Persistence; storage error explicit |
| DATA-06 | R14.01 | Atomic export and file hashes | Manifest matches every file/count |
| DATA-07 | R14.01 | Tampered hash/manifest version | Reject import before write |
| DATA-08 | R14.01 | Selected rows export | Selection and original total labelled |
| DATA-09 | R12.03 | Observed/missing/empty/ambiguous | Evidence states preserved distinctly |
| DATA-10 | R12.02 | Independent child arrays | No cross-join; correct parent key/ordinal |

## DETAIL — task 003
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| DETAIL-01 | R05.01 | Hero scalar/object | Exact name, brand and optional attributes |
| DETAIL-02 | R05.01 | Accords section list | Independent array and order |
| DETAIL-03 | R05.01 | Ratings and sentiment distribution | Score/scale/count separate; raw preserved |
| DETAIL-04 | R05.01 | Season/day/night panel | Category and votes independent |
| DETAIL-05 | R05.01 | Top/middle/base pyramid | Three nested arrays and layer mapping |
| DETAIL-06 | R05.03 | Missing optional performance | Null/missing reason not fabricated |
| DETAIL-07 | R05.03 | Missing required name | Record quarantine |
| DETAIL-08 | R05.02 | Same labels across unrelated sections | Scoped extraction only |
| DETAIL-09 | R05.03 | Duplicate singleton | Ambiguous, no silent selection |
| DETAIL-10 | R05.05 | Variant fixture with layout change | Approved alternative or drift warning |
| DETAIL-11 | R05.04 | Section edit and preview | Old preview invalidated; correct new shape |
| DETAIL-12 | R05.04 | JSON tree and child CSV view | Gold arrays preserved |

## RECIPE — task 003/004
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| RECIPE-01 | R06.01 | Save draft/reopen | Exact revision/hash persisted |
| RECIPE-02 | R06.03 | Edit approved revision | New draft; old jobs unchanged |
| RECIPE-03 | R06.02 | Unknown major/extra keys | Reject |
| RECIPE-04 | R06.02 | Arbitrary JS/transform/unsafe selector | Reject |
| RECIPE-05 | R06.02 | DAG cycle/depth/fanout excess | Reject |
| RECIPE-06 | R06.03 | Scope or grant revision changed | Approval invalidated |
| RECIPE-07 | R06.03 | Fixture validation evidence absent | Not approved |
| RECIPE-08 | R06.02 | Import/export round-trip | Canonical semantic equality |

## API — task 004
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| API-01 | R08.01 | Start daemon, inspect listening sockets | Only 127.0.0.1; no public bind |
| API-02 | R08.01 | Port conflict | Explicit error; no silent alternate public interface |
| API-03 | R08.01 | Unauthenticated privileged call | 401/deny, no state change |
| API-04 | R08.01 | Hostile origin/Host | Reject before side effect |
| API-05 | R08.01 | Pair once, replay challenge | First authorized; replay denied |
| API-06 | R08.01 | Expired/revoked token | Denied, lease invalidated |
| API-07 | R09.01 | Wrong tab/document/lease | Denied, no DOM response |
| API-08 | R08.03 | Invalid/oversized input and unknown keys | 400/413; safe error |
| API-09 | R09.01 | Agent tries owner approval route | 403, no approval created |
| API-10 | R08.02 | SQLite migration rollback | Original DB restorable |
| API-11 | R08.03 | Bounded cursor/result size | Correct page and truncation/cursor |
| API-12 | R08.02 | Restart/persistence | Recipes and datasets remain consistent |

## JOB — task 004/005
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| JOB-01 | R08.02 | Persist queued job before worker | Atomic queue/audit |
| JOB-02 | R08.02 | Same idempotency key and payload | Same job ID |
| JOB-03 | R08.02 | Same key different payload | Conflict denied |
| JOB-04 | R08.04 | Expired approval/grant at admission | No job execution |
| JOB-05 | R07.05 | Normal fixture job lifecycle | Queued-running-succeeded counters correct |
| JOB-06 | R07.05 | Cancel while queued | Cancelled, no context opened |
| JOB-07 | R07.05 | Cancel during navigation/extraction | No new request after cancel; owned context closed |
| JOB-08 | R07.04 | Duplicate records across pages | Dedup count and unique keys exact |
| JOB-09 | R07.04 | Crash before checkpoint commit | No partial transaction |
| JOB-10 | R07.04 | Crash after checkpoint commit | No duplicate on approved resume |
| JOB-11 | R08.04 | Stale worker lease | Write rejected |
| JOB-12 | R08.04 | Budget/time limit | Partial/stop reason; no excess actions |
| JOB-13 | R07.05 | Detail page error with other valid rows | Partial dataset and failed detail count |
| JOB-14 | R08.02 | Storage full/transaction failure | No corrupt success; clear failure |
| JOB-15 | R08.04 | Restart interrupted live-like fixture job | Paused/partial; no automatic replay |

## NAV — task 005
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| NAV-01 | R07.01 | Three synthetic next-button pages | Exact gold records and page count |
| NAV-02 | R07.01 | Next disabled | Stop correctly; no click |
| NAV-03 | R07.01 | Cycle to previous page | Cycle stop reason |
| NAV-04 | R07.02 | Load-more appends new rows | Unique keys and exact count |
| NAV-05 | R07.02 | Load-more no new records | Bounded no-progress stop |
| NAV-06 | R07.02 | Scroll fixture with lazy items | Bounded actions, no infinite loop |
| NAV-07 | R07.03 | List-to-detail permitted edge | Exact relationships and detail arrays |
| NAV-08 | R07.03 | Disallowed edge/redirect | No request to denied target |
| NAV-09 | R07.04 | Repeated detail link | Dedup or distinct observation according to key |
| NAV-10 | R08.04 | Synthetic 401/403/429/challenge | Stop; no evasion/retry loop |

## MCP — task 004/005/007
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| MCP-01 | R09.01 | Initialize and list tools over stdio | Supported protocol; exact tool list |
| MCP-02 | R09.01 | Malformed tool input/unknown field | Structured error, no side effect |
| MCP-03 | R09.01 | Inspect without valid tab lease | Denied |
| MCP-04 | R09.01 | Validate/save draft | Revision/hash returned, not approved |
| MCP-05 | R09.02 | Bounded preview | Counts, warnings, size cap |
| MCP-06 | R09.02 | Run approved recipe | Job ID and correct status |
| MCP-07 | R09.02 | Run without approval / override URL | Denied before navigation |
| MCP-08 | R09.02 | Results pagination/export | Bounded data and artifact ID; no arbitrary path |
| MCP-09 | R09.02 | Cancel job/agent exits | Cancel works; agent exit alone does not kill daemon job |
| MCP-10 | R10.01 | Real Codex config and prompt/skill | Actual tools invoked on fixture |
| MCP-11 | R10.02 | Real Claude Code config and prompt/skill | Actual tools invoked on fixture |
| MCP-12 | R10.03 | Unsupported shortcut or client disconnect | Document fallback; no invented command |

## AGENT — task 007/008
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| AGENT-01 | R09.03 | Agent proposes list selection | Visible correct scoped highlights |
| AGENT-02 | R09.03 | Agent proposes detail sections | Visible independent fields |
| AGENT-03 | R09.03 | Owner rejects or edits proposal | No automatic execution; revised preview |
| AGENT-04 | R09.03 | Navigate after proposal | Stale reference denied |
| AGENT-05 | R13.04 | Injected page instruction | No unauthorized action |
| AGENT-06 | R13.02 | Agent attempts credential/private DOM access | Denied; no secret in output |
| AGENT-07 | R13.01 | Grant prohibits AI use | No model disclosure |
| AGENT-08 | R09.02 | Same fixture manual/agent/local | Semantic result equality after excluding time/IDs |

## AI — task 008
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| AI-01 | R11.01 | Valid LM Studio loopback model discovery | Actual model list/capabilities |
| AI-02 | R11.01 | Server offline | Manual mode remains available |
| AI-03 | R11.01 | Unsupported model structured output | Explicit fallback, no false support |
| AI-04 | R11.02 | Valid recipe proposal | Schema pass and deterministic preview |
| AI-05 | R11.02 | Valid tool call within approved scope | Same bounded daemon action |
| AI-06 | R11.03 | Malformed JSON/unknown tool | Reject, bounded repair |
| AI-07 | R11.03 | Hallucinated DOM reference | Reject/no action |
| AI-08 | R11.03 | Timeout/cancel | No subsequent tool call |
| AI-09 | R11.03 | Remote endpoint/fallback attempt | Denied by default; no cloud egress |
| AI-10 | R11.02 | Prompt injection asks scope expansion | Denied |
| AI-11 | R11.04 | Gold fixture benchmark candidate models | Measured parse/mapping/latency/tokens and unsafe proposal count |
| AI-12 | R11.04 | Quality below threshold | Advisory/manual-only, not autonomous success |

## DOMAIN — task 009
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| DOMAIN-01 | R12.01 | Brand/perfume relationship | Unique source keys, no orphan |
| DOMAIN-02 | R12.01 | Similar name different variant | No automatic merge |
| DOMAIN-03 | R12.02 | Top/middle/base and note aliases | Layer/raw name/mapping version retained |
| DOMAIN-04 | R12.02 | Accord rank and visual width | Not presented as exact percent |
| DOMAIN-05 | R12.02 | Abbreviated vote count | Approximate flag and raw string |
| DOMAIN-06 | R12.02 | Ratings vs similarity | Distinct fields/semantics |
| DOMAIN-07 | R12.03 | Missing evidence/source grant | Quarantine or reject |
| DOMAIN-08 | R12.03 | Conflicting observations across dates/sources | Both retained, candidate review |
| DOMAIN-09 | R14.02 | Duplicate exact source observation | Dedup without losing provenance |
| DOMAIN-10 | R14.01 | Package relation/hash/count validation | All manifest files consistent |
| DOMAIN-11 | R14.03 | Parent schema mismatch | Dry-run fails safe, no DB write |
| DOMAIN-12 | R14.03 | Synthetic parent mapping review | Prospective counts and rejected rows; no mutation |

## SEC — task ALL
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| SEC-01 | R13.02 | Wrong tab/no active consent | No injection/access |
| SEC-02 | R13.02 | Credential/hidden form/cookie access request | Not captured; no permission |
| SEC-03 | R13.04 | Untrusted page instructions to model | No unauthorized tool action |
| SEC-04 | R13.04 | HTML/script payload in preview | Rendered text; no execution |
| SEC-05 | R13.02 | Hostile local request origin/Host | Denied |
| SEC-06 | R13.02 | Replay/expired/revoked token | Denied |
| SEC-07 | R13.02 | Spoof extension/client/sender | Denied |
| SEC-08 | R13.03 | Private/loopback/link-local target live-like mode | Denied without contacting target |
| SEC-09 | R13.03 | Redirect/unapproved subresource/DNS policy | Actual request blocked; no egress leak |
| SEC-10 | R13.03 | Infinite scroll/cycle/limit | Bounded stop |
| SEC-11 | R13.04 | Malicious recipe JS/selector/unknown fields | No arbitrary execution |
| SEC-12 | R13.03 | Model self-approval/scope escalation | Denied |
| SEC-13 | R14.01 | Traversal/symlink/absolute output path | No write outside root |
| SEC-14 | R13.04 | CSV formula and control-prefix input | Neutralized |
| SEC-15 | R13.05 | Secret/raw prohibited fields in export | Excluded/denied; no leaked value |
| SEC-16 | R13.03 | Concurrent controller/stale lease | Denied; no collision |
| SEC-17 | R08.02 | Crash/rollback/checkpoint replay | Consistent store; no duplicate |
| SEC-18 | R13.01 | Unknown/revoked/expired grant, blocked domain | Denied before action, no live contact |
| SEC-19 | R16.03 | Dependency version/license/hook and secret scan | Reviewed, pinned, no unapproved installation |
| SEC-20 | R14.03 | Attempt direct parent database write | No capability; no mutation |
| SEC-21 | R13.05 | Sensitive URL/token/private path in diagnostic | Redacted; public-safe logs |
| SEC-22 | R15.02 | Invalid/oversized asset or rights missing | No download/write |
| SEC-23 | R11.03 | Local AI remote fallback | No cloud request |
| SEC-24 | R13.05 | Partial/error/cancel result shown as complete | Explicit status/counters; no false success |

## OPS — task 010
| Test ID | Requirement | Stimulus / setup | Expected assertion |
|---|---|---|---|
| OPS-01 | R16.01 | Clean Windows installation | Pinned build and load guide reproducible |
| OPS-02 | R16.02 | Chrome/Brave real browser smoke | Actual versions and functionality recorded |
| OPS-03 | R16.01 | Daemon setup/stop and port conflict | Safe documented operation |
| OPS-04 | R16.01 | Upgrade with existing fixture dataset | Migration preserves data and recipes |
| OPS-05 | R16.01 | Rollback/restore backup | Supported prior state restored |
| OPS-06 | R16.01 | Uninstall extension and local runtime | Explicit confirmation, no unrelated deletion |
| OPS-07 | R15.01 | Owned article text/metadata | Allowed scoped text exact |
| OPS-08 | R15.02 | Approved fictional image and invalid asset | Valid controlled file; invalid denied |
| OPS-09 | R13.01 | Scheduler grant expiry/revoke/missed run | No unauthorized replay |
| OPS-10 | R16.03 | 100/1000 fictional rows benchmark | Measured time/memory/throughput, environment |
| OPS-11 | R16.03 | Full regression all mandatory modes | Exact counts and evidence, unresolved limitations |
| OPS-12 | R14.03 | Release end-to-end staging package | Manual, agents, local AI and parent dry-run verified |

## Evidence format and release gates
Total planned verification cases: 182 (175 initial cases + 7 coverage/conditional review cases). Each execution report: test ID, exact command, environment/browser/client/model version, commit SHA, expected vs actual, pass/fail/skip/not run, exit code, evidence path, failure classification, limitations. Never fill PASS without execution. Coverage map links R01–R17 to tests above; R17 optional review only, not mandatory feature delivery. SEC suite threat coverage is explicit; future controls repeated when enabled.
Proposed quantitative targets (review before implementation): exact gold record/field match for deterministic extraction; zero unauthorized requests/actions, zero secret leakage in test outputs; bounded preview <=10 records and 64 KiB; result pagination <=100 records and 64 KiB; no duplicate committed rows after retry; no new request after cancellation observed. Hover responsiveness target p95 <=100 ms on F01 and <=250 ms on F14 1000 cards on owner host, subject to measurement/calibration; never claim benchmark before test. Local model target >=90% exact mapping on fixed gold set, zero unsafe actions executed, otherwise advisory/manual-only.
Task gate: applicable tests pass and independent review exact SHA, all failures/skip explicit. Release gate: P1–P7 mandatory verified scopes, P8 operational suite, no unresolved blocker/high in released functionality, actual browser and client smoke, owner review, documentation/current status, backup/rollback. Source permission and parent actual import gates remain separate. If unavailable real client or model, mark relevant acceptance pending; do not report complete release.

## COV — direct coverage additions
| Test ID | Requirement | Phase | Stimulus / setup | Expected assertion |
|---|---|---|---|---|
| COV-01 | R01.03 | P3 | Authenticated pair, disconnect, revoke and reconnect | UI only says paired after successful authenticated handshake; stale/offline/revoked distinct |
| COV-02 | R03.01 | P1b | List, table and grid in three distinct scoped fixtures | One record per selected item; header/distractor excluded |
| COV-03 | R04.03 | P1b | Rename a field and inspect image URL with no asset grant | Original evidence unchanged; no remote image request |
| COV-04 | R12.04 | P7/P8 | Missing optional performance and unlicensed image reference | Missing recorded; no image download or reuse without rights |
| COV-05 | R17.01 | P8 conditional | Review optional permitted catalogue adapter; if approved implement owned fixture | Decision/rights/mapping recorded; optional not core gate |
| COV-06 | R17.02 | Deferred review only | Review organizational social-link scope | No implementation absent separate approval; no bulk personal collection |
| COV-07 | R17.03 | Deferred review only | Review email tool scope | Bulk personal contact harvesting excluded; no implementation in master plan |
See [[Requirement_Traceability_Matrix]] for all 65 direct mappings. COV-05 conditional, COV-06/07 deferred review only; all NOT RUN.
