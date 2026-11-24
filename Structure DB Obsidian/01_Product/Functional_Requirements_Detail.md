# Detailed functional requirements and scope
Proposed expansion of R01–R17 in [[Requirements_Register]]. Parent product scope remains independent. IDs below are stable requirement sub-identifiers for review and tests. Each requirement is normative only after Gate P.

## Personas and modes
Owner/operator: installs extension, selects permitted tab, defines recipes, reviews results, exports and imports. Agent operator: requests actions through approved MCP client, reviews proposed changes and job limits. Local model planner: suggests bounded structured actions, no authority. Reviewer: reads source/tests, exact SHA and synthetic evidence; no runtime authority. No multi-user accounts or SaaS subscription.

## Functional requirements
| ID | User-visible behavior | Acceptance / edge conditions | Task |
|---|---|---|---|
| R01.01 | Toolbar action opens side panel for active tab | Chrome and Brave actual Windows smoke; unsupported browser explains fallback | 001 |
| R01.02 | Home shows tools, recipes, data, jobs, settings | Manual mode usable with daemon absent; no broken loading spinner | 001 |
| R01.03 | Connection state distinguishes local, paired, offline, revoked | No green connected before authenticated handshake | 004 |
| R01.04 | Picker state persists while side panel is reopened | No invisible selection after tab changes; recovery from worker restart | 001 |
| R02.01 | Hover outlines candidate element | No layout shift; overlay non-intercepting except intentional selection click | 001 |
| R02.02 | Repeating sibling group count and outline | Exact count on fixture; excludes distractor outside scoped parent | 001 |
| R02.03 | User cycles candidate ancestors and chooses row/card | Explicit confidence and count; manual fallback if ambiguous | 001 |
| R02.04 | Include/exclude and reselect, undo, escape | No unintended navigation; cancel clears overlays/listeners | 001 |
| R02.05 | Track DOM changes/scroll/resize while selecting | Stale selection flagged, new matches not silently accepted | 001 |
| R03.01 | Detect list/table/grid from selected scope | One record per selected repeating item; table header not data row | 002 |
| R03.02 | Suggest visible fields and URL attributes | Show sample, coverage, inferred type, relative selector; user edits names/types | 002 |
| R03.03 | Preview exact rows and field-level warnings | Missing -> null with reason; no fabricated values | 002 |
| R03.04 | Extract page on explicit user action | Permission check before DOM access; single-page deterministic | 002 |
| R04.01 | Separate data view tab or window | No dependency on original tab staying open after extraction | 002 |
| R04.02 | Search, sort, filter, row drilldown | Does not mutate original acquisition data or lose nested arrays | 002 |
| R04.03 | Rename mapping and view image URL safely | Original evidence retained; no remote image fetch by default | 002 |
| R04.04 | JSON and CSV download | UTF-8, newline/quote handling, formula-safe CSV; counts match | 002 |
| R04.05 | Dataset deletion confirmation | Local only, no remote delete; storage quota failure reported | 002 |
| R05.01 | Select multiple independent sections on detail page | Scalar, object, repeated list and nested arrays supported | 003 |
| R05.02 | Select section scope then relative field selectors | Unrelated sections never zipped together by position | 003 |
| R05.03 | Required/optional and cardinality validation | Duplicate singleton flagged; missing optional not silently coerced | 003 |
| R05.04 | Section preview and editing | Independent section warnings and full JSON tree | 003 |
| R05.05 | Recipe reusable on similar pages | Validate on varied synthetic templates; drift stops or quarantines | 003 |
| R06.01 | Save named versioned draft recipe | Immutable approved revision; edit creates new revision | 002/003 |
| R06.02 | Import/export validated recipe document | Reject unknown version, arbitrary JS and unapproved URLs | 003 |
| R06.03 | Compare recipe revisions and validation status | Show changes, source policy and approval state | 003 |
| R07.01 | Pagination by approved next control | Disabled/cycle/no-new rows stop; no arbitrary clicks | 005 |
| R07.02 | Load more and bounded scroll | Stop after budget or no new unique IDs; finite timeout | 005 |
| R07.03 | List-to-detail DAG using approved extracted links | Each URL checked before request; per-detail error isolated | 005 |
| R07.04 | Dedup and resume checkpoint | Same job+record key not duplicated after restart | 005 |
| R07.05 | Progress counts and cancellation | No requests started after cancel observed; terminal result partial if data exists | 005 |
| R08.01 | Loopback bridge with authenticated pairing | Reject unknown origin/token, revoke session, no wildcard CORS | 004 |
| R08.02 | Persistent daemon recipes/jobs/records | Restart recovery, transaction atomicity, migration rollback tested | 004 |
| R08.03 | Status and bounded results API | Poll with cursor, no full unbounded dump to agent | 004/005 |
| R08.04 | Job limits checked independently of UI/model | Expiry and policy changes halt job | 005/006 |
| R09.01 | Custom MCP list/inspect/propose/validate/save | Strict schemas, tab lease, preview safe and bounded | 004/007 |
| R09.02 | MCP preview/run/status/results/export/cancel | Job control shares daemon authorization; no arbitrary browser/script/file command | 004/007 |
| R09.03 | Agent selection shown visually to user | Proposal distinct from committed selection; explicit confirmation for scope expansion | 007 |
| R10.01 | Codex supported MCP configuration and skill | Real installed client smoke against fixtures, supported syntax documented | 007 |
| R10.02 | Claude Code supported MCP configuration and skill | Same tool contract, separate integration verification | 007 |
| R10.03 | Short command or plain-language fallback | No invented slash command; unsupported integration shows manual instructions | 007 |
| R11.01 | LM Studio endpoint/model selection | Loopback and explicit model capability smoke; offline works manual | 008 |
| R11.02 | Model suggests JSON recipe and tool calls | Schema validation + no invented facts, bounded iterations | 008 |
| R11.03 | Invalid model output, timeout or context limit | Stop safely, show error/fallback; no automatic cloud call | 008 |
| R11.04 | Compare local model quality/latency | Gold fixture evaluation and measured hardware results, no fixed untested model | 008 |
| R12.01 | Brand/catalogue/detail shape | Fictional fixtures, independent IDs and relationship edges | 003/009 |
| R12.02 | Accords, votes, wear and note pyramid | Array semantics, raw display and units, approximate flags | 003/009 |
| R12.03 | Source provenance per field | Timestamp, grant, rule revision, missing/ambiguous status | 003/009 |
| R12.04 | Image refs and optional performance | No unauthorized downloads; unavailable values null | 009/010 |
| R13.01 | Source registry default deny | All modes share deny; expired/unknown grant stops before access | ALL |
| R13.02 | Least privilege and local credential isolation | No broad permissions, no tokens in log/export/Git | ALL |
| R13.03 | Browser network safety and limits | Redirect/private IP/challenge tests; no evasion | 006 |
| R13.04 | Safe rendering/export and prompt injection defense | XSS/CSV test, model data treated as untrusted | ALL |
| R13.05 | Reviewable audit and retention | No raw prohibited or private materials in public repo | ALL |
| R14.01 | Versioned staging export | Manifest hashes and counts; reproducible data transforms | 009 |
| R14.02 | Dedup and quarantine | Uncertain candidates never promoted silently | 009 |
| R14.03 | Parent compatibility/dry-run | Separate parent plan, no direct DB write or deployment | 009 |
| R15.01 | Clean page text and metadata on permitted content | Scoped text only; no blanket page HTML dump | 010 |
| R15.02 | Image URL collection and optional download | Grant covers storage, file type/size checks; refs default | 010 |
| R16.01 | Windows installation/uninstallation guide | Clean install, upgrade, rollback and no paid mandatory API | ALL |
| R16.02 | Chrome/Brave compatibility and failure fallback | Test actual versions; no unsupported claims | 001/010 |
| R16.03 | Reproducible dependency and build records | Pinned versions, lockfile, review license and hooks | ALL |
| R17.01 | Optional permitted catalogue adapter | Separate source approval, mapping and tests; not first release blocker | 010 |
| R17.02 | Social link extraction scope review | Public authorized organizational links only if separately approved | deferred |
| R17.03 | Email extractor evaluation | Bulk personal contact harvesting explicitly out of scope | deferred |

## Priority and definition of success
P1/P2 mandatory usability milestone, P3/P4 mandatory reliable automation, P5/P6 mandatory agent modes, P7 mandatory handoff, P8 conditional features and final release. P8 extras cannot block P1. No account creation, purchases, messages, posting, credential harvesting, hidden endpoint scraping, or bot bypass. No cloning protected assets/code. No live source pilot without rights. No automatic parent database updates.
A feature is accepted only when the mapped test passes on exact reviewed SHA, docs match observed behavior, failures are recorded, and Owner merges implementation PR. Manual success does not establish MCP success; fixture success does not establish site permission or compatibility.
