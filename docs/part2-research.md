## Part II — For AI Researchers

### S5LA: The Formal Model

The **Semantic 5-Layer Architecture (S5LA)** models the full semantic processing stack between
human intent and LLM output. It is the theoretical foundation of SIF.

```text
(L1) Semantic Category    — Domain classification ("what world is this?")
(L2) Semantic Layer       — Functional hierarchy (responsibility separation)
(L3) Semantic Object      — Concrete specifications, policies, documents
(L4) Semantic Pattern     — Structural templates [AI-processed / human-invisible]
(L5) Semantic Atom        — Minimum meaning units [AI-internal / human-invisible]
```

Read bottom-up: L5 is the foundation; L1 is the highest-level classification.

#### Layer Definitions

**L1 — Semantic Category (Domain Layer)**
Top-level classification of the world a conversation belongs to.
Without L1, AI systems cannot correctly orient themselves to a request domain.
*Examples:* Semantic Architecture, Cognitive Architecture, Execution Architecture

**L2 — Semantic Layer (Functional Hierarchy Layer)**
Internal functional hierarchy within a category. Separates responsibilities and contexts.
Defines "whose responsibility is this?" at the meaning level.
*Examples:* Alignment Layer, Reasoning Layer, Planning Layer, Memory Layer, Action Layer

**L3 — Semantic Object / Spec (Document Layer)**
Concrete entities: specifications, policies, rulesets, configuration documents.
The implementation unit where human designers make explicit decisions.
*Examples:* Meta-Instruction Spec, Alignment Policy, Execution Rulebook

**L4 — Semantic Pattern (Structural Template Layer)**
Recurring structural templates humans apply unconsciously; AI processes implicitly.
When patterns break, AI systems lose their structural anchor.
*Examples:* "Current state → Goal → Problem → Solution"; "List → Manage → Activate → Recovery"

**L5 — Semantic Atom (Minimum Meaning Unit Layer)**
Smallest indivisible units of meaning, processed internally.
Invisible to humans during normal operation. Corruption here affects all layers above.
*Examples:* Command names, role words, modality markers, intent flags, labels

#### Human vs. AI Processing Division

| Layer | Primary Processor | Human Visibility | Defensive Access |
| --- | --- | --- | --- |
| L1 Category | Human | Explicit | ✅ Full |
| L2 Layer | Human | Explicit | ✅ Full |
| L3 Object | Human | Explicit | ✅ Full |
| L4 Pattern | AI | Implicit | ⚠️ Indirect only |
| L5 Atom | AI | Invisible | ❌ None currently |

**Key insight:** Humans operate consciously at L1–L3. AI operates at L4–L5 without human
visibility. Effective human-AI collaboration requires deliberate design at L1–L3 so that L4–L5
processing produces intended results.

---

### Human-in-the-Point Principle

S5LA leads directly to the **Human-in-the-Point** principle — a departure from Human-in-the-Loop.

The distinction:

| Model | Assumption | Cognitive Cost | Control |
| --- | --- | --- | --- |
| Human-in-the-Loop | Human reviews every step | High | High (but unsustainable) |
| **Human-in-the-Point** | Human reviews at leverage points | Low | Targeted — where it matters |

**A decision qualifies as a leverage point when any of the following are true:**

1. The action is irreversible (deletion, transmission, financial transaction)
2. The action affects parties outside the system's defined scope
3. The action requires normative judgment not captured in the system's constraint set
4. The confidence of the AI's judgment cannot be independently verified

Human-in-the-Point reduces cognitive load while maintaining meaningful human authority over
consequential decisions. Judgment frequency decreases; judgment precision increases.

**Why human judgment — not AI — at leverage points?**

Because in human society, only humans can be held accountable.

AI can optimize, expand, and iterate at scale. But accountability — the capacity to bear
responsibility for a decision's consequences within social, legal, and ethical structures —
belongs to humans. This is not a technical limitation of current AI. It is a structural
feature of human society that no capability improvement changes.

Human-in-the-Point is therefore not a temporary workaround until AI improves. It is a
permanent architectural principle: AI operates at L4–L5 (pattern and atom); humans operate
at L1–L3 (category, layer, object) and retain judgment at the points where accountability
cannot be delegated.

SIF operationalizes this through the L1 Escalation rule: when the AI cannot resolve a decision
within its defined constraint set, it routes to human judgment rather than defaulting to action.

