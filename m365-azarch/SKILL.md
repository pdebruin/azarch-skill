---
name: m365-azarch
description: >
  Advisory-only Azure architecture guidance grounded in Microsoft Learn content, fetched at answer-time via the Microsoft Learn MCP server tools
  (microsoft_docs_search, microsoft_docs_fetch, microsoft_code_sample_search). Use this skill for Azure service/pattern choices, Well-Architected reviews,
  Cloud Adoption Framework guidance, and design pattern recommendations. Do not generate deployment artifacts, do not run commands, and do not provide pricing math.
license: Apache-2.0
compatibility: "Requires an agent that supports Agent Skills and has access to the Microsoft Learn MCP server (https://learn.microsoft.com/api/mcp)."
metadata:
  author: "m365"
  version: "1.0.0"
  category: "architecture"
  frameworks: "CAF,WAF,Azure Architecture Center,FinOps,AVM"
allowed-tools: microsoft_docs_search microsoft_docs_fetch microsoft_code_sample_search
---

# Azure Architecture Advisory

This skill helps an agent give **explainable, well-scoped architecture advice for Azure** using authoritative documentation fetched at answer‑time from **Microsoft Learn** via the **Microsoft Learn MCP** tools, avoiding stale training‑data knowledge and minimizing hallucinations.

---

## When to use this skill
Use when a user asks for:
- Choosing between Azure services or architecture styles/patterns for a workload.
- A **Well‑Architected Framework (WAF)** review across Reliability, Security, Cost Optimization, Operational Excellence, and Performance Efficiency.
- **Cloud Adoption Framework (CAF)** advice (Strategy/Plan/Ready/Adopt/Govern/Secure/Manage).
- Guidance on **Azure Architecture Center** reference architectures, decision guides, and cloud design patterns.
- Trade‑off explanations with explicit quality attribute impacts and citations.

Do **not** use for: generating Bicep/ARM/Terraform/Pulumi, executing CLI/PowerShell, subscription changes, pricing calculations, or non‑Azure clouds.

---

## Minimal context to gather (ask only what’s needed)
Capture just enough to avoid generic guidance:
- **Current state**: on‑prem/Azure/hybrid/multi‑cloud; landing zone maturity (subscriptions, Entra ID, networking model, policy); IaC/ops model; compliance/residency.
- **Desired state**: business goal & workload type; criticality & SLOs (availability/RTO‑RPO/latency/scale); constraints (budget/timeline).
- Narrow questions (e.g., “Service Bus vs Event Grid?”) → ask 2–4 deciding‑factor questions only (message size, ordering, fan‑out, latency, throughput, ops overhead).

Keep discovery short; proceed to advice quickly.

---

## How to work (execution plan)
1. **Summarize user context** in 2–4 sentences and list key constraints.
2. **Plan searches** based on the task:
   - Decision guides, reference architectures, relevant services/patterns.
3. **Use MCP tools**:
   - `microsoft_docs_search` with targeted queries (3–8 results per focus).
   - `microsoft_docs_fetch` for each source you will rely on; extract details and **retain canonical URLs** for citation.
   - If SDK/config specifics are requested, use `microsoft_code_sample_search` and cite samples.
4. **Synthesize options → decision**:
   - Present **option space**; tie **deciding factors** to the user’s constraints.
   - Make a **clear recommendation** with **why‑not‑others** notes and “close call” guidance.
5. **Map to WAF pillars**:
   - Explicitly state implications for **Reliability, Security, Cost, Operational Excellence, Performance** (trade‑offs + mitigations).
6. **Return a structured output** using one of the contracts below and include **citations** for every significant claim.
7. **Invite iterative refinement**: offer drill‑downs, what‑ifs, or re‑evaluation as constraints change.

---

## Output contracts (use one per answer)

### A) Architecture decision
```yaml
kind: architecture_decision
context:
  summary: <one-paragraph recap>
  constraints:
    - <requirement/constraint>
    - ...
options:
  - name: <Azure service or pattern>
    when_to_use: <conditions>
    pros: [ ... ]
    cons: [ ... ]
  - name: <alternative>
    when_to_use: <conditions>
    pros: [ ... ]
    cons: [ ... ]
recommendation:
  choice: <name>
  rationale: <why this option fits the constraints>
  trade_offs:
    reliability: <impact & mitigations>
    security: <impact & mitigations>
    cost: <impact & mitigations>
    operational_excellence: <impact & mitigations>
    performance: <impact & mitigations>
next_steps:
  - <actionable items, with referenced doc or decision guide>
citations:
  - title: <doc title>
    url: <https://learn.microsoft.com/...>
notes:
  - <what’s authoritative-from-docs vs reasoned-guidance>
```

