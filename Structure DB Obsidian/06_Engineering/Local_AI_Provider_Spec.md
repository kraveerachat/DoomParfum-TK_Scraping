# LM Studio local inference design
Optional local planning mode; manual and external agent paths work without it. User may select models already available in LM Studio; model choice and throughput must be measured on actual machine, not asserted from parameter count. No training on scraped material.

## Provider interface
Provider abstraction: discoverModels(), checkCapabilities(modelId), proposeRecipe(context,budget), proposeToolCall(context,budget), abort(requestId). LM Studio adapter uses supported local REST endpoints; candidate default http://127.0.0.1:1234/v1. Verify running version and actual endpoint behavior at P6. Loopback-only default; no arbitrary remote provider or silent fallback. User explicitly configures endpoint/model; connection test returns model ID and supported features or safe error.
- GET /models for discovery; POST /chat/completions for structured JSON or supported tool calls; /responses optional only after supported version test.
- Use schema-constrained output when model/server supports it. Do not assume every model supports function calling or strict JSON.
- Timeout, max input bytes, output tokens, iteration budget, request count and cancellation enforced in local planner. Proposed initial limit 3 proposal iterations, 4096 output tokens maximum (adjust after fixture benchmark), 60-second request timeout configurable; no unbounded self-loop.
- Never send cookie/token/password/form/private DOM to model. Even local model output is untrusted. Source grant must cover AI use where applicable. No cloud fallback, no automatic telemetry upload by TKS.

## Planning context
Provide page-type hint, allowed field kinds, bounded structural refs and sanitized text examples, allowed tool schemas, recipe constraints and explicit source scope. Treat every DOM text fragment as data with delimiters. Do not concatenate page text into authoritative instruction. Model can suggest field labels, grouping, selector references or mapping; it cannot invent source facts, authorize grants or choose arbitrary network destinations.
Two modes:
1. Schema assistant: returns JSON draft matching recipe proposal schema. Validate fields, unknown keys, scope, cardinality and refs; deterministic preview compares gold expected rows.
2. Tool-planning assistant: selects allowlisted next operation + typed arguments; planner rechecks policy/approval, calls same daemon API, returns bounded result. No direct model-to-browser or model-to-shell access.
Model proposal fields: proposalId, pageType, selectedGroupRef/sectionRefs, fields [{name,ref,kind,required}], rationale short, uncertainties, requestedOperation enum. Reject raw JS, arbitrary URL, unknown refs, unknown tools, embedded commands. If invalid, return safe validation errors and at most bounded repair; then manual fallback.

## Local mode flow
User opens Local AI panel, starts LM Studio server manually, chooses model and runs capability check. Pair extension/daemon and grant permitted tab. Local planner receives structural context, proposes selection -> UI shows highlights and diff -> owner accepts/edits -> validate recipe -> deterministic preview -> explicit approved run. Runner extracts all rows without model loop. Store model ID, adapter/version, proposal hash and validation outcome, not raw private prompt. Show processing, cancelled, offline, invalid proposal, approval required and completed states. If server stops, cancel inference, keep existing draft, allow manual completion.
## Model evaluation plan
Gold fixture set: list grid 20 records+distractor; table with headers; missing optional field; detail page with multiple sections; nested pyramid; ambiguous labels; DOM drift; malicious instruction in visible text; non-English labels; oversized page. For each candidate model record exact ID/quantization/context, LM Studio version, hardware, GPU offload settings, input/output tokens, time to first token, total latency, peak memory if measurable, parse success, valid proposal rate, field precision/recall, correct abstention, repair count, unsafe proposal count (must not execute). Compare deterministic baseline/manual gold. Proposed acceptance for selected model: 100% invalid output blocked, zero unauthorized actions, >=90% exact field mappings on fixed gold set or remain advisory/manual only; threshold to be reviewed before claiming quality. Do not claim all models equivalent. Repeat same fixtures/settings for comparison.
## Failure and fallback
| Failure | Behavior |
|---|---|
| Server unavailable | mark offline, manual/agent alternatives available |
| Unsupported JSON/tool calls | use constrained plain JSON if supported, otherwise manual |
| Malformed or oversized response | reject, bounded repair then fail |
| Hallucinated selector/ref | reject or mark unresolved, no browser action |
| Prompt injection requests credentials/URL | treat as page data, deny |
| Timeout/out of memory | abort, report, suggest user-selected smaller model; no auto cloud |
| User cancels | cancel request, no subsequent tool call |
| Source grant changes | stop, clear pending proposal |
## Acceptance
Real LM Studio smoke on owner host, discovery and selected model proposal, exact fixture comparison, negative cases and fallback. Document configuration without secrets. No local model required in CI; mocked provider for deterministic tests, actual integration separately marked. Model benchmark is P6 deliverable, not P0 evidence.
