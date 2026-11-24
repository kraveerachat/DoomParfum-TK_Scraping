# Tk-Web-Scraper — governance entry point
Status: PROPOSED, TKS-GOV-001. This standalone docs-only governance proposal is separate from the Master Plan PR #1. Neither draft is an approved implementation plan.

## Read first
- [ChatGPT workflow](Structure%20DB%20Obsidian/06_Engineering/Governance/01_ChatGPT_Workflow/WORKFLOW.md) — Owner dialogue, planning, PR routing, prompts, independent review, continuity.
- [Coding agent instructions](Structure%20DB%20Obsidian/06_Engineering/Governance/02_Coding_Agent_Workflow/AGENTS.md) — Codex/Claude Code, tools, implementation, Git, tests, security, documentation.
- [Obsidian recording protocol](Structure%20DB%20Obsidian/06_Engineering/Governance/02_Coding_Agent_Workflow/Obsidian_Protocol.md) — shared by every writable meaningful session, including ChatGPT.
- [Governance task](Structure%20DB%20Obsidian/07_Delivery/Tasks/TKS-GOV-001_Workflow_and_Reporting.md).
- [PR workstream index](Structure%20DB%20Obsidian/07_Delivery/Planning_PRs/TKS_PR_Workstream_Index.md).

## Relationship to PR #1
PR #1 contains the product master plan and existing short root AGENTS.md / DEVELOPMENT_WORKFLOW.md; PR #2 owns detailed governance only. Both target main independently, with disjoint changed paths. Do not copy product specs or parent-specific implementation decisions into these policies. Root entry files in PR #1 are not replaced here to avoid conflict.
After Owner approves both proposals, reconcile root AGENTS.md and DEVELOPMENT_WORKFLOW.md, vault README, Project_Index, Project_Status and shared logs to link this governance. If PR #1 is still Draft after PR #2 merge, do that as an authorized checkpoint on PR #1; otherwise use a separate docs-only task from latest main. Do not start runtime coding until both approvals and entry-point reconciliation are verified. PR #2 alone does not authorize implementation.
No direct main writes, merge or production activity by agents. Source rights remain independent. Future edits follow the accepted workflow, not this draft before Owner approval.
