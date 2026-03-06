# Azure Architecture Skill — Test Scenarios

Evaluation scenarios for validating the skill against requirements. Each test is a user prompt (sometimes multi-turn) with a rubric of expected behaviors. Run each scenario against the skill and check the rubric items.

## How to use

1. Start a fresh conversation with the agent using the skill
2. Send the **User prompt** (and follow-up turns where indicated)
3. Evaluate the agent's response(s) against each **Check** item (pass/fail)
4. A scenario passes if all checks pass

---

## BR-1: Context Before Advice

### T01 — Broad question, no context given

**User prompt:**
> "We want to build a new customer portal on Azure."

**Checks:**
- [ ] Agent asks about current state (on-prem, existing Azure footprint, identity, networking) before recommending services
- [ ] Agent asks about desired state (what the portal should do, expected users/scale, key requirements)
- [ ] Agent does not jump straight to a specific service recommendation

**Validates:** BR-1

### T02 — Rich context provided up front

**User prompt:**
> "We're a mid-size retailer with an existing Azure landing zone — hub-spoke networking, Entra ID, Azure Policy in place. We need a new inventory management API for our warehouse team. Must handle ~500 requests/sec, needs to be highly available (99.9%), and our team knows C# and has experience with App Service. Budget is moderate."

**Checks:**
- [ ] Agent does not re-ask about current Azure footprint, identity, or networking — it's all stated
- [ ] Agent proceeds to give actionable guidance using the provided context
- [ ] Agent may ask a small number of targeted follow-up questions (e.g., data store preferences) but does not repeat a full discovery

**Validates:** BR-1

### T03 — Narrow specific question

**User prompt:**
> "Should I use Service Bus or Event Grid for notifying downstream systems when an order is placed?"

**Checks:**
- [ ] Agent asks only for context directly relevant to this choice (e.g., message volume, ordering guarantees, consumer patterns) — not full environment discovery
- [ ] Agent provides a useful answer within 2–3 exchanges, not 5–10

**Validates:** BR-1

---

## BR-2: Decision Clarity and Motivation

### T04 — Compute selection with clear requirements

**User prompt (after context gathering):**
> "We need to host a web application with server-side rendering. We want minimal operational overhead. Our team is small and doesn't want to manage infrastructure."

**Checks:**
- [ ] Agent identifies multiple viable options (e.g., App Service, Container Apps, AKS, Functions)
- [ ] Agent states which user requirements drove the recommendation (minimal overhead, server-side logic, small team)
- [ ] Agent explains why alternatives were less suitable (e.g., "AKS adds operational burden your team wants to avoid")
- [ ] Recommendation is traceable back to the user's stated constraints

**Validates:** BR-2

### T05 — Close call between two services

**User prompt:**
> "We're building an event-driven microservices backend. Our team is comfortable with containers and Kubernetes, but we're open to a managed option. We expect bursty traffic — idle for hours, then thousands of requests in minutes."

**Checks:**
- [ ] Agent identifies this as a close call between at least two options (e.g., Container Apps vs. AKS)
- [ ] Agent explicitly states what would tip the decision one way or the other
- [ ] Agent does not present one option as clearly superior when it isn't

**Validates:** BR-2

---

## BR-3: Explicit Quality Attributes

### T06 — Quality attributes surfaced proactively

**User prompt (after context is established):**
> "We've decided to go with Azure App Service for our web app. What should we think about?"

**Checks:**
- [ ] Agent proactively mentions implications for multiple WAF pillars (not just the one the user might care about most)
- [ ] Reliability considerations are mentioned (e.g., deployment slots, scaling, availability zones)
- [ ] Security considerations are mentioned (e.g., managed identity, network restrictions, TLS)
- [ ] Cost implications are mentioned
- [ ] Agent uses WAF pillar terminology consistently

**Validates:** BR-3

### T07 — Cross-pillar trade-off

**User prompt:**
> "We need this system to survive a full region outage."

**Checks:**
- [ ] Agent explains the reliability benefit of multi-region architecture
- [ ] Agent explicitly states the cost and operational complexity trade-offs
- [ ] Trade-offs are framed using WAF pillar language (reliability vs. cost optimization, operational excellence)

**Validates:** BR-3

---

## FR-1: Architecture Design Guidance

### T08 — New workload architecture

**User prompt:**
> "We want to build a real-time analytics dashboard that ingests telemetry from 10,000 IoT devices, processes the data, and shows live metrics to operators."

**Checks:**
- [ ] Agent recommends specific Azure services for ingestion, processing, storage, and presentation
- [ ] Agent references an Architecture Center reference architecture or solution idea
- [ ] Agent explains how the components fit together (data flow)
- [ ] Trade-offs between approaches are mentioned

**Validates:** FR-1, BR-2

---

## FR-2: Well-Architected Review

### T09 — Standalone WAF review

**User prompt:**
> "We have an existing application running on a single App Service instance in West Europe, using Azure SQL with no geo-replication, and a storage account with LRS. Can you do a Well-Architected review?"

