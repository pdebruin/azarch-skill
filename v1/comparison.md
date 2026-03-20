# Deep Section-Level Comparison: claude vs ghcpcli vs m365

## 1. Frontmatter / Metadata

| | claude | ghcpcli | m365 |
|---|---|---|---|
| `name` | ✅ | ✅ | ✅ |
| `description` | ✅ (long, trigger-phrase-rich) | ✅ (concise, keyword-rich) | ✅ (concise, tool-aware) |
| `license` | ❌ missing | ✅ Apache-2.0 | ✅ Apache-2.0 |
| `compatibility` | ✅ one-liner | ✅ multi-line, explicit "no subscription needed" | ✅ one-liner |
| `metadata` | ❌ missing | ✅ author, version, domain | ✅ author, version, category, frameworks |
| `allowed-tools` | ❌ missing | ✅ explicit | ✅ explicit |

**Verdict:** ghcpcli and m365 are more complete. claude is missing `license`, `metadata`, and `allowed-tools` — an agent framework that uses these fields for tool filtering or skill discovery won't work optimally.

---

## 2. Activation / "When to use this skill"

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Where** | Embedded in description frontmatter (trigger phrases) | Dedicated "When to use" + "Do not activate" sections | Dedicated "When to use" + inline "Do not use for" |
| **Positive triggers** | Trigger phrases in description | Bulleted list of user intents | Bulleted list of user intents |
| **Negative triggers** | Described in "Scope Boundaries" table later | Explicit "Do not activate when" list right next to positive triggers | One-liner "Do not use for" list |

**Verdict:** ghcpcli is strongest — positive and negative triggers are co-located, making it easy for an agent to pattern-match. m365 is close but the negative triggers are compressed into a single line. claude relies on the description field and a scope table further down, splitting activation logic across two locations.

---

## 3. Scope Boundaries / Guard Rails (NFR-5)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Format** | Table with "Out of Scope" + "Redirect" columns | Integrated into "Do not activate" list + separate redirect in-context | Dedicated "Safety boundaries" section (bullet list) |
| **Redirect guidance** | ✅ Specific redirects per boundary (AVM, Pricing Calculator, etc.) | ✅ Redirects inline with each boundary | ⚠️ Only mentions Pricing Calculator; no AVM redirect |
| **Placement** | End of file | Top of file (near activation) | Near end of file |

**Verdict:** claude's table format is the clearest — each boundary paired with where to redirect. ghcpcli integrates it well near activation. m365's "Safety boundaries" is a bare list without redirects (except pricing), so the agent won't know what to suggest instead.

---

## 4. Context Gathering (BR-1)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Structure** | Step 1 of workflow. Two labeled blocks: "Current state" + "Desired state" with bullet lists | Dedicated section. Split by question breadth: "broad questions" vs "narrow questions" + 4 explicit rules | Compact section. Bullet-compressed current/desired state + narrow question example |
| **Adaptive depth** | ✅ "If narrow, ask only what's needed" (prose) | ✅ Explicit narrow vs broad distinction + "aim to be useful within 2-3 exchanges" rule | ✅ "ask 2-4 deciding-factor questions only" for narrow |
| **Anti-interrogation** | "Do not interrogate" | 4 numbered rules including "state assumptions and proceed" | "Keep discovery short; proceed to advice quickly" |

**Verdict:** ghcpcli is most actionable — the broad/narrow split with numbered rules gives the agent clear decision criteria. The "state assumptions and proceed" rule (rule 4) is uniquely practical. claude is thorough but longer. m365 is the most compressed — efficient but risks being too terse for an agent to follow well on edge cases.

---

## 5. MCP Tool Usage (NFR-1, NFR-4)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Tool documentation** | Table in SKILL.md + full reference file `mcp-query-patterns.md` (163 lines of example queries) | Table with example queries in SKILL.md + search strategy section | Numbered execution plan steps 2-3 |
| **Query examples** | ✅ Extensive — organized by framework (CAF, WAF, Arch Center), by service category, with fetch and code sample patterns | ✅ Moderate — one example per tool in hub, more in reference files | ⚠️ None — says "targeted queries" but gives no examples |
| **Fallback behavior** | Not explicitly stated in hub | ✅ "If MCP tools are unavailable, state that recommendations should be verified" (golden rule) | ✅ "If MCP is unavailable, disclose clearly, provide best-effort guidance" |
| **Search strategy** | Deferred to reference file | ✅ "Search before answering", "Use multiple searches if spans topics", "Fetch top results" | "3-8 results per focus" (specific count) |

