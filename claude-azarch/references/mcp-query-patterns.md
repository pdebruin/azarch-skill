# MCP Query Patterns

Effective query patterns for retrieving Azure architecture guidance via the Microsoft Learn MCP server. Use these as routing aids — always verify specifics by running the queries.

## General Principles

- Be specific: include service name, scenario, and intent
- Include version or SDK when relevant
- Add intent keywords: `overview`, `best practices`, `decision guide`, `reference architecture`, `quickstart`, `limits`
- For WAF content, include the pillar name: `reliability`, `security`, `cost optimization`, `operational excellence`, `performance efficiency`

## Query Patterns by Framework

### CAF Queries

```
# Landing zone and subscription design
"Azure landing zone design subscription topology"
"CAF landing zone accelerator"
"hub-spoke network topology Azure"
"Azure Virtual WAN vs hub-spoke"

# Migration
"CAF migration strategy rehost replatform refactor"
"Azure Migrate assessment"
"migration wave planning CAF"

# Governance
"Azure Policy governance CAF"
"management group hierarchy Azure"
"CAF govern cost management"

# Security (CAF Secure phase)
"CAF secure zero trust Azure"
"Microsoft Defender for Cloud recommendations"
"identity governance Entra ID CAF"

# Management
"Azure Monitor CAF manage"
"BCDR business continuity Azure"
"Azure Backup recovery CAF"
```

### WAF Queries

```
# Reliability
"WAF reliability [service name]"
"Azure [service] high availability options"
"multi-region active-active Azure"
"availability zones Azure [service]"
"Azure [service] SLA"

# Security
"WAF security [service name]"
"managed identity Azure [service]"
"private endpoint [service] Azure"
"Azure RBAC least privilege [service]"
"network security group Azure best practices"

# Cost Optimization
"WAF cost optimization [service name]"
"Azure [service] pricing tiers comparison"
"reserved instances savings plan Azure"
"autoscale cost Azure [service]"

# Operational Excellence
"WAF operational excellence [service name]"
"Azure Monitor Application Insights [service]"
"deployment slots Azure App Service"
"IaC Azure [service] best practices"

# Performance Efficiency
"WAF performance efficiency [service name]"
"Azure Cache for Redis [scenario]"
"Azure CDN static content"
"autoscale rules Azure [service]"

# Service-specific WAF guides
"Well-Architected review Azure [service name]"  # e.g., "Well-Architected review Azure Kubernetes Service"
```

### Architecture Center Queries

```
# Reference architectures
"Azure reference architecture [scenario]"         # e.g., "Azure reference architecture web app"
"microservices architecture Azure Kubernetes"
"event-driven architecture Azure"
"serverless architecture Azure Functions"

# Decision guides
"choose compute Azure App Service Functions Container Apps AKS"
"Azure data store decision guide"
"Azure messaging services comparison Service Bus Event Grid Event Hubs"
"Azure API management vs Application Gateway"

# Design patterns
"[pattern name] pattern Azure"                    # e.g., "Circuit Breaker pattern Azure"
"cloud design patterns Azure Architecture Center"
"[pattern] implementation Azure"

# Solution ideas
"[industry/scenario] solution Azure"              # e.g., "retail e-commerce solution Azure"
```

### Service Selection Queries

When helping a user choose between services, use comparison-focused queries:

```
# Compute
"Azure compute decision tree App Service Functions Container Apps AKS VMs"
"Azure App Service vs Container Apps comparison"
"Azure Functions vs Container Apps when to use"
"AKS vs Container Apps trade-offs"

# Data
"Azure SQL Database vs SQL Managed Instance comparison"
"Cosmos DB vs Azure SQL when to use"
"Azure Storage options blob table queue comparison"
"Azure Cache Redis vs in-memory caching"

# Messaging
"Azure Service Bus vs Event Grid vs Event Hubs comparison"
"when to use Service Bus vs Event Grid"
"Azure messaging services decision guide"

# Networking
"Azure Application Gateway vs Azure Front Door"
"API Management vs Application Gateway"
"Azure Firewall vs Network Security Groups"
"ExpressRoute vs VPN Gateway comparison"

# Identity
"Azure AD B2C vs External ID comparison"
"managed identity system-assigned vs user-assigned"
"Azure RBAC vs Azure AD roles"
```

## Fetch Patterns

Use `microsoft_docs_fetch` when:

1. **Decision guides** — fetch the full page to get complete comparison tables and decision trees
2. **Reference architectures** — fetch for component diagrams and full design rationale
3. **Design patterns** — fetch for full trade-off analysis, when-to-use, and when-not-to-use
4. **WAF service guides** — fetch for complete pillar-by-pillar assessment checklists
5. **CAF phase documentation** — fetch for step-by-step guidance on complex phases like Ready or Govern

## Code Sample Queries

Use `microsoft_code_sample_search` for:

```
# AVM modules
"Azure Verified Module bicep [resource type]"    # e.g., "Azure Verified Module bicep app service"
"AVM terraform [resource type]"

# SDK patterns (when user asks about application code)
"[service] managed identity [language]"          # e.g., "Key Vault managed identity python"
"[service] retry pattern [language]"
```
