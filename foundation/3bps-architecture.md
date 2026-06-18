# 3BPS — Three Brain Parallel System

## Definition

**3BPS（Three Brain Parallel System）** is a human-centered AI operation architecture that separates **decision**, **control**, **memory**, and **execution** into distinct components.

It treats LLMs not as autonomous decision-makers, but as external cognitive components connected to a human decision authority.

The purpose of 3BPS is to prevent unsafe AI operation by ensuring that:

- the human retains final decision authority,
- the control layer detects risk and regulates execution,
- the memory layer preserves context without making decisions,
- the execution layer acts only after explicit human approval.

In short:

```text
3BPS = Human decision authority + AI control layer + AI memory layer + execution layer

```

---

## Core Principle

```text
AI may assist cognition.
AI may structure information.
AI may retrieve and reconstruct context.
AI may support execution.

AI must not become the final decision-maker.

```

3BPS is designed around the principle of **Human-in-the-Point**.

This means the human is not merely “in the loop” as a reviewer.

The human remains the point of authority where judgment, responsibility, and final approval are anchored.

---

## Component Model

```text
Human
↓
Control Layer
↓
Memory Layer
↓
Execution Layer
↓
Persistent Log

```

| Component | Role | Authority |
| --- | --- | --- |
| Human / AK1RA OS | Final decision, responsibility, approval | Full decision authority |
| Control Layer / Oni Coach | Cognitive control, risk detection, execution brake | No decision authority |
| Memory Layer / Claude | Context reconstruction, summarization, semantic memory | No decision authority |
| Execution Layer / Cursor / Codex | Implementation, writing, coding, operational output | No decision authority |
| Persistent Log / Notion | Decision log, source of truth, audit trail | No decision authority |

---

## Why 3BPS Exists

LLM failures are not only model-side problems.

Many failures occur because human cognition, context, memory, and execution are mixed into a single unstable flow.

3BPS separates these functions so that each layer has a narrow responsibility.

```text
Mixed cognition
↓
role confusion
↓
unsafe delegation
↓
implicit execution
↓
loss of accountability

```

3BPS prevents this by separating:

```text
decision ≠ control
control ≠ memory
memory ≠ execution
execution ≠ approval

```

---

## Security Function

3BPS provides a practical security boundary for LLM operation.

It reduces the risk of:

- prompt injection causing unintended execution,
- LLMs overriding human judgment,
- memory contamination,
- context drift,
- execution without explicit approval,
- unclear accountability,
- over-reliance on AI-generated conclusions.

---

## Relationship to SIF / S5LA / SSF

3BPS is the **operational architecture** that applies security and alignment principles in real usage.

| Framework | Role |
| --- | --- |
| SIF | Semantic integrity / meaning-layer defense |
| S5LA | Layered security and alignment structure |
| SSF | Safety specification / secure system framing |
| 3BPS | Human-centered operational architecture |

3BPS answers the practical question:

```text
Who decides?
Who controls?
Who remembers?
Who executes?
Who records?

```

---

## Minimal Rule Set

1. The human makes the final decision.
2. The control layer may stop, warn, or restructure.
3. The memory layer may reconstruct context but must not decide.
4. The execution layer may produce outputs but must not self-execute.
5. Persistent logs are required for traceability.
6. No external action is performed without explicit human approval.

---

## Short Definition

**3BPS is a human-centered LLM operation architecture that separates decision, control, memory, and execution to prevent unsafe delegation and preserve human authority.**

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