**Verdict:** claude has the richest MCP guidance through its `mcp-query-patterns.md` reference (163 lines of concrete queries organized by framework and category). ghcpcli has good inline guidance plus reference files with `Live documentation` tables containing MCP queries. m365 is the weakest here — it tells the agent to search but provides zero query examples, so the agent has to invent queries from scratch.

---

## 6. Decision Clarity (BR-2)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Structure** | Step 3 of workflow: 4 numbered items | Dedicated section: 4 numbered items | Step 4 of execution plan: inline prose |
| **Content** | Option space → deciding factors → why-not-others → close calls | Identical 4-point structure | Same concepts but compressed into prose ("option space", "deciding factors", "why-not-others", "close call") |
| **Stakeholder test** | ✅ "be able to explain it to stakeholders" | ✅ "understand *why*, not just *what*" | ❌ Not stated |

**All three cover the same four requirements.** claude and ghcpcli use identical structure (numbered list). m365 compresses it into step 4 of its execution plan, which is less scannable. The stakeholder framing in claude/ghcpcli is a nice touch — it gives the agent a calibration target for depth.

---

## 7. Quality Attributes / WAF Pillars (BR-3)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Format** | Step 4: bullet list of 5 pillars with brief scope | Table: pillar × "Always consider" with specific items per pillar | Step 5: single sentence listing all 5 + "(trade-offs + mitigations)" |
| **Specificity** | Moderate (e.g., "Reliability — Availability targets, resilience patterns, multi-region") | High (e.g., "Reliability: Redundancy, failover, recovery time, availability zones, SLAs") | Low (just pillar names) |
| **Trade-off instruction** | ✅ Explicit example: "Adding a secondary region improves reliability but increases cost" | ✅ Same example + "Use WAF pillar terminology consistently" | ✅ "trade-offs + mitigations" but no example |

**Verdict:** ghcpcli's table format is the most useful — it gives the agent a concrete checklist per pillar to verify it hasn't missed anything. claude is solid with its example trade-off. m365 is too compressed to be actionable; an agent would need prior knowledge of what each pillar entails.

---

## 8. Architecture Design Guidance (FR-1)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Hub coverage** | 4-bullet summary | Routes to `DESIGN-GUIDANCE.md` | Inline "Decision heuristics" section |
| **Reference depth** | Deferred to `frameworks.md` (Architecture Center section) | **107-line reference** with: service selection methodology (3-step), workload category table, decision framework (5 priority-ordered factors), MCP decision tree queries, architecture composition (data flow + cross-cutting concerns), FinOps, AVM | 10-line bullet list of service categories with deciding factors in parentheses |
| **Service selection method** | Not structured | ✅ 3-step: identify category → apply decision framework → use Architecture Center decision trees | ❌ No method — just lists services |
| **MCP query guidance** | Via `mcp-query-patterns.md` | ✅ Table of decision tree queries per category | ❌ "verify via MCP" but no queries |

**Verdict:** ghcpcli is significantly deeper — it provides a structured methodology for service selection with priority-ordered decision factors and specific MCP queries to find decision trees. claude defers to reference files which cover it well. m365's decision heuristics are useful as a memory-jogger but lack methodology — the agent knows *what* services exist but not *how* to choose between them.

---

## 9. Well-Architected Review (FR-2)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Hub coverage** | 2-line summary, defers to `frameworks.md` | Routes to `WELL-ARCHITECTED.md` | Output contract (YAML schema) |
| **Reference depth** | `frameworks.md` has pillar table with example questions and search targets (34 lines) | **166-line reference** with: review approach (standalone + deep-dive), per-pillar assessment guides (key questions + common findings/remediation tables), trade-off patterns table, MCP queries | ❌ No reference — just the output schema |
| **Assessment questions** | ❌ Not provided | ✅ 6-7 key questions per pillar | ❌ Not provided |
| **Finding → remediation** | ❌ Not provided | ✅ Tables per pillar with finding, risk, and remediation | ❌ Not provided |
| **Trade-off patterns** | Mentioned but not cataloged | ✅ Table: Reliability↔Cost, Security↔Ops, Performance↔Cost, Reliability↔Performance with examples | ❌ Schema has `trade_offs` field but no guidance on what goes in it |

**Verdict:** ghcpcli is far ahead — its WAF reference is essentially a mini assessment toolkit with questions to ask, findings to look for, and remediations to suggest, plus trade-off patterns. claude's reference is thinner but covers the basics. m365 defines the *output format* (YAML schema) but provides zero guidance on *how to conduct the review* — it assumes the agent already knows how to do a WAF assessment.