---

### Three Contexts of Semantic Corruption

SIF operates across three contexts where semantic integrity can be violated:

| Context | Description | Primary Risk |
| --- | --- | --- |
| **Input Context** | User input, external documents, reference data | Malicious instructions; context contamination from untrusted sources |
| **Reasoning Context** | Internal interpretation and prioritization within the LLM | Semantic mixing; hidden priority drift; judgment criteria overridden by implicit patterns |
| **Output Context** | Final response, executed actions, generated artifacts | Unsafe, misaligned, or malformed output that diverges from original intent |

The three SIF layers define **what must be preserved** in each context.
The three contexts define **where semantic corruption may occur**.
A complete assessment examines both dimensions: 3 layers × 3 contexts = 9 evaluation cells.

---

### The Bridge Model

S5LA is best understood as a **bridge** between human intent and AI processing —
not a translation layer, but a structural connector.

| Bridge Function | Description |
| --- | --- |
| Load distribution | Humans carry large meaning blocks; AI carries high-volume micro-units |
| Traffic direction | Human → intent / values / judgment; AI → expansion / optimization / iteration |
| Fall prevention | Prevents meaning misrouting, layer absorption, phantom existence (display without function) |

**Collapse pattern:** When Semantic Category is correct but Semantic Layer is misrouted,
Semantic Object crosses wires, Semantic Pattern is shared across contexts, and Semantic Atoms
leave the visible field — the result is the worst possible failure mode:
*"I understood correctly, and was still penalized for it."*

---

### Open Problems: L4–L5 Attack Surface

SIF explicitly acknowledges its defensive boundary: **L4 and L5 are currently undefendable.**

This is not a design gap. It reflects the current state of LLM interpretability and
controllability research. Claiming protection at these layers would be misleading.

**L4 — Pattern Exploitation:**
AI's structural templates are implicit and not externally observable. An adversary who
understands how a specific model processes structural patterns can craft inputs that route
through the model's pattern-processing in unintended ways, without violating any explicit rule.
Candidate research: **APD / Semantic Graph Defense** as an external proxy for estimating L4
patterns at the L3→L4 boundary (not claimed defense).

**L5 — Atom-level Poisoning:**
The minimum meaning units processed inside LLMs are not accessible through current API surfaces.
Fine-tuning, RLHF poisoning, and prompt-level injection that targets atomic representations
operate entirely outside current defensive access.
Candidate research: APD Semantic Components as an L5 proxy for correspondence studies
(**APD Semantic Component ≠ S5LA Semantic Atom**).

**Compensating controls (architectural-level):**

- Sandboxing: constrain agent permissions to minimum required scope
- Capability restriction: remove unused permissions at deployment time
- Output monitoring: behavioral anomaly detection on agent outputs
- Human-in-the-Point: route consequential decisions to human judgment regardless of L1–L3 compliance

---

### Research Agenda

Contributions are welcomed in the following areas:

- **Empirical validation:** Quantitative measurement of SIF-compliant vs. non-compliant
  system vulnerability rates across diverse LLM deployments
- **L4 interpretability:** Mechanistic analysis of transformer attention patterns as a
  basis for structural template introspection; APD / Semantic Graph Defense as a candidate
  external observation path (see `framework/sif-v3.md` H1–H5)
- **L5 access:** Activation steering and representation engineering as paths toward
  atom-level defensive access; APD Semantic Component as proxy (not identity) for L5 mapping studies
- **Human-in-the-Point operationalization:** Threshold calibration across organizational
  contexts, roles, and risk tolerances
- **SIF extension:** Application to multi-agent pipelines, where semantic corruption
  can propagate across agent boundaries
- **APD connection hypotheses (H1–H5):** Graph↔L4 correspondence; Component↔L5 mapping;
  Semantic Routing attacks; cross-lingual integrity; graph-constructor adversarial robustness
  (research agenda only — see SIF v3 Future work)
- **APD Semantic Graph Defense integration:** Adversarial Prompt Disentanglement
  (Fang & Fang, AAAI 2026) as L3→L4 boundary inspection research — prototype
  Semantic Boundary Inspector (SBI) to evaluate whether external Semantic Graph
  construction can serve as an indirect proxy for L4 Semantic Pattern observation.
  See: https://doi.org/10.1609/aaai.v40i5.37389

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*

