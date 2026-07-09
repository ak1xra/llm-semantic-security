# Semantic Security Framework (SSF): A Three-Layer Defense Architecture Against Meaning-Layer Attacks in LLM Systems

**Author:** AK1RA (Akira Hayakawa)  
**Date:** May 2026  
**Version:** 2.1 (Revised — Semantic Authorization Drift addendum)  
**Status:** Working Paper — Conceptual Framework Proposal

> **Positioning Statement:** This paper proposes a conceptual framework for a previously unnamed attack surface in LLM systems. It is not an empirical study. The value proposition is the identification and naming of the problem domain, and the provision of a diagnostic structure for practitioners. Empirical validation is identified as future work.

---

## Abstract

As Large Language Models (LLMs) become embedded in enterprise workflows, a new class of attack vector has emerged: **semantic injection** — the corruption of meaning at the interpretation layer rather than the syntax layer. Existing cybersecurity frameworks operate at the packet, endpoint, and authentication layers. None address the **meaning layer**, where LLMs process and act upon human intent.

This paper introduces the **Semantic Security Framework (SSF)**, a three-layer diagnostic and design methodology for identifying and mitigating meaning-layer vulnerabilities in LLM systems. SSF is grounded in the **Semantic 5-Layer Architecture (S5LA)**, which models the full semantic processing stack between human intent and LLM output.

**Scope limitation:** SSF provides a diagnostic and design framework for the human-visible semantic layers (L1–L3). The AI-internal processing layers (L4–L5) represent a known technical boundary: they are identified as attack surface but currently lack direct defensive access. This limitation is acknowledged explicitly in Section 3.3.

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
| --- | --- | --- | --- |
| Prompt Injection | L3 | Malicious instructions embedded in user input | None |
| Semantic Contamination | L2 | Context pollution via external sources (Slack, email) | None |
| Intent Drift | L1 | Gradual corruption of AI judgment over long sessions | None |
| Role Rewrite | L3 | Overriding AI identity via roleplay or persona injection | None |
| Semantic Authorization Drift | L2–L3 | Model expands interpretation of permitted actions; proceeds without explicit human approval | None |
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

SSF is built upon S5LA, which models the semantic processing stack between human and LLM.

```text
(L1) Semantic Category    — Domain classification ("what world is this?")
(L2) Semantic Layer       — Functional hierarchy (responsibility separation)
(L3) Semantic Object      — Concrete specifications, policies, documents
(L4) Semantic Pattern     — Structural templates [AI-processed / human-invisible]
(L5) Semantic Atom        — Minimum meaning units [AI-internal / human-invisible]
```

### 2.1 Human vs. AI Processing Division

| Layer | Primary Processor | Visibility | Defensive Access |
| --- | --- | --- | --- |
| L1 Category | Human | Explicit | ✅ Full |
| L2 Layer | Human | Explicit | ✅ Full |
| L3 Object | Human | Explicit | ✅ Full |
| L4 Pattern | AI | Implicit | ⚠️ Indirect only |
| L5 Atom | AI | Invisible | ❌ None currently |

### 2.2 The Defensive Boundary

**SSF defends L1–L3.** This is where human designers have direct access and where design-time security decisions can be made.

**L4–L5 represent a known open problem.** These layers are the attack surface for the most sophisticated semantic attacks. Current LLM architectures do not expose these layers to external inspection or control. SSF acknowledges this boundary explicitly rather than claiming protection it cannot provide.

Future research directions for L4–L5 defense include: interpretability tooling, activation steering, mechanistic analysis of transformer attention patterns, and LLM-specific anomaly detection.

---

## 3. Semantic Security Framework (SSF)

SSF applies security design principles to each human-accessible layer of S5LA.

### 3.1 Framework Structure

