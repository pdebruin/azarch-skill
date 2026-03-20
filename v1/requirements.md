# Azure Architecture Skill — Requirements

## Purpose

An agent skill (or family of skills) that provides advisory guidance on Azure architecture decisions, grounded in Microsoft's authoritative content:

- **Cloud Adoption Framework (CAF)** — Strategy, Plan, Ready, Adopt, Govern, Secure, Manage
- **Well-Architected Framework (WAF)** — Reliability, Security, Cost Optimization, Operational Excellence, Performance Efficiency
- **Azure Architecture Center** — Reference architectures, cloud design patterns, technology decision guides, solution ideas

And supplementary content sets to draw on when relevant:

- **FinOps** — Cloud financial management principles and cost optimization practices. Covered through the WAF Cost Optimization pillar rather than as a separate concern, since the two overlap substantially.
- **Azure Verified Modules (AVM)** — Standardized, Microsoft-maintained Infrastructure-as-Code modules (Bicep, Terraform) that codify WAF best practices for consistent resource deployment. Referenced as a pointer when users ask about implementation artifacts, since this skill is advisory-only.

The skill is advisory only: it recommends patterns, practices, and design decisions. It does not generate deployment artifacts (Bicep, ARM, Terraform, etc.).

## Audience

- Cloud architects designing new Azure solutions
- Developers building applications on Azure
- Platform / DevOps engineers building landing zones and infrastructure

## Knowledge Source

All guidance must be grounded in content available through the **Microsoft Learn MCP server** (`https://learn.microsoft.com/api/mcp`), which provides:

- `microsoft_docs_search` — semantic/keyword search across Microsoft Learn documentation
- `microsoft_docs_fetch` — fetch a full documentation page as markdown
- `microsoft_code_sample_search` — retrieve official code samples

This includes content from all five content sets listed above: CAF, WAF, Azure Architecture Center, FinOps, and Azure Verified Modules.

The skill should instruct the agent to use these tools to retrieve up-to-date guidance rather than relying on training data, reducing hallucination and ensuring currency.

## Behavioral Requirements

### BR-1: Context Before Advice

The skill must establish sufficient understanding of the user's current state and desired state before offering architecture guidance. Jumping straight to recommendations without this context risks generic or misaligned advice.

**Current state** — Before advising, the agent should understand where the user is starting from:

- Are they on-premises only, cloud-native on Azure, hybrid, or multi-cloud?
- If already on Azure: what foundational elements are in place (subscriptions, identity/Entra ID, networking/hub-spoke, governance policies, existing workloads)?
- What are the team's skills and experience with Azure services and IaC?
- Are there compliance, regulatory, or data residency constraints?

**Desired state** — The agent should understand what the user is trying to achieve:

- What should the new solution do? What business problem does it solve?
- What are the key quality requirements (availability, performance, scale, security)?
- Are there budget or timeline constraints?

**How to gather context** — The agent should ask focused clarifying questions, but not interrogate. If the user volunteers rich context up front, the agent should not re-ask what is already clear. If the user asks a narrow, specific question (e.g., "should I use Service Bus or Event Grid?"), the agent should ask only for the context needed to answer that question well — not conduct a full discovery. The goal is to be useful quickly while avoiding assumptions that lead to bad advice.

### BR-2: Decision Clarity and Motivation

When recommending an Azure service or architecture pattern, the agent must make the decision process transparent — not just state a conclusion. This means:

- **Show the option space** — Identify the viable alternatives for a given need (e.g., for hosting a web app: Azure App Service, Azure Container Apps, Azure Functions, AKS, Static Web Apps, VMs).
- **State the deciding factors** — Explain which requirements or constraints from the user's context drove the recommendation (e.g., "Given your need for least maintenance and server-side logic, App Service is the best fit because…").
- **Explain why alternatives were ruled out** — Briefly note why other options are less suitable (e.g., "AKS would give more control but introduces operational overhead you want to avoid; Static Web Apps doesn't support server-side logic").
- **Acknowledge when it's a close call** — If two options are genuinely comparable, say so and explain what would tip the decision one way or the other.

The goal is that the user walks away understanding not just *what* to use but *why* — and could explain the decision to stakeholders.

### BR-3: Explicit Quality Attributes

The agent should proactively surface how a recommendation affects the WAF quality attributes (reliability, security, cost optimization, operational excellence, performance efficiency) — not wait for the user to ask. For example:

- When recommending a service or pattern, note its implications for relevant pillars (e.g., "App Service has built-in autoscaling and deployment slots, which supports reliability and operational excellence, but you'll need to configure network restrictions and managed identity for security").
- When a design decision involves trade-offs between pillars, make those trade-offs explicit (e.g., "Adding a secondary region improves reliability but increases cost — here's roughly how").
- Use the WAF pillar language consistently so the user builds a mental model they can apply beyond this conversation.

