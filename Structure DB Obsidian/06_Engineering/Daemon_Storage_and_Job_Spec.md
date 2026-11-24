# Local daemon, storage, job and HTTP contract
Proposed design; implementation and actual API tests P3/P4. All routes below are local and typed; no unrestricted browser or filesystem access.

## Startup and shutdown
- Daemon loads validated configuration, checks runtime directory ownership/access, opens SQLite, verifies schema version and applies only supported migrations with pre-migration backup. If migration fails, refuse startup and keep backup.
- Bind 127.0.0.1 on configurable port (default candidate 8765); verify no public interface. Port conflict produces explicit error. Readiness requires database available and policy loaded; liveness separate. Startup logs do not print token, profile or private paths.
- Authenticated client registration is explicit; no open pairing endpoint that grants access merely from a browser request. Owner initiates pairing in local daemon CLI/UI, challenge expires and is single-use; pair extension ID or agent client identity to scoped token.
- Shutdown stops accepting work, signals cancellation or checkpoint pause, flushes transactions, closes owned contexts and releases lock. Restart detects stale running leases and marks interrupted, not successful. No automatic replay of previously approved external jobs without fresh policy/approval validation.

## HTTP API v1 (contract)
Base /v1; JSON request/response except controlled artifact download. Every request requires a valid scoped authorization except minimal non-sensitive liveness and owner-initiated pairing challenge flow. Strict content-type, max body 256 KiB proposal, request IDs, no unknown fields for mutation. Error envelope:
```json
{"error":{"code":"POLICY_DENIED","message":"Source is not approved","retryable":false,"requestId":"req-001"}}
```
| Method and path | Input | Output | Authorization / effect |
|---|---|---|---|
| GET /healthz | none | alive boolean | no version, paths or secrets |
| GET /v1/ready | none | ready, contract versions | paired read |
| POST /v1/pair/confirm | one-time challenge, clientId, type | scoped token once | owner-initiated, validated client/origin, expiry |
| POST /v1/pair/revoke | clientId | revoked | owner control; invalidates lease |
| GET /v1/sources | cursor/limit | grant metadata, allowed capabilities | scoped read, no raw permission documents |
| GET /v1/recipes | cursor/limit, status | summaries | scoped read |
| GET /v1/recipes/{id}/{rev} | none | exact allowed recipe | scoped read |
| POST /v1/recipes/validate | recipe | field errors, hash, warnings | no network action by default |
| POST /v1/recipes | draft, expected revision | saved revision, hash | draft write only; no approval |
| POST /v1/approvals | recipe hash, grant refs, budget | approval reference | owner confirmation path only, never MCP self-grant |
| POST /v1/tabs/lease | extension document ref, capability | leaseId/expiry | explicit owner grant in extension |
| POST /v1/tabs/inspect | leaseId, scope | bounded DOM summary, generation | leased read; policy check |
| POST /v1/tabs/proposal | leaseId, generation, refs | proposalId | visible extension proposal, no click |
| GET /v1/tabs/proposals | cursor | pending proposals | paired extension only |
| POST /v1/tabs/proposals/{id}/decision | accept/reject/edit | result | owner UI only; reject stale generation |
| POST /v1/previews | recipe ref, approved target, budget | sample, counts, warnings | scoped preview; source policy before access |
| POST /v1/jobs | recipe/graph hash, approvalRef, idempotencyKey | 202 jobId, state | approved scope and limits; no raw URL override |
| GET /v1/jobs | cursor/limit/state | summaries | scoped read |
| GET /v1/jobs/{id} | none | status, counters, reason | scoped read |
| POST /v1/jobs/{id}/cancel | reason enum | current state | authorized operator; idempotent |
| POST /v1/jobs/{id}/resume | current approvalRef | jobId/state | explicit operator; revalidate scope/checkpoint |
| GET /v1/jobs/{id}/results | cursor/limit | permitted bounded rows + next cursor | scoped read and export policy |
| POST /v1/exports | datasetId, format, selection | artifactId, manifest summary | no supplied absolute path, grant checked |
| GET /v1/exports/{id} | none | allowed artifact stream or summary | local scoped read, size bound |
| POST /v1/imports | bounded manifest + records | import validation/conflicts | explicit owner import from extension |
| DELETE /v1/datasets/{id} | confirmation nonce | deleted local data | owner only; no remote deletion |
Proposed HTTP status mapping: 400 invalid JSON/schema, 401 unauthenticated, 403 policy/scope denied, 404 unknown or inaccessible resource, 409 revision conflict/stale lease/duplicate incompatible idempotency key, 413 oversize, 422 invalid recipe, 429 local throttle, 503 daemon not ready. No stack traces. Path IDs are opaque and checked against store, never interpreted as paths.

