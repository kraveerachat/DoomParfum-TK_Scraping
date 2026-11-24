# Roadmap and reviewable tasks
P0 TKS-PLAN-001 docs-only, Gate P owner merge. P1-P8 tasks below proposed, not approved or started. Each branch from latest main after predecessor merge; open one Draft implementation PR per task, independent review, owner merge. Split further if scope grows, never skip dependency.
| Phase / task | Scope | Depends | Acceptance evidence |
|---|---|---|---|
| P1a TKS-IMP-001 | workspace, synthetic fixtures, MV3 shell, picker and group detection | P0 | build, load unpacked Chrome + Brave, 20 cards incl. distractor, count exact, cancel/cleanup |
| P1b TKS-IMP-002 | field discovery, list preview, recipe save, standalone table, JSON/CSV | 001 | exact fixture rows, manual edit, Unicode export, no server/model required |
| P2 TKS-IMP-003 | detail extractor and nested sections/arrays, generic perfume fixtures | 002 | hero + accords + votes + pyramid exact fixture comparison, missing field handling |
| P3 TKS-IMP-004 | daemon, authenticated bridge, SQLite, custom MCP preview/status | 003 | pairing, expiry, origin denial, restart and typed tool validation; fixture-only |
| P4a TKS-IMP-005 | runner, next/load-more/bounded scroll, list-to-detail, job cancel | 004 | synthetic multi-page results exact, cycles/limits/cancel/recovery, no external traffic |
| P4b TKS-IMP-006 | hardened egress/storage and independent security acceptance | 005 | negative controls, source default deny, audit, report review; no live crawling |
| P5 TKS-IMP-007 | Codex and Claude Code skills/MCP, agent selection and recipe approval UI | 006 | both real clients fixture demo, visible selection, revoke, slash/skill or documented fallback |
| P6 TKS-IMP-008 | LM Studio provider, local planning loop, offline fallback | 007 | real host capability smoke, fixture evaluation, no cloud fallback, malformed output safe |
| P7 TKS-IMP-009 | export/parent adapter, validation/quarantine, licensed-source pilot if separately approved | 008 | manifest+hashes, dry-run mapping, no parent write, separate source approval; synthetic acceptable |
| P8 TKS-IMP-010 | text/image tools, scheduler/reliability, optional other tool evaluation | 009 | rights-safe fixtures, opt-in scheduling, install/upgrade/rollback guides, benchmark |
R15/R17 optional: email/social bulk personal harvesting non-goal; Shopify catalogue only permitted public feeds, separate amendment if needed. Page Text/asset downloader feature-flagged. No cloud required. Source approval is independent of P0 and runtime review; Fragrantica blocked. Final release acceptance: manual/agent/local modes on fixtures, deterministic exports, permission negative tests, user install/stop/uninstall, parent staging package. Record exact versions/commands/test counts.
Next agent: Codex P1a only after verifying P0 merge SHA; Claude Code reviews architecture/security; no source implementation now.