---

## 10. Cloud Adoption Guidance (FR-3)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Hub coverage** | 2-line summary, defers to `frameworks.md` | Routes to `CLOUD-ADOPTION.md` | ❌ No dedicated section (only mentioned in "When to use") |
| **Reference depth** | `frameworks.md` has phase table with coverage areas, example questions, search targets (20 lines) | **169-line reference** with: phase overview table, per-phase guidance (Strategy: motivations/outcomes/business case; Plan: assessment/waves/skills; Ready: landing zone components/approaches/subscription topology; Adopt: 5 R's + tooling; Govern: mechanisms table; Secure: 5 domains; Manage: 5 areas), MCP queries per phase | ❌ None |
| **Landing zone depth** | Not covered in detail | ✅ Component table, 3 approaches (start small / enterprise-scale / custom), subscription topology patterns (4 options) | ❌ None |
| **Migration strategies** | Not detailed | ✅ 5 R's table with descriptions and when-to-use, plus Replace/Retain, migration tooling | ❌ None |

**Verdict:** ghcpcli has comprehensive CAF coverage — its reference file is essentially a CAF cheat sheet with actionable content for each phase. claude's `frameworks.md` provides routing aids and example questions but less depth. m365 has no CAF-specific content at all beyond mentioning it in the activation criteria — a major gap.

---

## 11. Design Patterns (FR-4)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Hub coverage** | 3-bullet summary (when to apply, composition, Azure considerations) | Routes to `DESIGN-PATTERNS.md` | ❌ No dedicated section |
| **Reference depth** | `frameworks.md` has a "Key Design Patterns to Know" list (10 patterns, names only) | **114-line reference** with: pattern selection approach (4 steps), patterns organized by problem category (4 categories), per-pattern: problem it solves + Azure implementation, common composition examples, "when NOT to use" guidance, anti-patterning rules | ❌ None |
| **Pattern → Azure service mapping** | ❌ Not provided | ✅ Every pattern mapped to specific Azure services | ❌ Not provided |
| **Composition guidance** | Mentioned but not detailed | ✅ Common compositions per category (e.g., "Retry + Circuit Breaker + Bulkhead = resilience trinity") | ❌ None |
| **When NOT to use** | ❌ Not covered | ✅ 4 reasons to skip a pattern (simplicity, low scale, team readiness, operational cost) | ❌ None |

**Verdict:** ghcpcli's pattern reference is the standout — organized by problem category with Azure implementation details, composition examples, and crucially, "when NOT to use" guidance. claude has a pattern list but no depth. m365 has no pattern content at all.

---

## 12. Iterative Refinement (FR-5)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Coverage** | 4-bullet section: drill deeper, change constraints, what-ifs, revisit | 4-bullet section: re-evaluate, what-ifs, explain what changes, don't repeat | Step 7 one-liner: "offer drill-downs, what-ifs, or re-evaluation" |
| **"Don't repeat" rule** | ❌ | ✅ "Not repeating earlier answers — build on them" | ❌ |

**Verdict:** claude and ghcpcli are comparable and both adequate. ghcpcli's "don't repeat, build on" rule is a nice anti-pattern to avoid. m365 covers it in a single line — sufficient but minimal.

---

## 13. Citations / Traceability (FR-6)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Format** | Step 5: 3 bullet rules | Dedicated section: 4 numbered rules | Dedicated section: 3 bullet rules |
| **Anti-hallucination** | "Distinguish between guidance from docs and general reasoning" | "Use MCP tools to retrieve URLs — do not fabricate them" | "Distinguish Authoritative (Docs) vs Reasoned Guidance" |
| **In output contracts** | N/A (no contracts) | N/A (no contracts) | ✅ `citations` block in every contract |

**Verdict:** All three cover the basics. ghcpcli's "do not fabricate URLs" is the most direct anti-hallucination instruction. m365 reinforces citations structurally through its output contracts. claude is adequate.

---

## 14. Audience Awareness (NFR-3)

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Coverage** | Dedicated section with 3 examples (subscription topology → architecture-level; SDK retry → code-level; governance → operations-level) | Dedicated section with 3 examples (same examples as claude) + "match the hat the user is wearing *right now*" | ❌ Not addressed |

**Verdict:** claude and ghcpcli both handle this well with concrete examples. m365 completely omits audience awareness — a gap given that the requirements (NFR-3) explicitly call for it.

---

## 15. Decision Routing / Heuristics

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Mechanism** | Reference file pointers inline | ✅ Explicit 5-item decision tree routing to specific reference files | Inline "Decision heuristics" section |
| **Routing clarity** | Implicit — "see references/frameworks.md" | ✅ Numbered: service selection → DESIGN-GUIDANCE, WAF review → WELL-ARCHITECTED, adoption → CLOUD-ADOPTION, patterns → DESIGN-PATTERNS, cross-cutting → most relevant first | ❌ No routing — heuristics are just a flat list |

**Verdict:** ghcpcli's decision tree is the clearest routing mechanism — the agent knows exactly which file to load for each question type, saving tokens by not loading everything. claude relies on the agent to figure out which reference to consult. m365 loads everything every time (single file).

---

## 16. Output Format

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Prescribed format** | ❌ Free-form | ❌ Free-form | ✅ 3 YAML output contracts (architecture_decision, waf_review, caf_guidance) |
| **Self-check** | ❌ | ✅ "Acceptance criteria" checklist (6 items) | ✅ "Quality gates" checklist (5 items) |

**Verdict:** Trade-off. m365's contracts ensure consistent structure but may feel rigid and take up prompt space. ghcpcli and m365 both have self-check criteria (ghcpcli's is more detailed). claude has neither — it trusts the agent to format well.