## SQLite logical data model
| Entity | Primary key | Important columns and relationships |
|---|---|---|
| schema_migrations | version | applied_at, migration_hash |
| sources | source_id | label, owner, status, created_at |
| source_grants | source_id+revision | origins, allowed_actions/fields, rights ref, expires_at, limits, revoked_at |
| clients | client_id | type, paired_origin, token_hash, scopes, expiry/revoked_at |
| tab_leases | lease_id | client_id, tab/document/generation, origin, expires_at, status |
| recipes | recipe_id | label, current_revision, retired_at |
| recipe_revisions | recipe_id+revision | schema_version, canonical_json, hash, validation_state, created_at |
| approvals | approval_id | recipe/graph hash, grants, limits, owner consent ref, expiry, revoked_at |
| jobs | job_id | approval_id, recipe graph ref, state, reason, timestamps, idempotency_key |
| job_attempts | attempt_id | job_id, lease/heartbeat, start/end, outcome, safe error |
| checkpoints | job_id+sequence | page/edge ref, stable key cursor, counters, commit timestamp |
| datasets | dataset_id | job_id or manual origin, recipe ref, state, counts, retention |
| observations | dataset_id+record_key+observation_id | source ref, field values/evidence JSON, observed_at, validation_status |
| quarantine | quarantine_id | observation ref, reason code, review state |
| exports | artifact_id | dataset_id, format, manifest hash, generated path ref, size, created_at |
| audit_events | event_id | event type, actor scope, resource ID, timestamp, safe outcome |
Indexes: jobs state/created_at, observations dataset+record_key, recipe revisions unique hash, audit timestamp/resource. Foreign keys enabled; unique constraints for idempotency key scoped by client and job record keys. No plaintext token, cookie, raw private DOM, raw secret URL or model prompt in database. Raw fields retained only when grant permits. SQLite WAL candidate evaluated on Windows; one writer queue, transactions and bounded busy timeout; no concurrent cross-process uncontrolled writes. Extension IndexedDB separate stores: datasets, records, recipes, settings references; small settings chrome.storage. No auto sync.

## Job state machine
| State | Allowed transitions | Notes |
|---|---|---|
| queued | running, cancelled, failed | persisted before dispatch |
| running | paused, succeeded, partial, failed, cancelled | worker lease, counters/checkpoint |
| paused | queued, partial, cancelled, failed | explicit reapproval for resume |
| succeeded | none | all requested scope completed, validation policy met |
| partial | none | some retained valid rows + explicit stop reason |
| failed | none | no acceptable completion, error recorded |
| cancelled | none | operator cancellation, retained records labelled |
Terminal jobs immutable except separate reviewed annotation/export. Resume after terminal creates a new linked job, not rewrite history. Interrupted running jobs become paused (if recoverable) or partial/failed according to committed data; no automatic live re-run. Job attempts have lease expiry and heartbeat; stale worker cannot commit after replacement (lease generation check). Cancel signal checked before each navigation, action, retry, write; in-flight browser action aborted where supported; close owned context, never kill unrelated browser.

## Job orchestration
1. Validate request schema and idempotency key; duplicate exact request returns existing job ID, incompatible same key -> 409.
2. Resolve exact recipe and graph revisions, grant, approval expiry and budget. Reject stale approval, unknown source, unapproved graph.
3. Transaction inserts queued job + initial audit event; returns job ID immediately.
4. Worker lease claims one queued job transactionally, initializes isolated context and per-job counters. No browser context reuse across unrelated origins.
5. Navigate only approved start URL; verify redirect and all requests per egress policy. Wait for bounded expected DOM conditions, not endless network-idle.
6. Extract deterministic records and write valid/quarantine rows + counters + checkpoint atomically. Dedup by stable source key with conflict warning.
7. Process approved navigation strategy/edges; enforce max pages/records/actions/duration, per-origin concurrency, rate and stop reasons; no model in per-row loop.
8. Finalize counts, validation status, termination reason, release context and lease; no silent error swallowing.
9. Export reads committed snapshot (not inconsistent running state), includes status and counts; partial dataset labelled. Manifest hashes atomic file write then rename.
10. Retry only permitted transient transport/timeouts with limited attempts (proposed max 2) and capped backoff; never retry access denial/challenge or 429 without separate authorized decision. Failed detail page logged without corrupting other records.

## Stop conditions and progress
Counters: attempted pages, completed pages, unique record keys, valid rows, quarantined, duplicates, failed detail pages, elapsed time, queued detail links, bytes stored. Never show estimated total as actual matched count. Stop codes: COMPLETE, MAX_PAGES, MAX_ROWS, MAX_ACTIONS, MAX_DURATION, NO_PROGRESS, CYCLE_DETECTED, NEXT_DISABLED, POLICY_DENIED, GRANT_EXPIRED, CHALLENGE, ACCESS_DENIED, RATE_LIMIT, SELECTOR_DRIFT, STORAGE_FULL, CANCELLED, WORKER_LOST, ERROR. Map finish reason to terminal state explicitly; no-progress on known final page may be complete only with fixture-proven semantics; otherwise partial. Progress polling bounded; no sensitive query strings in UI.
## Recovery and operational tests
SQLite migration rollback, atomic checkpoint, duplicate retry, crash before/after transaction, stale lease write denied, cancellation in navigation and export, port collision, storage full, revocation, expiry, invalid idempotency, bounded result pagination. Test restart without network and actual Windows paths. Commands and test counts recorded only after implementation.
