# Candidate dependencies and upstream research
Verified documentation reads only 2026-11-25; no installs or exact package pins yet. Use package manager not vendoring unless license reviewed; record SPDX, pinned release/commit, license/notice, security audit, install hooks, compatibility and smoke test before adoption. No copied proprietary UI code/assets.
| Tool | Role | Decision |
|---|---|---|
| https://github.com/microsoft/playwright | deterministic runner and E2E; Apache-2.0 per upstream LICENSE | primary |
| https://github.com/microsoft/playwright-mcp | optional interactive browser inspection; separate existing-browser extension | optional, license/version verify |
| https://github.com/modelcontextprotocol/typescript-sdk | custom MCP; official main docs now describe v2 split packages; choose tested stable release, do not assume old SDK API | primary |
| https://github.com/apify/crawlee | queue/retry/storage reference; Apache-2.0, local library | defer adoption until P4 review |
| https://github.com/mozilla/readability | clean text extraction; Apache-2.0 | optional P8 |
| https://github.com/colinhacks/zod | typed schema validation | candidate verify version/license |
| https://github.com/fastify/fastify | local HTTP | candidate verify version/license |
| https://github.com/vitejs/vite | bundle extension | candidate verify version/license |
| SQLite driver, CSV lib | local store/export | Windows compatibility and license spike |
Docs: https://developer.chrome.com/docs/extensions/reference/api/sidePanel ; https://developer.chrome.com/docs/extensions/get-started/tutorial/scripts-activetab ; https://developer.chrome.com/docs/extensions/develop/migrate/to-service-workers ; https://nodejs.org/en/about/previous-releases ; https://lmstudio.ai/docs/developer/openai-compat/structured-output ; https://lmstudio.ai/docs/developer/openai-compat/tools ; https://lmstudio.ai/docs/integrations/codex ; https://developers.openai.com/plugins/concepts/skills ; https://code.claude.com/docs/en/skills (verify client syntax at P5).
No Apify paid platform required. Upstream source access or license does not authorize scraping a third-party domain. Pin exact versions only after review, no npx @latest in durable config. External checkout if needed in ignored .agent-tools/external, not app root. No installs in planning.
