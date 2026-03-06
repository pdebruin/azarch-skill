# Azure Architecture Agent Skill

Comparative exploration of agent skill approaches for Azure architecture advisory guidance, grounded in the Cloud Adoption Framework (CAF), Well-Architected Framework (WAF), and Azure Architecture Center.

## Structure

| Folder | Approach | Status |
|--------|----------|--------|
| `ghcpcli-azarch/` | GitHub Copilot CLI — hybrid skill (hub + reference files) | ✅ Implemented |
| `claude-azarch/` | Claude — TBD | Placeholder |
| `hybrid-azarch/` | Hybrid/cross-platform — TBD | Placeholder |
| `m365-azarch/` | M365 Copilot — TBD | Placeholder |

## Shared artifacts

- `requirements.md` — Shared spec: behavioral, functional, and non-functional requirements
- `test-scenarios.md` — 18 evaluation scenarios with rubrics (T01–T18)
- `comparison.md` — Scoring matrix for comparing variants

## Knowledge sources

All implementations use the [Microsoft Learn MCP server](https://learn.microsoft.com/api/mcp) for up-to-date documentation retrieval, covering:

- Cloud Adoption Framework (CAF)
- Well-Architected Framework (WAF)
- Azure Architecture Center (reference architectures, design patterns, decision guides)
- FinOps (via WAF Cost Optimization pillar)
- Azure Verified Modules (as implementation pointers)