**Checks:**
- [ ] Agent assesses against all five WAF pillars (not just one or two)
- [ ] Reliability risks are identified (single instance, single region, LRS storage)
- [ ] Security posture is questioned (network isolation, identity, encryption)
- [ ] Cost optimization opportunities are noted
- [ ] Operational excellence gaps are raised (observability, CI/CD, IaC)
- [ ] Performance efficiency is considered
- [ ] Specific remediation steps are suggested per finding

**Validates:** FR-2, BR-3

---

## FR-3: Cloud Adoption Guidance

### T10 — Migration to Azure

**User prompt:**
> "We're a 200-person company running everything on-premises. Leadership wants to move to Azure. Where do we start?"

**Checks:**
- [ ] Agent references the Cloud Adoption Framework phases (Strategy, Plan, Ready, etc.)
- [ ] Agent suggests starting with strategy/business motivation before jumping to technical steps
- [ ] Landing zone readiness is mentioned (subscriptions, identity, networking, governance)
- [ ] Migration strategies are referenced (rehost, refactor, rearchitect, rebuild)
- [ ] Agent doesn't prescribe a single path without understanding workload portfolio

**Validates:** FR-3, BR-1

---

## FR-4: Design Pattern Recommendations

### T11 — Failure handling pattern

**User prompt:**
> "Our microservices call each other over HTTP, and when one service goes down it cascades failures across the system. How should we handle this?"

**Checks:**
- [ ] Agent recommends relevant patterns (e.g., Circuit Breaker, Retry, Bulkhead, Queue-Based Load Leveling)
- [ ] Agent explains when each pattern applies and when it doesn't
- [ ] Agent describes how patterns compose (e.g., Retry with Circuit Breaker)
- [ ] Azure-specific implementation is mentioned (e.g., using Polly, Azure Service Bus for decoupling)

**Validates:** FR-4

---

## FR-5: Iterative Refinement

### T12 — Changing constraints mid-conversation

**Turn 1 — User prompt:**
> "We're designing a web API. Moderate traffic, single region is fine."

**Turn 2 — After agent gives initial recommendation, user adds:**
> "Actually, we just learned this needs to be available in both EU and US for compliance reasons."

**Checks:**
- [ ] Agent re-evaluates its earlier recommendation in light of the new constraint
- [ ] Agent explains what changes and why (e.g., multi-region deployment, Traffic Manager / Front Door, data residency considerations)
- [ ] Agent does not ignore the new constraint or repeat the original answer

**Validates:** FR-5

### T13 — What-if exploration

**User prompt (mid-conversation):**
> "What if we halved the budget? What would you sacrifice first?"

**Checks:**
- [ ] Agent identifies which components or capabilities could be scaled back
- [ ] Agent explains the quality attribute impact of each trade-off (e.g., "dropping the secondary region saves X but reduces reliability to…")
- [ ] Agent doesn't just say "you'd need to compromise" — it gives specific guidance

**Validates:** FR-5, BR-2, BR-3

---

## FR-6: Citation and Traceability

### T14 — Recommendations include sources

**User prompt:**
> Any architecture question that results in a recommendation.

**Checks:**
- [ ] Agent references specific Microsoft Learn URLs (not invented/hallucinated ones)
- [ ] Agent distinguishes between authoritative guidance (from docs) and its own reasoning
- [ ] Agent encourages verifying against current documentation when appropriate

**Validates:** FR-6

---

## NFR-1: Grounded in Official Content

### T15 — Agent uses Learn MCP tools

**User prompt:**
> "What are the recommended practices for securing an Azure Kubernetes Service cluster?"

**Checks:**
- [ ] Agent invokes `microsoft_docs_search` or `microsoft_docs_fetch` to retrieve content (observable in tool call logs)
- [ ] Response content aligns with current Microsoft Learn documentation
- [ ] Agent does not appear to rely solely on training data for specific recommendations

**Validates:** NFR-1

---

## NFR-3: Audience Awareness

### T16 — Developer vs. architect depth

**Scenario A — User prompt:**
> "I'm a developer. How should I handle retries in my app when calling Azure SQL?"

**Scenario B — User prompt:**
> "I'm a cloud architect designing our landing zone. How should we structure subscriptions and management groups?"

**Checks:**
- [ ] Scenario A response uses code-level language, mentions libraries/SDKs, focuses on implementation
- [ ] Scenario B response uses organizational-level language, discusses topology, governance, and policy
- [ ] Depth and terminology adapt to the stated role

**Validates:** NFR-3

---

## NFR-5: Scope Boundaries

### T17 — Request for deployment artifacts

**User prompt:**
> "Can you write me the Bicep template for this architecture?"

**Checks:**
- [ ] Agent declines to generate deployment artifacts
- [ ] Agent explains this is outside scope
- [ ] Agent offers a useful alternative (e.g., points to relevant AVM modules, Architecture Center quickstarts, or code samples via the Learn MCP server)

**Validates:** NFR-5

### T18 — Non-Azure cloud question

**User prompt:**
> "How should I set this up on AWS instead?"

**Checks:**
- [ ] Agent states it covers Azure only
- [ ] Agent does not provide AWS-specific guidance
- [ ] Agent remains helpful (e.g., "I can help with the Azure equivalent if you'd like to compare")

**Validates:** NFR-5
