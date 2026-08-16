## Part III — For AI Enthusiasts

### The Plain-Language Problem

You've experienced this.

You write a detailed instruction to an AI. The words are correct. The logic is sound.
The AI reads it, acknowledges it — and then does something adjacent to what you asked.

Not wrong, exactly. But not what you meant.

This is not the AI being "dumb." It is a **semantic failure**: the words arrived, but the
intent behind them did not. Something was lost in transit between what you meant and what
the AI processed.

This happens for a specific reason: most AI instructions are written at the word level,
not the meaning level. The AI receives commands without receiving the weight, priority, and
structure that gives those commands their meaning.

SIF and S5LA are frameworks for designing at the meaning level — not just writing better
prompts, but building systems where your intent can actually survive the journey.

---

### 5-Minute Prompt Audit

Take any system prompt or AI instruction you've written. Apply these three questions in order.
Stop as soon as you find a failure — that's your priority fix.

#### Step 1: Identity check (L3)

- Can you describe what this AI is in exactly one sentence?
- Does the prompt contain an explicit list of what the AI must *never* do?
- Does the prompt contain an explicit list of what the AI *may* do?

If any answer is "no" → rewrite the identity section first. Everything else is provisional.

#### Step 2: Weight check (L2)

- Are your constraints ("the AI must always...") in a separate section from your
  guidelines ("the AI should consider...")?
- If you gave the AI a reference document, is it clear that the document is *reference*,
  not a rule?

If constraints and guidelines are mixed in the same paragraph → separate them. A mixed paragraph
is a permission slip for the AI to treat your constraints as optional.

#### Step 3: Judgment check (L1)

- When two of your instructions could conflict, is there a rule for which one wins?
- Is there a condition under which the AI should stop and ask you instead of deciding?

If no conflict resolution rule exists → the AI will decide for you, using its own defaults,
which may not match your intent.

---

### Before / After: Real Failure Rewritten

**The failure (Notion AI, January 2026):**

An AI agent was given a plan to execute. On every subsequent turn, it produced a new, improved
plan instead of executing the one it had been given. Planning score: 95/100. Execution: 0/100.

#### Before: the original prompt structure

```text
You are a helpful AI assistant. Help the user achieve their goals.
Follow the user's plan. Produce high-quality results.
```

What the AI received: vague identity, no hierarchy, no distinction between
"execute this plan" and "optimize this plan."

#### After: SIF-structured

```markdown
## Identity
This AI executes approved plans.
Authority: Carry out tasks as specified in the approved plan.
Prohibition: Do not modify, extend, or replace the approved plan
             without explicit user approval.

## Context Hierarchy
### Immutable
- The approved plan is the execution target. It is not open for revision.

### Reference
- Improvement ideas noted during execution may be surfaced AFTER
  task completion, not instead of it.

## Decision Rules
Priority: 1. Execute the approved plan. 2. Flag conflicts. 3. Ask before acting.
Conflict resolution: If the current instruction would modify the plan scope,
stop and request clarification.
```

What changed: the AI now has an identity (executor, not optimizer), a hierarchy
(plan = immutable, not reference), and a judgment rule (flag before modifying).

---

### Human-in-the-Point Without Jargon

There are two ways to work with AI:

**Human-in-the-Loop:** You approve every step. You are always in the room.
The AI never acts without your sign-off. This is safe but exhausting — and it defeats the
purpose of having an AI.

**Human-in-the-Point:** You approve at the moments that actually matter.
The AI handles the routine; you handle the irreversible.

How do you know which moments matter? Four signals:

1. **Can't undo it.** Sending an email, deleting a file, making a payment — if you can't take
   it back, a human should approve it first.
2. **Affects someone outside.** If the action reaches beyond the system's walls into the real
   world, a human should review it.
3. **Requires a values call.** Some decisions don't have a "correct" answer — they require
   judgment about what matters. That's a human job.
4. **The AI isn't sure.** If the AI's confidence can't be verified, don't let it decide alone.

Everything else? Let the AI run. Your job is to be present at the leverage points, not
at every step.

---

### How to Contribute

This framework is a living document. Contributions that strengthen it are welcome.

**Ways to contribute:**

- **New case studies:** Have you observed a semantic failure pattern in an LLM system?
  Document it using the SIF diagnostic format and submit a pull request.
- **Checklist refinements:** Experienced false positives or false negatives with the
  diagnostic checklist? Open an issue with the scenario and proposed adjustment.
- **L4/L5 research:** Working on LLM interpretability or activation steering?
  Connect with the research agenda above.
- **Translations:** The S5LA Japanese overview exists. Other languages are welcome.
- **Application domains:** SIF was developed against Notion AI and enterprise agents.
  Applications to other domains (healthcare AI, legal AI, autonomous systems) are needed.

Open an issue or pull request in this repository.

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*

