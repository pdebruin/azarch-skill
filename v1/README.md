# Azure Architecture Agent Skill

Comparative exploration of agent skill approaches for Azure architecture advisory guidance, grounded in the Cloud Adoption Framework (CAF), Well-Architected Framework (WAF), and Azure Architecture Center.

Three agents each implemented the same `requirements.md` spec independently. The results were compared across 18 functional areas, and the best elements were combined into a final **hybrid-azarch** variant.

## Structure

| Folder | Description | Status |
|--------|-------------|--------|
| `hybrid-azarch/` | **Best-of-all** — rich hub + 4 references, built from ghcpcli base + 8 enhancements | ✅ Final (95/100, 18/18 tests) |
| `ghcpcli-azarch/` | GitHub Copilot CLI agent — hybrid skill (hub + reference files) | ✅ Implemented (base for hybrid) |
| `claude-azarch/` | Claude agent — hybrid skill with strong MCP query patterns | ✅ Implemented |
| `m365-azarch/` | M365 Copilot agent — monolith skill with dialogue patterns | ✅ Implemented |

## Shared artifacts

- `requirements.md` — Shared spec: 14 requirements (BR-1–3, FR-1–6, NFR-1–5)
- `test-scenarios.md` — 18 evaluation scenarios with rubrics (T01–T18)
- `comparison.md` — Deep section-level comparison of all 3 source variants across 18 areas

## Approach

1. **Implement** — three agents (ghcpcli, claude, m365) each build a skill from the same requirements
2. **Compare** — section-level analysis across 18 functional areas (see `comparison.md`)
3. **Combine** — take the strongest base (ghcpcli) and enhance with the best ideas from the others
4. **Validate** — evaluate against the hybrid-skills-creator rubric (95/100) and all 18 test scenarios (64/64 checks pass)

## Knowledge sources

All implementations use the [Microsoft Learn MCP server](https://learn.microsoft.com/api/mcp) for up-to-date documentation retrieval, covering:

- Cloud Adoption Framework (CAF)
- Well-Architected Framework (WAF)
- Azure Architecture Center (reference architectures, design patterns, decision guides)
- FinOps (via WAF Cost Optimization pillar)
- Azure Verified Modules (as implementation pointers)