---

## 17. Dialogue Patterns

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Coverage** | ❌ None | ❌ None | ✅ 3 patterns: narrow comparison, new solution sketch, WAF review — each with a brief protocol |

**Verdict:** m365 is unique here. The dialogue patterns give the agent a playbook for the three most common interaction types. claude and ghcpcli leave this to the agent's discretion.

---

## 18. Anti-Patterns / Pitfalls

| | claude | ghcpcli | m365 |
|---|---|---|---|
| **Coverage** | ❌ None | ✅ "Common pitfalls" section with 6 items (recommending without context, single-option answers, ignoring quality attributes, stale guidance, scope creep, overloading discovery) | ❌ None |

**Verdict:** ghcpcli is the only one with explicit anti-pattern guidance. This is valuable — telling the agent what NOT to do is often more effective than telling it what to do.

---

## Summary: Requirement Coverage Matrix

| Requirement | claude | ghcpcli | m365 |
|---|---|---|---|
| BR-1: Context before advice | ✅ Good | ✅ Best (broad/narrow split + rules) | ⚠️ Compressed |
| BR-2: Decision clarity | ✅ Good | ✅ Good | ⚠️ Compressed |
| BR-3: Quality attributes | ✅ Good | ✅ Best (table format) | ⚠️ Pillar names only |
| FR-1: Architecture design | ⚠️ Thin hub, OK references | ✅ Best (methodology + depth) | ⚠️ Heuristics only, no method |
| FR-2: WAF review | ⚠️ Thin | ✅ Best (assessment toolkit) | ⚠️ Schema only, no how-to |
| FR-3: CAF guidance | ⚠️ Routing aids only | ✅ Best (full phase coverage) | ❌ Missing |
| FR-4: Design patterns | ⚠️ Pattern list only | ✅ Best (categorized + Azure mapping + composition) | ❌ Missing |
| FR-5: Iterative refinement | ✅ Good | ✅ Good | ⚠️ One-liner |
| FR-6: Citations | ✅ Good | ✅ Good | ✅ Good (+ structural) |
| NFR-1: MCP grounding | ✅ Best (163-line query guide) | ✅ Good (inline + reference) | ⚠️ No query examples |
| NFR-3: Audience awareness | ✅ Good | ✅ Good | ❌ Missing |
| NFR-4: Currency | ✅ Good | ✅ Good | ✅ Good |
| NFR-5: Scope boundaries | ✅ Best (table with redirects) | ✅ Good | ⚠️ No redirects |

## Overall Assessment

**ghcpcli** — Most complete. Strongest reference depth (725 lines total). Best structured for token efficiency (load references on demand). Has unique strengths: anti-pattern guidance, decision tree routing, assessment toolkit for WAF reviews. Covers every requirement with actionable depth.

**claude** — Cleanest hub file. MCP query patterns reference is its standout strength. Covers all behavioral requirements well but has thinner functional coverage (FR-1 through FR-4). Good balance of brevity and depth through its 2-file reference system.

**m365** — Most compact (192 lines, no references). Unique strengths: output contracts for structural consistency, dialogue patterns as interaction playbooks. But has significant gaps: no CAF content (FR-3), no pattern content (FR-4), no audience awareness (NFR-3), no MCP query examples, no WAF assessment methodology. The decision heuristics tell the agent *what* exists but not *how* to use it.
