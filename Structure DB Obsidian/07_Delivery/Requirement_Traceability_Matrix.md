# Requirements-to-task-to-verification traceability
S2 planning-only. 65 detailed requirements, 182 planned verification cases including conditional/deferred review cases. All cases NOT RUN. This table maps every requirement to at least one directly labelled case; applicable security, integration and regression cases also required. Task numbers refer to TKS-IMP-xxx; ALL means enforce at every relevant phase. Deferred features are not implementation authorization.
| Requirement | Task | Direct verification IDs | Acceptance condition |
|---|---|---|---|
| R01.01 | 001 | EXT-01, EXT-02 | Chrome and Brave actual Windows smoke; unsupported browser explains fallback |
| R01.02 | 001 | EXT-03 | Manual mode usable with daemon absent; no broken loading spinner |
| R01.03 | 004 | COV-01 | No green connected before authenticated handshake |
| R01.04 | 001 | EXT-10 | No invisible selection after tab changes; recovery from worker restart |
| R02.01 | 001 | EXT-04 | No layout shift; overlay non-intercepting except intentional selection click |
| R02.02 | 001 | EXT-05, ALG-01, ALG-03 | Exact count on fixture; excludes distractor outside scoped parent |
| R02.03 | 001 | EXT-06, ALG-02, ALG-04 | Explicit confidence and count; manual fallback if ambiguous |
| R02.04 | 001 | EXT-07, EXT-08, EXT-12 | No unintended navigation; cancel clears overlays/listeners |
| R02.05 | 001 | EXT-09, ALG-05 | Stale selection flagged, new matches not silently accepted |
| R03.01 | 002 | COV-02 | One record per selected repeating item; table header not data row |
| R03.02 | 002 | ALG-06, ALG-07 | Show sample, coverage, inferred type, relative selector; user edits names/types |
| R03.03 | 002 | EXT-16, ALG-08, ALG-09, ALG-12 | Missing -> null with reason; no fabricated values |
| R03.04 | 002 | ALG-10, ALG-11 | Permission check before DOM access; single-page deterministic |
| R04.01 | 002 | EXT-13 | No dependency on original tab staying open after extraction |
| R04.02 | 002 | EXT-14, DATA-04 | Does not mutate original acquisition data or lose nested arrays |
| R04.03 | 002 | COV-03 | Original evidence retained; no remote image fetch by default |
| R04.04 | 002 | DATA-01, DATA-02 | UTF-8, newline/quote handling, formula-safe CSV; counts match |
| R04.05 | 002 | EXT-15, DATA-05 | Local only, no remote delete; storage quota failure reported |
| R05.01 | 003 | DETAIL-01, DETAIL-02, DETAIL-03, DETAIL-04, DETAIL-05 | Scalar, object, repeated list and nested arrays supported |
| R05.02 | 003 | DETAIL-08 | Unrelated sections never zipped together by position |
| R05.03 | 003 | DETAIL-06, DETAIL-07, DETAIL-09 | Duplicate singleton flagged; missing optional not silently coerced |
| R05.04 | 003 | DETAIL-11, DETAIL-12 | Independent section warnings and full JSON tree |
| R05.05 | 003 | DETAIL-10 | Validate on varied synthetic templates; drift stops or quarantines |
| R06.01 | 002/003 | RECIPE-01 | Immutable approved revision; edit creates new revision |
| R06.02 | 003 | RECIPE-03, RECIPE-04, RECIPE-05, RECIPE-08 | Reject unknown version, arbitrary JS and unapproved URLs |
| R06.03 | 003 | RECIPE-02, RECIPE-06, RECIPE-07 | Show changes, source policy and approval state |
| R07.01 | 005 | NAV-01, NAV-02, NAV-03 | Disabled/cycle/no-new rows stop; no arbitrary clicks |
| R07.02 | 005 | NAV-04, NAV-05, NAV-06 | Stop after budget or no new unique IDs; finite timeout |
| R07.03 | 005 | NAV-07, NAV-08 | Each URL checked before request; per-detail error isolated |
| R07.04 | 005 | JOB-08, JOB-09, JOB-10, NAV-09 | Same job+record key not duplicated after restart |
| R07.05 | 005 | JOB-05, JOB-06, JOB-07, JOB-13 | No requests started after cancel observed; terminal result partial if data exists |
| R08.01 | 004 | API-01, API-02, API-03, API-04, API-05, API-06 | Reject unknown origin/token, revoke session, no wildcard CORS |
| R08.02 | 004 | API-10, API-12, JOB-01, JOB-02, JOB-03, JOB-14, SEC-17 | Restart recovery, transaction atomicity, migration rollback tested |
| R08.03 | 004/005 | API-08, API-11 | Poll with cursor, no full unbounded dump to agent |
| R08.04 | 005/006 | JOB-04, JOB-11, JOB-12, JOB-15, NAV-10 | Expiry and policy changes halt job |
| R09.01 | 004/007 | API-07, API-09, MCP-01, MCP-02, MCP-03, MCP-04 | Strict schemas, tab lease, preview safe and bounded |
| R09.02 | 004/007 | MCP-05, MCP-06, MCP-07, MCP-08, MCP-09, AGENT-08 | Job control shares daemon authorization; no arbitrary browser/script/file command |
| R09.03 | 007 | AGENT-01, AGENT-02, AGENT-03, AGENT-04 | Proposal distinct from committed selection; explicit confirmation for scope expansion |
| R10.01 | 007 | MCP-10 | Real installed client smoke against fixtures, supported syntax documented |
| R10.02 | 007 | MCP-11 | Same tool contract, separate integration verification |
| R10.03 | 007 | MCP-12 | No invented slash command; unsupported integration shows manual instructions |
| R11.01 | 008 | AI-01, AI-02, AI-03 | Loopback and explicit model capability smoke; offline works manual |
| R11.02 | 008 | AI-04, AI-05, AI-10 | Schema validation + no invented facts, bounded iterations |
| R11.03 | 008 | AI-06, AI-07, AI-08, AI-09, SEC-23 | Stop safely, show error/fallback; no automatic cloud call |
| R11.04 | 008 | AI-11, AI-12 | Gold fixture evaluation and measured hardware results, no fixed untested model |
| R12.01 | 003/009 | DOMAIN-01, DOMAIN-02 | Fictional fixtures, independent IDs and relationship edges |
| R12.02 | 003/009 | DATA-10, DOMAIN-03, DOMAIN-04, DOMAIN-05, DOMAIN-06 | Array semantics, raw display and units, approximate flags |
| R12.03 | 003/009 | DATA-09, DOMAIN-07, DOMAIN-08 | Timestamp, grant, rule revision, missing/ambiguous status |
| R12.04 | 009/010 | COV-04 | No unauthorized downloads; unavailable values null |
| R13.01 | ALL | EXT-11, AGENT-07, SEC-18, OPS-09 | All modes share deny; expired/unknown grant stops before access |
| R13.02 | ALL | AGENT-06, SEC-01, SEC-02, SEC-05, SEC-06, SEC-07 | No broad permissions, no tokens in log/export/Git |
| R13.03 | 006 | SEC-08, SEC-09, SEC-10, SEC-12, SEC-16 | Redirect/private IP/challenge tests; no evasion |
| R13.04 | ALL | DATA-03, AGENT-05, SEC-03, SEC-04, SEC-11, SEC-14 | XSS/CSV test, model data treated as untrusted |
| R13.05 | ALL | SEC-15, SEC-21, SEC-24 | No raw prohibited or private materials in public repo |
| R14.01 | 009 | DATA-06, DATA-07, DATA-08, DOMAIN-10, SEC-13 | Manifest hashes and counts; reproducible data transforms |
| R14.02 | 009 | DOMAIN-09 | Uncertain candidates never promoted silently |
| R14.03 | 009 | DOMAIN-11, DOMAIN-12, SEC-20, OPS-12 | Separate parent plan, no direct DB write or deployment |
| R15.01 | 010 | OPS-07 | Scoped text only; no blanket page HTML dump |
| R15.02 | 010 | SEC-22, OPS-08 | Grant covers storage, file type/size checks; refs default |
| R16.01 | ALL | OPS-01, OPS-03, OPS-04, OPS-05, OPS-06 | Clean install, upgrade, rollback and no paid mandatory API |
| R16.02 | 001/010 | OPS-02 | Test actual versions; no unsupported claims |
| R16.03 | ALL | SEC-19, OPS-10, OPS-11 | Pinned versions, lockfile, review license and hooks |
| R17.01 | 010 | COV-05 | Separate source approval, mapping and tests; not first release blocker |
| R17.02 | deferred | COV-06 | Public authorized organizational links only if separately approved |
| R17.03 | deferred | COV-07 | Bulk personal contact harvesting explicitly out of scope |
## Review rules
- For each implementation PR copy the applicable requirement IDs and test IDs into the task note; cite actual code path/line, test path, evidence and reviewed HEAD.
- A test ID is not a passing test. A skipped/blocked required test prevents acceptance of that scope.
- Conditional COV-05 and deferred COV-06/07 are scope/rights review obligations, not permission to implement unapproved features.
- Negative controls may cover multiple requirements; the direct map is a minimum, not an exhaustive regression exclusion.
- Owner merge of this plan authorizes only the approved implementation scope; no source grant, parent database write or production deployment follows from this table.
