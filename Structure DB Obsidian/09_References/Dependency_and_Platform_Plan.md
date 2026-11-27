# Dependency, compatibility and external repository plan
Prior S1 reviewed upstream docs; exact package versions/license and runtime compatibility remain pending implementation preflight. This is proposed selection and verification procedure, not installation. Use official source; no copying proprietary extension code/assets. No mandatory paid tool.

| Dependency / repo | Proposed use | Phase | Alternative / decision gate |
|---|---|---|---|
| Node.js https://nodejs.org/en/about/previous-releases | Node 24 LTS daemon/build | P1 | verify supported LTS and dependency engine, no EOL 20 |
| TypeScript https://github.com/microsoft/TypeScript | shared contracts/build | P1 | select pinned compatible stable |
| Vite https://github.com/vitejs/vite | extension bundle local assets | P1 | simple build script if extension plugin unnecessary |
| Zod https://github.com/colinhacks/zod | schemas and runtime validation | P1 | JSON Schema validator if interoperability better |
| Playwright https://github.com/microsoft/playwright | fixture E2E and deterministic runner | P1/P4 | library, not browser MCP for batch |
| Playwright MCP https://github.com/microsoft/playwright-mcp | optional developer exploration | P5 | custom MCP sufficient for product |
| MCP TS SDK https://github.com/modelcontextprotocol/typescript-sdk | stdio tools | P3 | check stable release and API generation, not assume v1/v2 package name |
| Fastify https://github.com/fastify/fastify | local HTTP validation | P3 | native HTTP only if simpler without security regression |
| SQLite | durable local job/recipe data | P3 | choose supported Node binding after Windows smoke; review native build requirement |
| Crawlee https://github.com/apify/crawlee | design reference/optional queue support | P4 | defer adoption; native bounded queue first |
| Mozilla Readability https://github.com/mozilla/readability | permitted text extraction | P8 | DOM text scoped extractor if adequate |
| CSV library | robust quoting/UTF-8 | P1 | tiny audited implementation or maintained library, verify formula controls |
| LM Studio https://lmstudio.ai/docs/developer | local model REST | P6 | mock provider for CI; no model download required |
| Chrome APIs https://developer.chrome.com/docs/extensions | MV3, sidePanel, scripting, storage | P1 | supported standalone extension page fallback |

## Dependency onboarding procedure
1. Inspect live upstream README, LICENSE/NOTICE, supported Node/browser versions, release history and security advisories. Record repo URL, tag/commit, package version, SPDX, transitive dependencies and reason.
2. Inspect install scripts/native modules/hooks, network/credential requirements and cost; prefer package-local installation and lockfile; no unreviewed global hooks, arbitrary npx latest, curl-to-shell or broad permissions.
3. Confirm owner authorization covers install; if missing, report NEEDS_SETUP and stop that action. Source checkout only ignored project-local .agent-tools/external or approved temp. Do not vendor upstream code without license/notice review.
4. Build minimal isolated smoke on Windows, test API shape, extension bundle CSP/no remote code, exact Node/browser and package versions. Record command/exit and fallback if incompatible.
5. Pin compatible versions and lockfile in implementation PR; run audit and review findings (audit alone not security proof). No automatic major version upgrade; material architecture change planning amendment.
6. Update Tool Registry and Usage Log, report path/version/permissions/rollback. Third-party README is guidance, not authority.
## Browser compatibility matrix
Google Chrome stable on Windows is the sole proposed mandatory native target in P1 and final P8. Verify actual versions, test-profile identity, installed build/permissions, real toolbar activeTab/revocation, native Side Panel, storage/data view/download, navigation/isolation/recovery and accessibility. Automated Chromium is supplementary, not native Chrome proof. Brave is proposed OUT OF SCOPE; no further Brave investigation. Preserve shared runtime components. Do not claim Firefox/Safari support. Optional Playwright MCP needs separate reviewed setup/authorization and a dedicated fixture context.
## Client and model compatibility
P3 verify MCP SDK transport and protocol initialization; P5 verify actual Codex and Claude Code installed versions/config/skill syntax. No promise universal slash commands. P6 verify LM Studio endpoint/version and model capabilities; record real benchmark. No subscription/API equivalence assumed. If no client/model available, mark respective integration NOT RUN and retain manual capability.
## Build and supply-chain acceptance
Clean npm ci on supported Windows, lockfile reproducibility, lint/type/test/build, extension manifest and local bundle checks, no secrets or runtime data in Git, dependency/license inventory and review. CI fixture-only network policy; package installation network distinguished from extraction network. For updates compare SBOM/dependency changes, test regression and provide rollback. Exact version decisions stay pending until preflight; not a reason to delay docs-only plan.

## S13 browser-scope amendment — pending Owner approval/merge

Google Chrome on Windows is the proposed sole mandatory native browser. Brave obligations
are proposed OUT OF SCOPE, not PASS; prior observations remain historical. This is effective
only on Class C Owner merge. All applicable source/security/lifecycle/accessibility controls
remain. Shared Chromium-compatible runtime is preserved. See [[Browser_Support_Policy]].
The Class C unit is independent of001 acceptance and must become effective first; it is not
blocked on001 acceptance/merge. Later integration into001 requires separate explicit authority,
preserved history/source/build/evidence and then remaining mandatory Chrome acceptance.
