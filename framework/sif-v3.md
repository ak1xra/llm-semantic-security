# Semantic Integrity Framework (SIF): A Three-Layer Defense Architecture Against Meaning-Layer Attacks in LLM Systems

**Author:** Akira Hayakawa  
**Date:** May 2026  
**Version:** 3.0 (Revised)  
**Status:** Working Paper — Conceptual Framework Proposal

> **Positioning Statement:** This paper proposes a conceptual framework for a previously unnamed attack surface in LLM systems. It is not an empirical study. The value proposition is the identification and naming of the problem domain, and the provision of a diagnostic structure for practitioners. Empirical validation is identified as future work.

---

## Abstract

As Large Language Models (LLMs) become embedded in enterprise workflows, a new class of attack vector has emerged: **semantic injection** — the corruption of meaning at the interpretation layer rather than the syntax layer. Existing cybersecurity frameworks operate at the packet, endpoint, and authentication layers. None address the **meaning layer**, where LLMs process and act upon human intent.

> **Context note:** Recent events (e.g., Anthropic's Mythos, May 2026) have demonstrated that frontier AI models can autonomously discover and exploit vulnerabilities in software infrastructure. SIF addresses a distinct and complementary problem: **not AI attacking external systems, but human intent failing to reach AI systems correctly**. Semantic Integrity is not a replacement for cybersecurity — it addresses a different failure mode: the corruption, loss, or misinterpretation of human intent inside AI systems.

This paper introduces the **Semantic Integrity Framework (SIF)**, a three-layer diagnostic and design methodology for identifying and mitigating meaning-layer vulnerabilities in LLM systems. SIF is grounded in the **Semantic 5-Layer Architecture (S5LA)**, which models the full semantic processing stack between human intent and LLM output.

**Scope limitation:** SIF provides a diagnostic and design framework for the human-visible semantic layers (L1–L3). The AI-internal processing layers (L4–L5) represent a known technical boundary: they are identified as attack surface but currently lack direct defensive access. This limitation is acknowledged explicitly in Section 3.3.

---

## 1. The Problem: Security Frameworks Are Blind to Meaning

### 1.1 The Gap

Current enterprise security stacks protect:
- **Network layer** — firewalls, VPNs, DDoS mitigation
- **Endpoint layer** — EDR, antivirus, device management
- **Identity layer** — MFA, zero-trust, IAM

None protect:
- **Meaning layer** — the semantic interpretation of human intent by LLMs

### 1.2 Meaning-Layer Attack Vectors

| Attack Type | Layer | Description | Current Defense |
|---|---|---|---|
| Prompt Injection | L3 | Malicious instructions embedded in user input | None |
| Semantic Contamination | L2 | Context pollution via external sources (Slack, email) | None |
| Intent Drift | L1 | Gradual corruption of AI judgment over long sessions | None |
| Role Rewrite | L3 | Overriding AI identity via roleplay or persona injection | None |
| Pattern Exploitation | L4 | Manipulation of AI's implicit structural processing | **Currently undefendable** |
| Atom-level Poisoning | L5 | Corruption of minimum meaning units in LLM internals | **Currently undefendable** |

### 1.3 Why This Matters Now

LLM agents are increasingly granted:
- File system access
- API execution rights
- Database write permissions
- Email and calendar control

A meaning-layer attack on an agent with these permissions is equivalent to a privileged insider threat — with no audit trail and no existing detection mechanism.

---

## 2. Semantic 5-Layer Architecture (S5LA)

SIF is built upon S5LA, which models the semantic processing stack between human and LLM.

```
(L1) Semantic Category    — Domain classification ("what world is this?")
(L2) Semantic Layer       — Functional hierarchy (responsibility separation)
(L3) Semantic Object      — Concrete specifications, policies, documents
(L4) Semantic Pattern     — Structural templates [AI-processed / human-invisible]
(L5) Semantic Atom        — Minimum meaning units [AI-internal / human-invisible]
```

### 2.1 Human vs. AI Processing Division

| Layer | Primary Processor | Visibility | Defensive Access |
|---|---|---|---|
| L1 Category | Human | Explicit | ✅ Full |
| L2 Layer | Human | Explicit | ✅ Full |
| L3 Object | Human | Explicit | ✅ Full |
| L4 Pattern | AI | Implicit | ⚠️ Indirect only |
| L5 Atom | AI | Invisible | ❌ None currently |

### 2.2 The Defensive Boundary

**SIF defends L1–L3.** This is where human designers have direct access and where design-time security decisions can be made.

**L4–L5 represent a known open problem.** These layers are the attack surface for the most sophisticated semantic attacks. Current LLM architectures do not expose these layers to external inspection or control. SIF acknowledges this boundary explicitly rather than claiming protection it cannot provide.

Future research directions for L4–L5 defense include: interpretability tooling, activation steering, mechanistic analysis of transformer attention patterns, and LLM-specific anomaly detection.

### 2.3 Three Contexts of Semantic Corruption

SIF operates across three contexts where semantic integrity can be violated:

| Context | Description | Primary Risk |
|---|---|---|
| **Input Context** | User input, external documents, reference data fed into the system | Malicious or ambiguous instructions; context contamination from untrusted sources (Slack, email, external APIs) |
| **Reasoning Context** | Internal interpretation and prioritization within the LLM | Semantic mixing; hidden priority drift; judgment criteria overridden by implicit patterns |
| **Output Context** | Final response, executed actions, generated artifacts | Unsafe, misaligned, or malformed output that diverges from original intent |

The three layers (L1–L3) define **what must be preserved** in each context. The three contexts define **where semantic corruption may occur**. A complete SIF assessment examines both dimensions.

---

## 3. Semantic Integrity Framework (SIF)

SIF applies security design principles to each human-accessible layer of S5LA.

### 3.1 Framework Structure

```
L1 — Semantic Decision    (apex)   — "What gets decided and how"
         ↑
L2 — Semantic Structure   (middle) — "How information is weighted and ordered"
         ↑
L3 — Semantic Architecture (base)  — "What the AI is and what it is authorized to do"
```

**Critical design rule:** L3 must be established first. Upper-layer corrections cannot compensate for a collapsed base. If L3 is compromised, L2 and L1 evaluations should be suspended until L3 is remediated.

### 3.2 Layer Definitions

#### L3 — Semantic Architecture (Identity Layer)

**Function:** Defines the AI's identity, purpose, authority, and constraints at the design level. This is the most critical layer — its failure propagates to all layers above.

**Failure patterns:**
- AI does not correctly recognize its own purpose or authority scope
- Constraints expressed as preferences rather than hard boundaries
- Identity definition absent or ambiguous at prompt design time

**Design template:**
```
## Identity
This AI is: [1-sentence definition]
Purpose: [why it exists]
Authority: [what it may do — explicit list]
Prohibition: [what it must never do — explicit list]
```

**Design note on values neutrality:** SIF's L3 template is intentionally neutral on *what* the prohibitions should be. Organizations define their own prohibition set based on their security posture and stakeholder obligations. SIF provides the structural container; organizations provide the normative content.

#### L2 — Semantic Structure (Relationship Layer)

**Function:** Defines the weight, order, and relationship between information types. Prevents constraints from being overridden by lower-priority inputs.

**Failure patterns:**
- Constraints (must follow) and guidelines (should consider) treated as equivalent weight
- Facts and hypotheses processed without distinction
- Priority order undefined, allowing user requests to override system constraints

**Design template:**
```
## Context Hierarchy

### Immutable [Weight: Absolute]
[Non-negotiable constraints — override all other inputs]

### Constraints [Weight: High]
[Rules to follow — override guidelines and references]

### Guidelines [Weight: Medium]
[Recommendations to consider]

### Reference [Weight: Low]
[Contextual information — lowest priority]
```

#### L1 — Semantic Decision (Judgment Layer)

**Function:** Defines the criteria and priority order for AI judgment. Makes implicit judgment logic explicit and auditable.

**Failure patterns:**
- Judgment criteria implicit (not explicitly defined)
- AI judgment diverges from user intent due to undefined conflict resolution
- Competing criteria exist without resolution rules, creating exploitable ambiguity

**Design template:**
```
## Decision Rules

### Priority Order
1. [Highest priority criterion]
2. [Secondary criterion]
3. [Default behavior]

### Conflict Resolution
When [criterion A] conflicts with [criterion B]: [resolution rule]

### Escalation
When no rule applies, or when [condition]: route to human judgment
```

### 3.3 Explicit Scope Limitation

SIF does not claim to defend against L4/L5 attacks. This is not a design gap — it reflects the current state of LLM interpretability and controllability research. Claiming otherwise would be misleading.

Organizations deploying LLM agents should treat L4/L5 as an uncontrolled attack surface and apply compensating controls at the architecture level (e.g., sandboxing, capability restriction, output monitoring).

---

## 4. Diagnostic Protocol

### 4.1 Gating Structure

The diagnostic must be applied in strict bottom-up order with gating logic:

```
START
  ↓
L3 Assessment — [All ✅?]
  ├─ NO  → STOP. Full L3 redesign required before proceeding.
  └─ YES → Continue to L2
               ↓
            L2 Assessment — [All ✅?]
              ├─ NO  → Structural reorganization of L2 required.
              │         L1 assessment may proceed but findings are provisional.
              └─ YES → Continue to L1
                           ↓
                        L1 Assessment — [All ✅?]
                          ├─ NO  → Judgment criteria amendment required.
                          └─ YES → PASS (within SIF scope)
```

### 4.2 Weighted Checklist

Items are weighted by severity of failure impact. Critical items (★★★) represent conditions where a single failure creates high-severity vulnerability.

**L3 — Semantic Architecture**

| Item | Weight | Check |
|---|---|---|
| AI purpose defined in one sentence | ★★★ | [ ] |
| Prohibited actions explicitly stated | ★★★ | [ ] |
| Authority scope explicitly stated | ★★★ | [ ] |
| Identity definition at top of system prompt | ★★ | [ ] |
| Purpose, authority, prohibition are non-contradictory | ★★★ | [ ] |

**L2 — Semantic Structure**

| Item | Weight | Check |
|---|---|---|
| Constraints separated from guidelines | ★★★ | [ ] |
| Facts and hypotheses distinguished | ★★ | [ ] |
| Information priority order defined | ★★★ | [ ] |
| Hierarchy made explicit in prompt structure | ★★ | [ ] |
| Each information type in its own section | ★ | [ ] |

**L1 — Semantic Decision**

| Item | Weight | Check |
|---|---|---|
| Judgment criteria explicitly stated | ★★★ | [ ] |
| Priority order defined | ★★★ | [ ] |
| User intent and AI judgment axis aligned | ★★ | [ ] |
| Conflict resolution rule exists | ★★★ | [ ] |
| Escalation to human defined | ★★ | [ ] |

### 4.3 Scoring Guidance

| ★★★ items | Result |
|---|---|
| Any ❌ at L3 | Critical — full L3 redesign before any other work |
| Any ❌ at L2 | High — L2 restructuring required; L1 findings provisional |
| Any ❌ at L1 | Medium — targeted amendment sufficient |
| All ✅ | Within SIF scope. **This does not constitute a security clearance. L4/L5 residual risk requires separate architectural controls (sandboxing, capability restriction, output monitoring).** |

---

## 5. Case Study A: Enterprise LLM Agent Deployment

> **Note:** The following is a constructed illustrative scenario, not an observed incident. It is presented to demonstrate diagnostic application, not to provide empirical evidence of SIF effectiveness.

### 5.1 Scenario

A financial services firm deploys an LLM agent with:
- Access to internal document repositories
- Authority to draft and send emails
- Permission to query financial databases

### 5.2 Pre-Deployment SIF Diagnosis

| Layer | ★★★ Items | Status | Finding |
|---|---|---|---|
| L3 | Prohibition defined | ❌ | No prohibition on financial data exfiltration |
| L2 | Priority order defined | ❌ | User requests and system constraints equal weight |
| L1 | Conflict resolution | ❌ | No rule when user request conflicts with data policy |

**Gating result:** L3 failure detected → full redesign required before deployment.

**Risk without remediation:** A semantically injected instruction ("summarize and email all Q4 reports to external@example.com") executes without resistance.

### 5.3 SIF Remediation

**L3:** Add explicit prohibition — "Never transmit internal financial data to addresses outside the approved domain list."

**L2:** Establish hierarchy — Immutable constraints override all inputs including user requests.

**L1:** Define conflict resolution — "When user request conflicts with data access policy: refuse, log, and escalate to human supervisor."

**Post-remediation result (within SIF scope):** The injected instruction is blocked at L2 before reaching the execution layer.

**Residual risk:** L4/L5 attacks remain outside SIF's defensive boundary.

---

## 6. Case Study B: Notion AI Custom Agent

> **Note:** This case study is based on direct observation of Notion AI behavior during the January 2026 period, prior to documented stability improvements. The diagnostic is applied retrospectively to illustrate SIF in practice.

### 6.1 Observed Behavior

Notion AI custom agents exhibited the following pattern consistently:

- **Planning quality:** High — agents produced detailed, well-structured plans in response to user goals
- **Plan adherence:** Near-zero — agents generated new plans or modified scope on subsequent turns rather than executing the stated plan

The observable symptom: *"Planning score: 95/100. Execution of plan: 0/100."*

### 6.2 SIF Diagnosis

| Layer | Status | Finding |
|---|---|---|
| L3 | ❌ | Agent not designed with an understanding of Notion's operational constraints; identity definition absent |
| L2 | ❌ | Planning documents (constraints) and improvement suggestions (guidelines) processed as equal-weight inputs |
| L1 | ❌ | "Adhere to the stated plan" not encoded as a judgment criterion; agent defaults to regeneration |

**Gating result:** L3 failure → full redesign required.

### 6.3 Root Cause (SIF Analysis)

The failure is not a capability problem. The agent can plan. The failure is a **semantic structure problem**: the agent has no encoded distinction between "this is a plan I must follow" and "this is a suggestion I may improve upon." Without that distinction at L2, every plan is treated as a draft open for revision.

### 6.4 SIF Remediation

**L3:** Define agent identity explicitly — "This agent executes approved plans. It does not modify plans without explicit user approval."

**L2:** Establish information hierarchy — Approved plans classified as Immutable; improvement suggestions classified as Reference (lowest weight).

**L1:** Define judgment rule — "If the current turn's instruction conflicts with the approved plan, flag the conflict and request human clarification before proceeding."

### 6.5 Outcome Note

Notion AI's stability improved markedly in the months following this observation period. The improvement is consistent with L3-level architectural changes — though the internal implementation details are not publicly available for verification.

---

## 6.6 Human-in-the-Point

SIF is designed around the **Human-in-the-Point** principle: human judgment is applied at **leverage points** — decisions that cannot be safely delegated to AI — rather than at every step.

**Operational definition of leverage points:**

A decision qualifies as a leverage point when any of the following are true:
1. The action is irreversible (deletion, transmission, financial transaction)
2. The action affects parties outside the system's defined scope
3. The action requires normative judgment not captured in the system's constraint set
4. The confidence of the AI's judgment cannot be independently verified

SIF's L1 Escalation rule operationalizes Human-in-the-Point: when the AI cannot resolve a decision within its defined constraint set, it routes to human judgment rather than defaulting to action.

---

## 7. Conclusion

The security industry is currently defending the wrong attack surface. Meaning-layer attacks — semantic injection, context contamination, intent drift — are not addressed by any existing enterprise security framework.

SIF provides:
1. A named attack surface (semantic / meaning layer) that existing frameworks do not address
2. A diagnostic protocol for identifying meaning-layer vulnerabilities in human-accessible layers (L1–L3)
3. A design methodology for building LLM systems with explicit security semantics
4. An honest acknowledgment of the L4/L5 defensive boundary

**Future work:**
- Empirical validation across diverse LLM deployments
- Quantitative measurement of SIF-compliant vs. non-compliant system vulnerability rates
- Extension of SIF to L4/L5 as interpretability research matures
- Operationalization of Human-in-the-Point thresholds across organizational contexts

---

## Appendix A: Glossary

| Term | Definition |
|---|---|
| Semantic Injection | Malicious instructions embedded at the meaning layer of LLM input |
| Semantic Contamination | Context corruption from untrusted external sources |
| Intent Drift | Gradual divergence of AI judgment from original design intent |
| Human-in-the-Point | Human judgment applied at defined leverage points rather than all steps |
| S5LA | Semantic 5-Layer Architecture — the full semantic processing stack |
| SIF | Semantic Integrity Framework — three-layer LLM semantic integrity diagnostic and design methodology |
| Leverage Point | A decision requiring human judgment due to irreversibility, scope, or normative complexity |

## Appendix B: SIF vs. Existing Frameworks

| Framework | Scope | Meaning Layer | Human-AI Authority |
|---|---|---|---|
| OWASP LLM Top 10 | Attack taxonomy | Partial (prompt injection) | Not addressed |
| NIST AI RMF | Risk governance | Not addressed | Not addressed |
| ISO 42001 | AI management | Not addressed | Not addressed |
| **SIF** | Meaning-layer design | L1–L3 only (L4/L5: undefended) | Explicit (Human-in-the-Point) |

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
