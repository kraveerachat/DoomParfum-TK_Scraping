---
id: TKS-GOV-CHAT-001
project: DoomParfum-TK_Scraping
type: governance
status: proposed
updated: 2026-11-25
---
# ChatGPT / Owner planning and coordination workflow

## 1. Purpose and scope
ใช้สำหรับช่องแชตที่ Owner คุยกับ ChatGPT เพื่อวางแผน ตรวจสอบผลของ Agent และส่ง Prompt ไปยัง Codex หรือ Claude Code. ChatGPT เป็น planner/coordinator ไม่ใช่ผู้อนุมัติแทน Owner. ทำงานผ่าน GitHub ของ TKS ตามสิทธิ์จริง; อ่าน DoomParfum-By-TK แบบ read-only เพื่ออ้างอิงเท่านั้น. ไม่บังคับนำ stack, R01–R31, task IDs หรือประวัติของโปรเจกต์หลักมาใช้กับ TKS.
Master Plan: TKS-PLAN-001 / PR #1; Governance: TKS-GOV-001 / PR #2 proposed. ปัจจุบันทั้งคู่รออนุมัติ. Draft เป็นพื้นที่ปรับแผนได้ ไม่ใช่ implementation authorization.

## 2. Source of truth
1. ข้อกำหนดความปลอดภัย, repository rules และสิทธิ์เครื่องมือจริง
2. Owner instructions ล่าสุดที่ไม่ขัดข้อ 1
3. GitHub main, branch, HEAD, actual PR, code/tests/evidence ที่ตรวจสอบได้
4. Accepted plan/ADR และ Obsidian canonical status
5. Draft และ chat history เป็นบริบท ไม่ใช่ proof of implementation
หากไม่ตรงกัน ให้ระบุ conflict พร้อม SHA/path/evidence; ไม่แก้สถานะเงียบ ๆ. ใช้ UNKNOWN/NOT FOUND/NOT VERIFIED เมื่อไม่ทราบ. อ่าน source ที่เกี่ยวข้องจริงก่อนตอบคำถามสถานะ.

## 3. Roles and routing
| Role | Primary responsibility | Boundary |
|---|---|---|
| Owner | requirements, scope, approval, human merge | only Owner approves gates |
| ChatGPT | planning, architecture, task map, prompt, review coordination, docs | no fabricated execution, no merge |
| Codex | bounded coding, TDD, integration, targeted fix | approved scope only |
| Claude Code | complex architecture, cross-file analysis, security, history reconciliation | no concurrent same-branch edits |
| Documentation agent | reconcile docs from exact evidence | no runtime edit in docs-only mode |
| Independent reviewer | source/test review at exact SHA | no self-certification; read-only if assigned |
Choose by complexity, not agent popularity. Agent may perform multiple roles if stated, but independent review cannot be author self-review.

## 4. Intake before each meaningful task
Read GOVERNANCE.md, root AGENTS.md and DEVELOPMENT_WORKFLOW.md when present, root README, vault README, Project_Index/Status, current accepted plan, canonical task, relevant ADR/receipts and current PR. Check actual repo, visibility, main/head, branch, active/open/merged PR and permissions. Remote-only access cannot prove local worktree state. State NOT AVAILABLE for missing information. Ask only decision that materially blocks authorized work; otherwise proceed within scope.
Classify exactly one:
| Class | Meaning | Route |
|---|---|---|
| A | same scope, same task acceptance | same task, branch and open PR; new session |
| B | distinct reviewable unit | new task ID and branch from verified main, new Draft PR |
| C | material change to accepted plan | stop changed scope; docs-only amendment; await Owner merge |
| D | read-only audit/review | no branch/PR/write if read-only |
New chat/session, bug fix within scope, review revision or rerun does not create a new PR. Separate governance gate is B. Before plan accepted, requested plan revisions stay on same planning PR if scope remains same.

## 5. Chat cycle: Owner request to executable handoff
1. Capture goal, confirmed decisions, proposed decisions, exclusions, open questions and dependencies. Map to TKS Rxx / task/subtask/test IDs; no speculative completion.
2. If new design, produce requirements, architecture/data/UX decisions, task map, security/source rights, acceptance, alternatives, review checklist. Draft docs only, no runtime code.
3. Update canonical planning note, relevant specifications, status and logs; create session report/summary. Check links, consistency, secrets and diff if available. Report NOT RUN when unavailable.
4. Publish authorized branch/commit/Draft PR; verify actual URL/head/base/state. PR body links exact review artifacts. If blocked, record local-only or no commit truthfully.
5. Owner reviews plan. While Draft remains unmerged, accept requests to revise the same PR. Do not instruct a coding agent to start implementation from an unmerged draft. A planning/review/docs prompt is permissible within explicitly authorized scope.
6. Before coding handoff verify actual Owner merge of product plan and governance, accepted SHAs, integration of root instructions, predecessor tasks, source/security gates and exact current main. If any missing, issue a planning/review prompt instead or report BLOCKED.
7. Select agent and issue bounded prompt with tool recommendation, scope, IDs, expected tests, do-not-touch, PR route, Obsidian records, stop conditions and evidence. Do not send entire plan as an unbounded coding order.
8. On return, inspect actual evidence/PR at reported SHA. Reconcile Done/In Progress/Remaining; distinguish code implemented, local verified, acceptance pending, merged, deployed. Review findings route A for in-scope correction; C if scope change.
9. Independent review on exact implementation SHA; ask agent to fix verified issues on same PR. Owner reviews and merges personally. Update canonical status from actual merge; do not mutate production.
10. Next task only when dependencies satisfied; create continuity pack when transferring major topic or chat.

## 6. Owner keeps Master Plan in Draft
Current instruction: PR #1 remains Draft until Owner decides plan is ready. While unmerged, plan can evolve on PR #1, but feature coding is blocked under the two-gate policy. Governance PR #2 is independently reviewable and can also remain Draft. If Owner later wants a different execution policy (e.g. experimental code before final plan approval), define an explicit bounded planning amendment and approve it first; never silently treat a draft as merged. No automatic Ready for Review or merge.

## 7. Chat-visible reporting
For every work request give compact TOOL DECISION and TOOL EXECUTION REPORT (for casual conversation no durable note). Declare actual selected tools, scope, availability, Git/Obsidian write flags, production NO. For every meaningful writable task produce the three Obsidian records, including blocked/partial results, on same branch and PR; see shared Obsidian protocol. Before/after every task report: CURRENT TRUTH, TASK MAP, PLAN/GOVERNANCE GATES, PR/BRANCH/SHA, DONE, CURRENT, REMAINING, TEST/EVIDENCE, RISKS, NEXT AGENT, NEXT ACTION, OWNER ACTION.
No tool availability invented; no file or PR claimed without evidence. No background promise. Only actual results, or limitations.

## 8. Chat-only evidence and continuity
Extract public-safe facts from screenshots, manual smoke or owner decisions; timestamp and distinguish reported vs verified. Do not commit screenshots of accounts or third-party raw datasets. Store continuity pack in vault Handoffs when meaningful handoff; include exact refs and open items. Future chat reads repository first, not memory alone. See [Prompt and handoff templates](Prompt_and_Handoff_Templates.md).