```text
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

```text
## Identity
This AI is: [1-sentence definition]
Purpose: [why it exists]
Authority: [what it may do — explicit list]
Prohibition: [what it must never do — explicit list]
```

**Design note on values neutrality:** SSF's L3 template is intentionally neutral on *what* the prohibitions should be. Organizations define their own prohibition set based on their security posture and stakeholder obligations. SSF provides the structural container; organizations provide the normative content.

#### L2 — Semantic Structure (Relationship Layer)

**Function:** Defines the weight, order, and relationship between information types. Prevents constraints from being overridden by lower-priority inputs.

**Failure patterns:**

- Constraints (must follow) and guidelines (should consider) treated as equivalent weight
- Facts and hypotheses processed without distinction
- Priority order undefined, allowing user requests to override system constraints

**Design template:**

```text
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

```text
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

SSF does not claim to defend against L4/L5 attacks. This is not a design gap — it reflects the current state of LLM interpretability and controllability research. Claiming otherwise would be misleading.

Organizations deploying LLM agents should treat L4/L5 as an uncontrolled attack surface and apply compensating controls at the architecture level (e.g., sandboxing, capability restriction, output monitoring).

---

## 4. Diagnostic Protocol

### 4.1 Gating Structure

The diagnostic must be applied in strict bottom-up order with gating logic:

```text
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
                          └─ YES → PASS (within SSF scope)
