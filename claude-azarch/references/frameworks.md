# Azure Framework Reference

This file helps route questions to the right framework and content. Use it to determine what to search for — always verify specifics via the MCP tools rather than relying on the static content here.

## Cloud Adoption Framework (CAF)

CAF covers the organizational and technical journey to Azure adoption. Route questions here when the user is concerned with strategy, migration planning, landing zones, governance, or operational maturity.

| Phase | Covers | Example Questions |
|-------|--------|-------------------|
| **Strategy** | Business motivations, expected outcomes, stakeholder alignment | "Why move to Azure?", "How do I build a business case?" |
| **Plan** | Digital estate assessment, migration waves, skills readiness, adoption plan | "What should I migrate first?", "How do I assess my workloads?" |
| **Ready** | Landing zone design, subscription topology, hub-spoke networking, governance baseline, identity foundation | "How should I structure subscriptions?", "What's a landing zone?", "Hub-spoke or VWAN?" |
| **Adopt — Migrate** | Migration strategies (rehost/lift-and-shift, replatform, refactor), migration tools | "How do I move VMs to Azure?", "Azure Migrate vs manual?" |
| **Adopt — Innovate** | Modernization paths, cloud-native development, AI/ML adoption | "How do I modernize my .NET app?", "Cloud-native patterns?" |
| **Govern** | Policy framework, cost management, resource consistency, security baseline | "How do I enforce tagging?", "Azure Policy vs Blueprints?" |
| **Secure** | Zero trust, identity, network security, threat protection, data protection | "How do I implement zero trust?", "Defender for Cloud?" |
| **Manage** | Monitoring, backup, business continuity, incident response, operational fitness | "How do I set up monitoring?", "What's my BCDR strategy?" |

**Search targets:** `learn.microsoft.com/azure/cloud-adoption-framework`

## Well-Architected Framework (WAF)

WAF covers the quality of individual workloads across five pillars. Route questions here when the user is concerned with workload design quality, reliability, security posture, cost, operations, or performance.

| Pillar | Covers | Example Questions |
|--------|--------|-------------------|
| **Reliability** | Availability targets, failure mode analysis, resilience patterns, multi-region, backup and recovery | "How do I make this highly available?", "What RTO/RPO should I target?", "Circuit breaker pattern?" |
| **Security** | Identity and access (managed identity, RBAC), network controls (private endpoints, NSGs, WAF), data protection, key management, threat detection | "How do I secure my App Service?", "Should I use managed identity?", "Private endpoint vs service endpoint?" |
| **Cost Optimization** | Right-sizing, reserved instances, autoscaling, FinOps practices, cost allocation, commitment discounts | "How do I reduce my Azure bill?", "When should I use reserved instances?", "How do I right-size VMs?" |
| **Operational Excellence** | Observability (Azure Monitor, Log Analytics, Application Insights), CI/CD, IaC, deployment safety (slots, canary), runbooks | "How do I set up observability?", "Deployment slots?", "How do I manage IaC?" |
| **Performance Efficiency** | Scaling (horizontal vs vertical, autoscale), caching (Redis, CDN), database performance, latency optimization, load testing | "How do I scale my API?", "When should I use Redis Cache?", "Database bottleneck?" |

**Search targets:** `learn.microsoft.com/azure/well-architected`

WAF also publishes service guides (e.g., "Well-Architected review for Azure Kubernetes Service") — search for these when reviewing a specific service.

## Azure Architecture Center

The Architecture Center covers reference architectures, design patterns, and technology decision guides. Route questions here when the user needs a complete solution blueprint, a pattern recommendation, or a decision guide for choosing between services.

| Content Type | Covers | Search For |
|---|---|---|
| **Reference architectures** | End-to-end solution designs with component diagrams | "web app reference architecture Azure", "microservices on AKS", "event-driven architecture Azure" |
| **Solution ideas** | Scenario-specific blueprints | "e-commerce on Azure", "IoT solution Azure", "data analytics platform" |
| **Cloud design patterns** | Reusable patterns with trade-offs and implementation guidance | "Circuit Breaker pattern Azure", "CQRS pattern", "Saga pattern", "Strangler Fig" |
| **Technology decision guides** | Structured comparisons to choose between services | "choose compute Azure", "choose data store Azure", "messaging services comparison" |
| **Best practices** | Service-specific and scenario-specific guidance | "API design best practices Azure", "microservices design guidance" |

**Search targets:** `learn.microsoft.com/azure/architecture`

### Key Design Patterns to Know

When recommending patterns, fetch the full Architecture Center page for current trade-offs and implementation notes. Common patterns include:

- **Retry / Circuit Breaker / Bulkhead** — Resilience patterns for distributed systems
- **Queue-Based Load Leveling** — Decouple producers and consumers
- **CQRS** — Separate read and write models
- **Event Sourcing** — Persist state as a sequence of events
- **Saga** — Manage distributed transactions
- **Gateway Aggregation / Offloading / Routing** — API gateway patterns
- **Strangler Fig** — Incremental modernization
- **Sidecar / Ambassador** — Container-based extensibility
- **Competing Consumers** — Scale message processing
- **Priority Queue** — Differentiated processing tiers

## Azure Verified Modules (AVM)

AVM provides standardized, Microsoft-maintained IaC modules for Bicep and Terraform that codify WAF best practices. This skill is advisory only — when users ask about implementation artifacts, point them to AVM rather than generating modules yourself.

- Bicep modules: `github.com/Azure/bicep-registry-modules`
- Terraform modules: registry under `Azure/` on the Terraform Registry
- Documentation: `azure.github.io/Azure-Verified-Modules`

**Search targets:** `microsoft_code_sample_search` with query terms like "AVM bicep [resource]" or "Azure Verified Module [resource]"

## FinOps

FinOps principles are covered primarily through the WAF Cost Optimization pillar. When users raise cloud financial management questions, route to WAF Cost Optimization first, then supplement with FinOps Foundation content if needed.

Topics include: cost allocation tagging, showback/chargeback, unit economics, commitment discounts (reserved instances, savings plans), rightsizing, and idle resource elimination.