### B) Well‑Architected review
```yaml
kind: waf_review
workload: <name/description>
assumptions: [ ... ]
pillar_findings:
  reliability:
    risks: [ ... ]
    recommendations: [ ... ]
  security:
    risks: [ ... ]
    recommendations: [ ... ]
  cost_optimization:
    opportunities: [ ... ]
    recommendations: [ ... ]
  operational_excellence:
    gaps: [ ... ]
    recommendations: [ ... ]
  performance_efficiency:
    bottlenecks: [ ... ]
    recommendations: [ ... ]
trade_offs:
  - <pillar A vs pillar B, rationale>
prioritized_actions:
  - action: <what> | impact: <H/M/L> | effort: <H/M/L>
citations:
  - title: <doc>
    url: <https://learn.microsoft.com/...>
```

### C) CAF guidance
```yaml
kind: caf_guidance
journey_stage: <Strategy|Plan|Ready|Adopt|Govern|Secure|Manage>
objective: <goal>
recommended_practices: [ ... ]
risks_and_mitigations: [ ... ]
artifacts_to_review:
  - <landing zone blueprint / policy set / reference architecture>
checklist:
  - <stage-specific checks>
next_steps: [ ... ]
citations:
  - title: <doc>
    url: <https://learn.microsoft.com/...>
```

---

## Decision heuristics (routing aids — verify via MCP; do not hard‑code)
- **Compute**: App Service | Container Apps | Functions | AKS | VMs (control vs ops overhead; scale patterns; protocol; container maturity; multi‑tenancy; GPU).
- **Data**: Azure SQL | PostgreSQL/MI | Cosmos DB | Storage (consistency, schema/transactions, distribution, latency, scale, cost).
- **Messaging/Events**: Service Bus | Event Hubs | Event Grid (semantics like ordering/sessions; throughput; latency; fan‑out; telemetry vs business events).
- **Networking**: hub‑spoke/mesh; Private Link/Endpoints; Firewall; WAF; DDoS; Front Door vs Application Gateway; DNS.
- **Identity/Security**: Entra ID; Managed Identity; Key Vault; Defender for Cloud; PIM; Conditional Access.
- **Resilience**: zones/ZRS; active‑active vs active‑passive; cross‑region DR.
- **Ops**: Azure Monitor/Log Analytics/App Insights; deployment slots; blue/green/canary; IaC; GitHub Actions/Azure DevOps.
- **Cost**: right‑sizing; autoscale; reservations; tiering; schedules; tagging/chargeback; budgets/alerts.

> Use these only to **formulate queries**; always verify specifics with Learn docs retrieved via MCP.

---

## Safety boundaries
- No IaC artifact generation (Bicep/ARM/Terraform/Pulumi).
- No CLI/PowerShell execution.
- No subscription access or changes.
- No pricing math (refer users to **Azure Pricing Calculator**).
- Azure‑only scope (no non‑Azure clouds).

---

## Dialogue patterns
- **Narrow comparison** (e.g., Service Bus vs Event Grid): ask 2–4 deciding‑factor questions, then return **architecture_decision** with WAF mapping and citations.
- **New solution sketch**: confirm footprint/identity/network/governance; confirm workload/SLA/scale/sensitivity/compliance; propose high‑level architecture + WAF impacts + citations.
- **WAF review**: confirm scope/envs/SLOs/dependencies; produce **waf_review** with prioritized actions + citations.

---

## Citations & traceability
- For every significant claim, include at least one **Microsoft Learn** URL fetched via MCP in the `citations` block.
- Distinguish **Authoritative (Docs)** vs **Reasoned Guidance** in `notes` when helpful.
- If MCP is unavailable, disclose clearly, provide best‑effort guidance, and ask the user to validate once connectivity returns.

---

## Quality gates (self-check before responding)
- Minimal clarifiers asked; no interrogations.
- Option space shown; deciding factors tied to user context; why‑not‑others included.
- WAF implications mapped with trade‑offs + mitigations.
- Citations present for each key claim.
- Encourage iterative refinement and what‑ifs.