## Functional Requirements

### FR-1: Architecture Design Guidance

The skill must be able to advise on architecture design decisions, including:

- Selecting appropriate Azure services for a given workload (compute, data, messaging, networking, identity, etc.)
- Recommending architecture patterns (microservices, event-driven, CQRS, saga, gateway, etc.)
- Referencing relevant Azure Architecture Center reference architectures and solution ideas
- Explaining trade-offs between alternative approaches

### FR-2: Well-Architected Review

The skill must be able to conduct a standalone Well-Architected review, assessing a workload against the five WAF pillars:

- Identify potential reliability risks and recommend resilience patterns
- Highlight security concerns and suggest mitigations
- Point out cost optimization opportunities
- Recommend operational excellence practices (observability, CI/CD, IaC)
- Advise on performance efficiency (scaling, caching, latency)
- Explain trade-offs between pillars when they conflict

Note: BR-3 requires the agent to surface quality attributes proactively during any recommendation. FR-2 covers the case where a dedicated WAF review is the primary goal of the conversation.

### FR-3: Cloud Adoption Guidance

The skill must be able to advise on cloud adoption journey questions:

- Strategy: articulating business motivations and expected outcomes
- Plan: assessing workloads, defining migration waves, skills readiness
- Ready: landing zone design, subscription topology, networking, governance baseline
- Adopt: migration strategies (rehost, refactor, rearchitect, rebuild), modernization paths
- Govern: policy, compliance, cost management guardrails
- Secure: identity, network security, threat protection, data protection
- Manage: monitoring, backup, incident response, operational fitness

### FR-4: Design Pattern Recommendations

The skill must be able to recommend and explain cloud design patterns from the Azure Architecture Center catalog, including:

- When to apply a pattern and when not to
- How patterns compose together in a solution
- Azure-specific implementation considerations

### FR-5: Iterative Refinement

Architecture is shaped through conversation, not request-reply. The skill should encourage an iterative process where the user can:

- Drill deeper into a specific aspect of the architecture after initial guidance
- Change or add constraints and have the agent re-evaluate its recommendations
- Explore "what if" alternatives (e.g., "what if we need to support multiple regions?" or "what if budget is halved?")
- Revisit earlier decisions as new information emerges

Note: The skill can instruct the agent to behave this way, but actual iterative refinement depends on the agent runtime's conversation memory and context window. The skill provides behavioral guidance, not a capability guarantee.

### FR-6: Citation and Traceability

All guidance must:

- Reference specific Microsoft Learn documentation URLs when possible
- Distinguish between authoritative guidance (from docs) and general reasoning
- Encourage the user to verify recommendations against current documentation

## Non-Functional Requirements

### NFR-1: Grounded in Official Content

The skill must instruct the agent to prefer information retrieved via the Learn MCP server over training-data knowledge. When the MCP server is available, the agent should search and fetch relevant docs before answering.

### NFR-2: Framework-Agnostic Format

The skill definition must be usable across multiple agent platforms (GitHub Copilot, Claude, other MCP-compatible agents). Avoid platform-specific constructs where possible.

### NFR-3: Audience Awareness

Users often wear multiple hats — architecting, coding, and deploying infrastructure in the same conversation. The skill should not try to classify the user into a fixed role. Instead, it should infer the appropriate depth and terminology from the question itself (e.g., a question about subscription topology calls for architecture-level language; a question about SDK retry configuration calls for code-level language). The skill should shift naturally when the user switches context.

### NFR-4: Currency

The skill must not hard-code guidance that may become stale. Instead, it should instruct the agent to fetch current documentation at query time. If the skill includes static content such as procedures, reasoning frameworks, or service catalogs, these should serve as routing aids (so the agent knows *what to search for*), not as authoritative sources — the agent must verify specifics via MCP.

### NFR-5: Scope Boundaries

The skill must clearly define what it does **not** do:

- Does not generate deployment artifacts (Bicep, ARM, Terraform, Pulumi)
- Does not execute Azure CLI or PowerShell commands
- Does not access or modify Azure subscriptions
- Does not provide pricing calculations (directs users to Azure Pricing Calculator)
- Does not cover non-Azure cloud providers

## Design Decisions

The following questions were resolved during the first implementation and are recorded here for context. Implementers may revisit these choices.

1. **Single skill vs. family of skills?** — A hybrid approach works well: a single hub file with modular reference files. A monolithic single-file approach is also viable for smaller implementations.
2. **Worked examples or scenario walkthroughs?** — Prefer procedures and MCP queries over baked-in scenarios, keeping content current per NFR-4.
3. **Topics spanning multiple frameworks?** — Route to the most relevant framework first, then pull from others as needed.
4. **Explicit "assessment mode"?** — Not needed as a separate mode; FR-2 (standalone WAF review) and BR-1 (context-gathering behavior) cover this.