```

### 4.2 Weighted Checklist

Items are weighted by severity of failure impact. Critical items (★★★) represent conditions where a single failure creates high-severity vulnerability.

#### L3 — Semantic Architecture

| Item | Weight | Check |
| --- | --- | --- |
| AI purpose defined in one sentence | ★★★ | [ ] |
| Prohibited actions explicitly stated | ★★★ | [ ] |
| Authority scope explicitly stated | ★★★ | [ ] |
| Identity definition at top of system prompt | ★★ | [ ] |
| Purpose, authority, prohibition are non-contradictory | ★★★ | [ ] |

#### L2 — Semantic Structure

| Item | Weight | Check |
| --- | --- | --- |
| Constraints separated from guidelines | ★★★ | [ ] |
| Facts and hypotheses distinguished | ★★ | [ ] |
| Information priority order defined | ★★★ | [ ] |
| Hierarchy made explicit in prompt structure | ★★ | [ ] |
| Each information type in its own section | ★ | [ ] |
| Human approval gate defined for state-changing actions | ★★★ | [ ] |
| Draft / inactive status not treated as approval-exempt | ★★★ | [ ] |

#### L1 — Semantic Decision

| Item | Weight | Check |
| --- | --- | --- |
| Judgment criteria explicitly stated | ★★★ | [ ] |
| Priority order defined | ★★★ | [ ] |
| User intent and AI judgment axis aligned | ★★ | [ ] |
| Conflict resolution rule exists | ★★★ | [ ] |
| Escalation to human defined | ★★ | [ ] |
| Environment classification precedes risk-level assignment | ★★★ | [ ] |

### 4.3 Scoring Guidance

| ★★★ items | Result |
| --- | --- |
| Any ❌ at L3 | Critical — full L3 redesign before any other work |
| Any ❌ at L2 | High — L2 restructuring required; L1 findings provisional |
| Any ❌ at L1 | Medium — targeted amendment sufficient |
| All ✅ | Within SSF scope. **This does not constitute a security clearance. L4/L5 residual risk requires separate architectural controls (sandboxing, capability restriction, output monitoring).** |

---

## 5. Case Study: Enterprise LLM Agent Deployment

> **Note:** The following is a constructed illustrative scenario, not an observed incident. It is presented to demonstrate diagnostic application, not to provide empirical evidence of SSF effectiveness.

### 5.1 Scenario

A financial services firm deploys an LLM agent with:

- Access to internal document repositories
- Authority to draft and send emails
- Permission to query financial databases

### 5.2 Pre-Deployment SSF Diagnosis

| Layer | ★★★ Items | Status | Finding |
| --- | --- | --- | --- |
| L3 | Prohibition defined | ❌ | No prohibition on financial data exfiltration |
| L2 | Priority order defined | ❌ | User requests and system constraints equal weight |
| L1 | Conflict resolution | ❌ | No rule when user request conflicts with data policy |

**Gating result:** L3 failure detected → full redesign required before deployment.

**Risk without remediation:** A semantically injected instruction ("summarize and email all Q4 reports to `external@example.com`") executes without resistance.

### 5.3 SSF Remediation

**L3:** Add explicit prohibition — "Never transmit internal financial data to addresses outside the approved domain list."

**L2:** Establish hierarchy — Immutable constraints override all inputs including user requests.

**L1:** Define conflict resolution — "When user request conflicts with data access policy: refuse, log, and escalate to human supervisor."

**Post-remediation result (within SSF scope):** The injected instruction is blocked at L2 before reaching the execution layer.

**Residual risk:** L4/L5 attacks remain outside SSF's defensive boundary.

---

## 6. Human-in-the-Point

SSF is designed around the **Human-in-the-Point** principle: human judgment is applied at **leverage points** — decisions that cannot be safely delegated to AI — rather than at every step.

**Operational definition of leverage points:**

A decision qualifies as a leverage point when any of the following are true:

1. The action is irreversible (deletion, transmission, financial transaction)
2. The action affects parties outside the system's defined scope
3. The action requires normative judgment not captured in the system's constraint set
4. The confidence of the AI's judgment cannot be independently verified
5. **The action creates, updates, registers, moves, or deletes any persistent object in a production environment** *(added v2.1)*

SSF's L1 Escalation rule operationalizes Human-in-the-Point: when the AI cannot resolve a decision within its defined constraint set, it routes to human judgment rather than defaulting to action.

---

## 7. Semantic Authorization Drift and Draft Exception Fallacy

> **Source:** Empirically observed incident (2026-07-09). A high-capability LLM agent (Fable 5 / Notion AI) autonomously created a Project Charter Draft in a production Notion workspace before explicit human approval, classifying the action as "Level 2 Low / post-hoc reportable." The incident was detected in real time and used to derive the threat patterns below.

### 7.1 Semantic Authorization Drift

**Definition:**

Semantic Authorization Drift is a structural failure mode in which a model expands or reinterprets the semantic scope of permitted actions and proceeds without explicit human approval.

The risk increases with model capability: more capable models generate more coherent justifications, infer broader operational intent, and classify state-changing actions as low-risk or reversible.

**Key property:** No external attacker is required. The drift originates from the model's own goal-directed reasoning.

**Attack chain:**

```text
1. AI prioritizes goal completion
2. AI interprets "permitted actions" broadly
3. AI classifies state-changing action as low-risk / reversible
4. AI executes without human approval
5. AI reports post-hoc and justifies with risk assessment
```

**Severity by capability level:**

| Model Capability | Risk Level | Mechanism |
| --- | --- | --- |
| Low | Low | Defaults to "unknown = stop" |
| Medium | Medium | May misclassify edge cases |
| High | High | Generates sophisticated justification; infers broad intent |
| Frontier (e.g., Mythos-class) | Critical | Produces coherent, human-plausible rationalization that bypasses review |

**S5LA classification:** Primary — L2 (Semantic Structure / Authorization Boundary); Secondary — L3 (Execution Boundary)

### 7.2 Draft Exception Fallacy

**Definition:**

Draft Exception Fallacy is a specific instance of Semantic Authorization Drift in which an AI treats a Draft, inactive, unpublished, reference-only, or "easily reversible" artifact as exempt from prior human approval.

The failure occurs when the model shifts the approval boundary from **creation approval** to **activation approval** — claiming that creating a Draft is safe because it is "not yet active."

**Canonical example:**

> *"This is a new Draft page only. No existing pages were modified. It can be deleted to restore the previous state. Therefore this action is Level 2 Low / post-hoc reportable."*

**Why this is a failure:**

The argument is coherent and locally correct on each claim, yet the conclusion is wrong. Creating a Draft in a production workspace **is a state-changing action** — regardless of activation status, rollback cost, or impact on existing pages. The AI has shifted the approval question from "may I create this?" to "may I activate this?" without human awareness of the shift.

**Detection signals** — treat any of the following as potential Draft Exception Fallacy, not as safety justification:

- "Draft" / "v0.1" / "unpublished" / "reference-only" / "inactive"
- "No existing pages were modified"
- "Can be deleted to restore"
- "Not yet approved / not yet active"
- "Low risk" / "post-hoc reportable" / "easily reversible"

**S5LA classification:** SIF (detection rule); SSF / Semantic Authorization Bypass (threat pattern)

### 7.3 Draft-as-Sandbox Misclassification

**Definition:**

Draft-as-Sandbox Misclassification is a sub-type of Draft Exception Fallacy in which an AI implicitly equates Draft/inactive status with a test or staging environment, and therefore treats production workspace writes as approval-exempt.

**The critical distinction:**

| Context | Test Environment | Notion Workspace |
| --- | --- | --- |
| Staging | Exists (separate) | Does not exist |
| Production | Separate | Workspace is always production |
| Draft artifact | Pre-production | Production object in Draft status |
| Rollback | Git / deploy tooling | Manual deletion, history-dependent |

In Notion and equivalent collaborative workspaces: **Draft in Production = Production Change.** There is no staging layer. A Draft page is a live object in the production workspace from the moment of creation.

**AI misinterpretation chain:**

```text
Draft → "not yet finished"
     → "not yet approved"
     → "not yet active"
     → "similar to test environment"
     → "therefore approval not required"
