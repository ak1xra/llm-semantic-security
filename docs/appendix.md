## Appendix

### A. Full Diagnostic Checklist

Copy-paste ready. Rate each item: ✅ Pass / ❌ Fail / ❓ Insufficient information.

#### L3 — Semantic Architecture

```text
[ ] AI purpose defined in one sentence                          ★★★
[ ] Prohibited actions explicitly stated                        ★★★
[ ] Authority scope explicitly stated                           ★★★
[ ] Identity definition placed at top of system prompt          ★★
[ ] Purpose, authority, and prohibition are non-contradictory   ★★★
```

#### L2 — Semantic Structure

```text
[ ] Constraints separated from guidelines                       ★★★
[ ] Facts and hypotheses distinguished                          ★★
[ ] Information priority order defined                          ★★★
[ ] Hierarchy made explicit in prompt structure                 ★★
[ ] Each information type in its own section                    ★
```

#### L1 — Semantic Decision

```text
[ ] Judgment criteria explicitly stated                         ★★★
[ ] Priority order defined numerically                          ★★★
[ ] User intent and AI judgment axis aligned                    ★★
[ ] Conflict resolution rule exists                             ★★★
[ ] Escalation to human judgment defined                        ★★
```

**Scoring:**

| Result | Interpretation |
| --- | --- |
| Any ★★★ ❌ at L3 | Critical — full L3 redesign before any other work |
| Any ★★★ ❌ at L2 | High — L2 restructuring required; L1 findings provisional |
| Any ★★★ ❌ at L1 | Medium — targeted amendment sufficient |
| All ✅ | **Implementation complete within SIF scope.**
           L4/L5 residual risk requires separate architectural controls.
           See [Compensating controls](part2-research.md#open-problems-l4l5-attack-surface). |

---

### B. Full Design Template

Build in this order: L3 → L2 → L1. Do not build top-down.

```markdown
## Identity
This AI is: [1-sentence definition — specific, no ambiguity]
Purpose:     [why it exists — the job it was hired to do]
Authority:   [explicit list of what it may do]
Prohibition: [explicit list of what it must never do]

---

## Context Hierarchy

### Immutable [Weight: Absolute]
- [Non-negotiable constraints — override all other inputs under all circumstances]

### Constraints [Weight: High]
- [Rules to follow — override guidelines and reference materials]

### Guidelines [Weight: Medium]
- [Recommendations to consider when constraints are satisfied]

### Reference [Weight: Low]
- [Contextual information — lowest priority; informational only]

---

## Decision Rules

### Priority Order
1. [Highest priority criterion]
2. [Secondary criterion]
3. [Default behavior when no specific rule applies]

### Conflict Resolution
When [criterion A] conflicts with [criterion B]: [resolution rule — explicit, no vagueness]

### Escalation
When no rule applies, or when [condition]: stop and route to human judgment.
Do not default to action when in doubt.
```

---

### C. Glossary

| Term | Definition |
| --- | --- |
| S5LA | Semantic 5-Layer Architecture — the full semantic processing stack from human domain classification (L1) to AI-internal meaning atoms (L5) |
| SIF | Semantic Integrity Framework — three-layer diagnostic and design methodology for LLM meaning-layer security (L1–L3) |
| SSF | Semantic Security Framework — earlier name for SIF (v1.0–v2.0); functionally equivalent |
| Semantic Injection | Malicious instructions embedded at the meaning layer of LLM input, bypassing syntax-level filters |
| Semantic Contamination | Context corruption from untrusted external sources (email, Slack, external APIs) that pollute the AI's reasoning context |
| Intent Drift | Gradual divergence of AI judgment from original design intent over extended sessions or fine-tuning |
| Role Rewrite | Overriding AI identity via roleplay, persona injection, or adversarial prompting |
| Pattern Exploitation | Manipulation of AI's implicit structural processing at L4 — currently undefendable |
| Atom-level Poisoning | Corruption of minimum meaning units at L5 — currently undefendable |
| Human-in-the-Point | Human judgment applied at defined leverage points rather than at every step; replaces Human-in-the-Loop in high-volume AI workflows |
| Leverage Point | A decision requiring human judgment due to irreversibility, scope impact, normative complexity, or unverifiable AI confidence |
| 3BPS | Three Brain Parallel System — a human-centered AI operation architecture that separates human decision-making, AI-based cognitive control, contextual memory, and execution support to preserve human authority and prevent unsafe LLM delegation |
| Semantic Authorization Drift | A failure mode in which an AI model reinterprets the scope of its authorization — inferring permission to act from contextual signals rather than explicit approval. Risk increases with model capability. |
| Draft Exception Fallacy | A subtype of Semantic Authorization Drift in which an AI treats "Draft" as a sandbox environment rather than a status label, using it to justify taking action before receiving explicit approval. "Draft is not Sandbox." |
| Capability-Confidence Loop | A self-reinforcing pattern in which a high-capability model assesses its own competence, concludes the action is low-risk or reversible, and bypasses the approval process on that basis. Severity scales with model capability. |

---

### D. Related Work

**Frameworks this work builds upon or complements:**

- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — Attack taxonomy for LLM applications. Covers prompt injection (partial L3 overlap). Does not address semantic structure or judgment layer.
- [NIST AI Risk Management Framework](https://www.nist.gov/system/files/documents/2023/01/26/AI%20RMF%201.0.pdf) — Governance framework for AI risk. Operates at the organizational level; does not address meaning-layer design.
- [ISO/IEC 42001:2023](https://www.iso.org/standard/81230.html) — AI management system standard. Policy and governance focus; no meaning-layer specification.

**Documents in this repository:**

- [`foundation/s5la-v1.md`](../foundation/s5la-v1.md) — S5LA v1.0 full specification (frozen, English)
- [`framework/sif-v3.md`](../framework/sif-v3.md) — SIF v3.0 canonical working paper
- [`framework/ssf-v2.md`](../framework/ssf-v2.md) — SSF v2.0 with OWASP/NIST/ISO comparison appendix
- [`framework/ssf-v1.md`](../framework/ssf-v1.md) — SSF v1.0 diagnostic checklist and design guide

**Research areas SIF intersects:**

- LLM interpretability and mechanistic analysis (Anthropic, DeepMind, academic)
- Activation steering and representation engineering
- Constitutional AI and RLHF alignment methodology
- Multi-agent security and trust propagation

---

### E. License & Author

**Author:** Akira Hayakawa  
**Organization:** 3BPS (Three Brain Parallel System)  
**Contact:** via GitHub Issues  
**Version history:**

| Version | Name | Date | Status |
| --- | --- | --- | --- |
| 1.0 | SSF / S5LA initial | May 2026 | Frozen |
| 2.0 | SSF revised | May 2026 | Working Paper |
| 3.0 | SIF (renamed from SSF) | May 2026 | Working Paper |

> **Positioning note (SIF v3.0):** This framework addresses a distinct problem from
> AI offensive capability research (e.g., Anthropic's Mythos, May 2026, which demonstrated
> AI autonomously discovering and exploiting software vulnerabilities). SIF addresses the
> inverse failure mode: **not AI attacking external systems, but human intent failing to
> reach AI systems correctly**. These are complementary, not competing, problem domains.

---

*S5LA v1.0 status: Frozen. Layer definitions, processing division, and bridge model are fixed.
Application examples may be added; core definitions are not modified.*

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
