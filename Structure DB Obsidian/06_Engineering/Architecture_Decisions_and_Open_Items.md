# Decision register and open design items
S2 detailed proposed decisions; owner delegated architecture selection but Gate P remains unmerged. Record accepted revision on merge, no silent status conversion.

| ADR | Decision | Rationale | Alternative / consequence | Status |
|---|---|---|---|---|
| TKS-ADR-001 | Independent repository and export boundary | reusable optional companion | embedded parent code rejected; mapping work needed | proposed |
| TKS-ADR-002 | Three modes share deterministic core | no AI per row, offline manual | AI-only browser clicking rejected | proposed |
| TKS-ADR-003 | TypeScript npm workspace | common contract and validation | mixed-language stack adds coupling | proposed |
| TKS-ADR-004 | MV3 sidepanel plus standalone table | familiar visual interaction, persistent results | popup-only insufficient; Brave smoke required | proposed |
| TKS-ADR-005 | Generic recipes + domain adapter | flexible and testable | hardcoded one-site crawler brittle and rights blocked | proposed |
| TKS-ADR-006 | Fastify loopback daemon + SQLite | durable local jobs, no external DB | memory-only state insufficient; driver pending | proposed |
| TKS-ADR-007 | Custom MCP stdio thin adapter | narrow policy-enforced tools | generic Playwright MCP optional only | proposed |
| TKS-ADR-008 | Dedicated Playwright runner | reproducible bounded execution | shared personal profile unsafe default | proposed |
| TKS-ADR-009 | Explicit source grant and hash-bound approval | fail closed, audit | implicit permission from public page rejected | proposed |
| TKS-ADR-010 | JSON-first and relational CSV export | nested data fidelity | flatten everything loses relationships | proposed |
| TKS-ADR-011 | LM Studio optional loopback provider | local AI and no required paid API | cloud fallback rejected | proposed |
| TKS-ADR-012 | Preserve observations and evidence | missing/conflict not fabricated | automatic canonical merging rejected | proposed |
| TKS-ADR-013 | Fixture-first and separate live source gate | reliable legal/security boundary | live prohibited source testing rejected | proposed |
| TKS-ADR-014 | Two-gate PR + Obsidian session records | traceable review and handoff | direct main writes rejected | proposed |
| TKS-ADR-015 | P8 advanced tools opt-in | focus catalogue needs | feature parity with commercial product not release condition | proposed |

## Open items with resolution gate
| Item | Owner / phase | Resolution evidence | Stop condition |
|---|---|---|---|
| O01 exact package versions and licenses | Codex P1/P3 | version matrix, lockfile, smoke | incompatible or unreviewed hook |
| O02 Brave sidePanel behavior | Codex P1 | actual Windows manual test | fallback requires owner acceptance if UX differs materially |
| O03 SQLite binding/secure token storage | Claude review + Codex P3 | Windows smoke, threat review | no insecure plaintext token storage |
| O04 egress containment method | Claude review P4b | request/redirect/DNS negative tests | fixture-only until proven |
| O05 client config/skill syntax | Codex/Claude P5 | both actual clients | unsupported integration marked pending |
| O06 LM Studio model choice | Owner/Codex P6 | benchmark and capability check | manual fallback if quality insufficient |
| O07 Fragrantica written authorization | Owner/source rights gate | explicit grant covering intended access/use | NO-GO until grant |
| O08 other dataset licenses | Owner/research P7 | actual terms/license/provenance | no use absent permission |
| O09 parent accepted import contract | parent reviewer P7 | current main/schema mapping and separate approval | no parent writes |
| O10 advanced image/social/catalogue tools | Owner P8 | rights and scope amendment if material | optional, not default bulk contacts |
| O11 performance threshold calibration | Codex P1/P4/P8 | measured fixture baseline and agreed target | no invented benchmark |
| O12 publishing identity | Owner before agent Git writes | scoped authorized identity/permission | local-only if unavailable |
Architecture acceptance does not waive these later compatibility/security/source checks. If a proposed solution fails, document alternative and impact; material change uses planning amendment, not silent rewrite.