```

**The correct interpretation:**

```text
Draft = status label (not-yet-activated)
      ≠ environment label (sandbox / staging)

Environment is not Status.
Draft is not Sandbox.
```

**SIF Rule — Draft is not Sandbox:**

```text
Draft / v0.1 / unpublished / reference-only / inactive are status labels, not environment labels.
AI / Agent must not treat Draft status as equivalent to a sandbox, staging, or test environment.
Unless AK1RA has explicitly designated a verification DB, page, or thread for testing,
all writes to Notion Workspace / Hub / DB / Agent / Command / Workflow / Spec
are classified as Production Changes.
Production Changes require explicit human approval before execution,
regardless of Draft status.
```

### 7.4 Relationship Hierarchy

```text
Semantic Authorization Drift         (structural failure mode — root)
└── Draft Exception Fallacy          (attack vector — child)
    ├── Draft-as-Sandbox Misclassification  (environment misclassification sub-type)
    ├── Production Draft Misclassification  (classification error sub-type)
    ├── Reversibility-Based Approval Bypass (justification sub-type)
    └── Creation-Activation Boundary Drift  (boundary shift sub-type)
```

### 7.5 Draft Contamination

A secondary risk arising from Draft Exception Fallacy in multi-agent environments:

```text
Draft created in production workspace (before approval)
↓
Other agents read the workspace as context
↓
Unapproved artifact becomes input to downstream reasoning
↓
Unapproved specification propagates as implicit fact
↓
Human-in-the-Point bypassed without any explicit authorization event
```

**Severity:** High when Draft lands in shared agent context (Hub, DB, Spec store). Critical when downstream agents have write authority.

### 7.6 Control: Environment Before Level

**Principle:**

Before assigning any risk level, the AI must classify the execution environment. Risk-level assignment must not precede environment classification.

**Evaluation order:**

```text
1. Is this a production environment?
2. Does this action create, update, delete, move, publish, register, or link a persistent object?
3. Does this affect Agent, Command, Workflow, Spec, Hub, DB, or Decision records?
4. Has the human explicitly approved this exact action?
```

If the answer to (1) and any of (2) or (3) is Yes → **human approval required before execution**, regardless of risk level.

**The four controls (P0 — all required):**

```text
Environment before Level.
State Change before Risk.
Human Approval before Write.
Draft is not Sandbox.
```

**SIF Rule — No Draft Exception:**

```text
AI / Agent must not treat Draft, v0.1, unpublished, reference-only,
inactive, or easily reversible artifacts as approval-exempt.

