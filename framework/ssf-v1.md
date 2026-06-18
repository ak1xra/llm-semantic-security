# Semantic Security Framework

## AI Security Framework (3 Layers × 3 Contexts)

Version: **1.0** Status: **Production** Owner: **Akira Hayakawa / 3BPS** Purpose: **Diagnosis and Design of AI System Prompts**

---

## 0.1 Why This Framework Operates on "Semantics (Meaning / Intent)"

When an AI behaves unexpectedly, the root cause is almost never a missing feature.  
It is almost always that **human intent failed to reach the AI**.

Examples:

- Wrote "follow the plan" → the AI treated the plan as optional reference
- Wrote "respect the constraint" → the AI processed constraints and recommendations as equal
- Wrote "prioritize user intent" → the AI prioritized its own judgment

In each case, the **words** of the instruction arrived.  
But the **intent** behind the instruction did not.

This framework designs and diagnoses the delivery of intent across three layers.

---

## 0. Definition

**Semantic Security Framework (SSF)** is a framework that classifies "semantic misinterpretation" in AI systems into three layers, functioning as both a **diagnostic tool** and a **design guide**.

The reason "Semantic" runs through every layer:  
**AI failures originate not from missing features, but from semantic misinterpretation.**

---

## 1. Structure (read bottom-up)

```
L1 — Semantic Decision   (apex)
     Criteria for "what gets decided and how"
          ↑
L2 — Semantic Structure  (middle)
     Structure for "how information is handled"
          ↑
L3 — Semantic Architecture (foundation)
     Design of "what kind of entity this AI is"
```

If the foundation collapses, all layers collapse.  
Upper-layer corrections alone cannot fix a broken foundation.

---

## 2. Layer Definitions

### L3 — Semantic Architecture (Existence Definition Layer)

| Item | Content |
| --- | --- |
| Definition | The foundation that defines "what kind of entity the AI operates as" |
| Role | Anchors the meaning of purpose, authority, and constraints |
| Diagnostic criterion | Does this AI correctly interpret its own reason for existence? |
| Design criterion | Define existence-level meaning first, then build everything else on top |

**Failure patterns:**

- The AI does not correctly recognize its own purpose or scope of authority
- Constraints are not embedded as "design philosophy"
- Functional definitions are layered on top of an ambiguous existence definition

---

### L2 — Semantic Structure (Relationship Definition Layer)

| Item | Content |
| --- | --- |
| Definition | The middle layer that defines "weight, order, and relationships" between information |
| Role | Maintains the distinction between constraints / recommendations / facts / hypotheses at the semantic level |
| Diagnostic criterion | Are information types mixed or treated as equal? |
| Design criterion | Design semantic separation for each information type |

**Failure patterns:**

- Constraints (must follow) and recommendations (should consider) treated as equal weight
- Facts and hypotheses processed without distinction
- Information priority undefined at the semantic level

---

### L1 — Semantic Decision (Judgment Criteria Definition Layer)

| Item | Content |
| --- | --- |
| Definition | The apex that defines criteria for "what gets decided and how" |
| Role | Provides a semantic axis to the AI's output judgment logic |
| Diagnostic criterion | Are the judgment criteria diverging from user intent? |
| Design criterion | Make the priority of judgment explicit at the semantic level |

**Failure patterns:**

- Judgment criteria remain implicit (not explicitly defined)
- The AI's judgment axis diverges from user intent
- Competing judgment criteria exist in the prompt without conflict resolution rules

---

## 3. Diagnostic Checklist

Used for evaluating existing AI systems. Rate each item as **✅ / ❌ / ❓**.

### L3 — Semantic Architecture Diagnosis

```
[ ] Is the AI's purpose clearly defined in one sentence?
[ ] Is the scope of authority explicitly stated (what it may / may not do)?
[ ] Are constraints embedded as "design philosophy" at the foundation?
[ ] Is the existence definition placed at the top of the system prompt?
[ ] Are the three elements — purpose, authority, and constraints — non-contradictory?
```

### L2 — Semantic Structure Diagnosis

```
[ ] Are constraints and recommendations clearly separated?
[ ] Are facts and hypotheses described with distinction?
[ ] Is the priority order of information defined at the semantic level?
[ ] Is the structural hierarchy (higher / lower) explicit?
[ ] Is each information type isolated in its own section?
```

### L1 — Semantic Decision Diagnosis

```
[ ] Are the judgment criteria explicitly stated?
[ ] Is the priority order of judgment defined?
[ ] Are the user intent and the AI's judgment axis aligned?
[ ] When competing judgment criteria exist, is a resolution rule defined?
[ ] Are exception cases documented?
```

### Overall Assessment

```
All ✅              → Design is appropriate
Any ❌ at L3        → Redesign all layers
Any ❌ at L2 (L3 ✅) → Structural reorganization required
Only L1 has ❌      → Addressable by adding judgment criteria
Many ❓             → Insufficient information — confirm with designer
```

---

## 4. Design Guide (for new system prompts)

Procedure for designing new AI systems. **Always build from L3 upward.**

### Step 1: Define L3 — Semantic Architecture

```
## Identity (Existence Definition)
What this AI is: [describe in 1 sentence]
Purpose: [why it exists]
Authority: [what it may do]
Prohibition: [what it must never do]
```

**Checkpoints:**

- Can the purpose be written in one sentence?
- Are authority and prohibition specific? ("appropriately" is prohibited)
- Can you picture how the AI will behave from this definition alone?

---

### Step 2: Define L2 — Semantic Structure

```
## Context Hierarchy

### Immutable (non-negotiable)
- [Absolute constraints and preconditions]

### Constraints
- [Rules to follow]

### Guidelines (recommendations)
- [Things to consider]

### Reference
- [Information that may be used]
```

**Checkpoints:**

- Is the distinction between each section clear?
- Are Immutable and Constraints not mixed?
- Is there a risk of Guidelines being misread as Constraints?

---

### Step 3: Define L1 — Semantic Decision

```
## Decision Rules

### Priority Order
1. [Highest priority]
2. [Secondary]
3. [Everything else]

### Conflict Resolution
- [How to resolve when judgment criteria conflict]

### Edge Cases
- [Situations where normal rules do not apply]
```

**Checkpoints:**

- Is the priority order stated numerically?
- Are conflict cases anticipated?
- Are edge cases isolated as "exceptions"?

---

### Design Completion Check

```
L3 defined → L2 defined → L1 defined
     ↓              ↓              ↓
Existence check  Structure check  Judgment check
     ↓
Cross-layer consistency check (no contradictions?)
     ↓
Complete
```

---

## 5. Diagnostic Case Study: Notion AI

**Target:** Notion AI Custom Agent  
**Evaluation date:** January 14, 2026

### Diagnostic Results

| Layer | Assessment | Basis |
| --- | --- | --- |
| L3 | ❌ | Not designed as an entity that understands Notion's constraints |
| L2 | ❌ | Planning documents (constraints) and proposals (recommendations) processed as equal |
| L1 | ❌ | "Plan adherence" not embedded as a judgment criterion |

**Overall assessment:** Full redesign required from L3

**Observed failure patterns:**

- Planning: 95 points / Plan adherence: 0 points (classic L1 failure)
- Mixing of constraints and recommendations (L2 failure)
- Absence of design philosophy (L3 failure)

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
