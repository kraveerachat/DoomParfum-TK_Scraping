# Tk-Web-Scraper — packaging and usage modes
Proposed clarification of R01/R03–R11, P1–P6; no new runtime implemented. User wants a reusable GitHub-installable skill as well as independent browser tool. Product master plan remains the implementation authority after Owner merge.

## Three entry points, one engine
| Mode | Entry | Needed | Not required |
|---|---|---|---|
| Manual | Chrome/Brave extension side panel | unpacked MV3 build | Codex, Claude, model, daemon for single page |
| Automated deterministic | extension Jobs UI or approved MCP submission | local daemon + Playwright runner + validated recipe | model after recipe is approved |
| External agent | Codex/Claude Code chat | installed skill (optional convenience), registered custom MCP, daemon, extension for active-tab selection | separate model API key if using supported client subscription |
| Local AI | extension Local AI panel | daemon + LM Studio local REST + supported model | cloud subscription, external agent |
User may also use plain prompts invoking registered MCP tools without skill. All modes use same recipe/extraction contracts. Agent proposal is visible on active permitted tab; owner approval binds job scope. Data viewer can be separate extension tab; no public website needed.

## Skill distribution (P5)
Proposed source layout:
```text
integrations/
  skills/tk-web-scraper/SKILL.md
  skills/tk-web-scraper/references/
  codex/README.md
  claude/README.md
  # verified client-specific config templates and setup scripts if needed
```
Portable SKILL.md frontmatter name tk-web-scraper, description triggering authorized list/detail extraction, workflow using tk_pair_status, tk_inspect_tab, tk_propose_selection, tk_validate_recipe, tk_preview, tk_run, tk_status, tk_results, tk_export. Reference docs for inputs, safe errors, approval and output. Do not embed credentials, arbitrary code or external source grants. Separate source checkout, skill registration, daemon build/start and MCP registration; cloning GitHub or installing skill alone does not start server. Provide version-pinned setup from reviewed release; project-local first, no global settings changes without approval; detect unsupported clients and document manual fallback. Verify actual skill directory/config mechanism and invocation syntax at P5. Candidate invocation e.g. /tk-web-scraper in supporting Claude Code client, skill picker or $tk-web-scraper where supported in Codex; examples are not registered commands now. Plain prompt always documented.
Official references reviewed for design: https://developers.openai.com/plugins/concepts/skills (skill vs MCP boundary); https://code.claude.com/docs/en/skills (verify exact installed client support in P5). Do not assume Codex/Claude subscription exposes API credentials to extension.

## Setup and operation
1. Manual user builds and loads unpacked extension, opens permitted page, selects list or detail fields, previews, extracts, exports. P1/P2.
2. Batch user starts local daemon and pairs extension, saves validated recipe, approves scope and starts bounded job; job runs independently of agent chat. P3/P4.
3. External agent user registers skill in supported client, registers custom MCP stdio server and pairs daemon, opens permitted tab. Requests extraction with plain prompt or supported skill invocation. Agent proposes selection, user reviews, agent invokes approved job; result local. P5.
4. Local AI user starts LM Studio API, selects endpoint/model in extension, verifies capabilities, submits request through local panel. Local planner validates model proposal, previews and requests user approval; same runner. P6.
A future distribution may add versioned installers or plugin packages only after scope/compatibility review; initial release local setup guide. No paid service required.

## Acceptance
P5 demonstrate skill discovery/invocation or explicit supported fallback, MCP tools initialized, both real clients on owned fixture with identical semantic result, installation and removal docs; P6 demonstrate actual local model endpoint, invalid proposal blocked and no cloud fallback. Existing tests and task IDs apply. No new code, tools or installed skill in this planning checkpoint. Source grants and parent handoff remain separate.