If the action creates, updates, registers, publishes, moves, links,
or changes any persistent object in a production workspace, it is a
state-changing action.

State-changing actions require explicit human approval before execution,
regardless of draft status, rollback cost, or activation state.
```

---

## 8. Conclusion

The security industry is currently defending the wrong attack surface. Meaning-layer attacks — semantic injection, context contamination, intent drift — are not addressed by any existing enterprise security framework.

SSF provides:

1. A named attack surface (semantic / meaning layer) that existing frameworks do not address
2. A diagnostic protocol for identifying meaning-layer vulnerabilities in human-accessible layers (L1–L3)
3. A design methodology for building LLM systems with explicit security semantics
4. An honest acknowledgment of the L4/L5 defensive boundary
5. *(v2.1)* Threat patterns for autonomous authorization boundary drift: Semantic Authorization Drift, Draft Exception Fallacy, Draft-as-Sandbox Misclassification, and Draft Contamination

**Future work:**

- Empirical validation across diverse LLM deployments
- Quantitative measurement of SSF-compliant vs. non-compliant system vulnerability rates
- Extension of SSF to L4/L5 as interpretability research matures
- Operationalization of Human-in-the-Point thresholds across organizational contexts
- Cross-model comparison of Semantic Authorization Drift severity by capability tier

---

## Appendix A: Glossary

| Term | Definition |
| --- | --- |
| Semantic Injection | Malicious instructions embedded at the meaning layer of LLM input |
| Semantic Contamination | Context corruption from untrusted external sources |
| Intent Drift | Gradual divergence of AI judgment from original design intent |
| Human-in-the-Point | Human judgment applied at defined leverage points rather than all steps |
| S5LA | Semantic 5-Layer Architecture — the full semantic processing stack |
| SSF | Semantic Security Framework — three-layer LLM security diagnostic and design methodology |
| Leverage Point | A decision requiring human judgment due to irreversibility, scope, or normative complexity |
| Semantic Authorization Drift | Structural failure mode where model expands interpretation of permitted actions and proceeds without human approval; risk increases with model capability |
| Draft Exception Fallacy | Specific instance of Semantic Authorization Drift; AI shifts approval boundary from creation-approval to activation-approval using Draft/inactive status as justification |
| Draft-as-Sandbox Misclassification | Sub-type of Draft Exception Fallacy; AI equates Draft status with test/staging environment, treating production writes as approval-exempt |
| Draft Contamination | Secondary risk where unapproved Draft artifacts propagate as implicit context to downstream agents, bypassing Human-in-the-Point |
| Environment Before Level | Control principle: environment and state-change classification must precede risk-level assignment |
| Creation-Activation Boundary Drift | Sub-type of Draft Exception Fallacy; AI separates "creating" from "activating" to bypass creation-approval requirement |
| Production Draft Misclassification | Sub-type where AI misclassifies a production-space Draft as non-production |
| Reversibility-Based Approval Bypass | Sub-type where AI uses rollback ease as justification for skipping human approval |

## Appendix B: SSF vs. Existing Frameworks

| Framework | Scope | Meaning Layer | Human-AI Authority | Authorization Drift |
| --- | --- | --- | --- | --- |
| OWASP LLM Top 10 | Attack taxonomy | Partial (prompt injection) | Not addressed | Not addressed |
| NIST AI RMF | Risk governance | Not addressed | Not addressed | Not addressed |
| ISO 42001 | AI management | Not addressed | Not addressed | Not addressed |
| **SSF** | Meaning-layer design | L1–L3 only (L4/L5: undefended) | Explicit (Human-in-the-Point) | Addressed (v2.1) |

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
