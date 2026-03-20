# ghcpcli-azarch — Azure Architecture Skill for GitHub Copilot CLI

A hybrid agent skill providing Azure architecture advisory guidance, built for GitHub Copilot using the [agentskills.io](https://agentskills.io/) format.

## Architecture

**Hybrid pattern**: thin hub SKILL.md (~170 lines) handles behavioral rules and routing; 4 reference files provide deep content loaded on demand. Total ~725 lines.

```
ghcpcli-azarch/
├── SKILL.md                              ← Hub: behavior, decision tree, scope, MCP usage
└── references/
    ├── DESIGN-GUIDANCE.md                ← Service selection, reference architectures
    ├── WELL-ARCHITECTED.md               ← WAF review methodology, 5-pillar assessment
    ├── CLOUD-ADOPTION.md                 ← CAF phases, landing zones, migration strategies
    └── DESIGN-PATTERNS.md                ← Pattern catalog, composition, Azure implementation
```

## Key design decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Monolith vs. modular | Hybrid (hub + references) | Domain is large (~5 areas); modular saves tokens on activation while keeping depth available on demand |
| Behavioral rules location | All in hub | Context-gathering, decision clarity, and quality attributes are cross-cutting — every response needs them regardless of topic |
| Service catalogs in references | Yes, as routing aids | Agent needs to know *what to search for* via MCP; catalogs are scaffolds, not authoritative sources |
| MCP-first approach | Golden rule in hub | Ensures currency; skill contains procedures not cached facts |
| Audience model | Infer hat from question | Users wear multiple hats; fixed personas don't fit agent conversations |

## Requirements coverage

All 14 requirements (BR-1–3, FR-1–6, NFR-1–5) from `requirements.md` are addressed. All 18 test scenarios from `test-scenarios.md` pass validation. See `comparison.md` for the scoring matrix.

## How to use

Install as an agent skill in a GitHub Copilot-compatible environment. The skill requires access to the [Microsoft Learn MCP server](https://learn.microsoft.com/api/mcp) for documentation retrieval (`allowed-tools` in frontmatter).

The skill is **advisory only** — it does not generate deployment artifacts, run CLI commands, or access Azure subscriptions.
