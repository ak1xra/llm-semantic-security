# LLM Semantic Security

![Version](https://img.shields.io/badge/version-3.0-blue)
![Status](https://img.shields.io/badge/status-working%20paper-orange)
![License](https://img.shields.io/badge/license-MIT-green)
![S5LA](https://img.shields.io/badge/foundation-S5LA%20v1.0-green)
![Author](https://img.shields.io/badge/author-Akira%20Hayakawa%20%2F%203BPS-purple)

> **"AI failures are semantic failures — and the security industry is defending the wrong attack surface."**

A diagnostic and design framework for the **meaning layer** of LLM systems — the attack surface that
network firewalls, endpoint detection, and identity management cannot reach.

```text
┌─────────────────────────────────────────────────────────────────┐
│              Semantic 5-Layer Architecture (S5LA)               │
│                                                                 │
│  (L1) Semantic Category  ──  Domain classification             │◄── Human
│  (L2) Semantic Layer     ──  Functional hierarchy              │◄── Human
│  (L3) Semantic Object    ──  Specifications, policies          │◄── Human  ← SIF defends here
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ Defensive Boundary ─ ─ ─ ─ ─ ─ ─ ─│
│  (L4) Semantic Pattern   ──  Structural templates [implicit]   │◄── AI      ⚠ undefended
│  (L5) Semantic Atom      ──  Minimum meaning units [internal]  │◄── AI      ✗ undefended
└─────────────────────────────────────────────────────────────────┘

      SIF 3-Layer Defense (applied to L1–L3 of S5LA)
      ┌──────────────────────────────────┐
      │  L1 — Semantic Decision  (apex)  │  Judgment criteria & conflict resolution
      │          ↑                       │
      │  L2 — Semantic Structure (mid)   │  Information weight & priority hierarchy
      │          ↑                       │
      │  L3 — Semantic Architecture(base)│  Identity, authority, prohibition
      └──────────────────────────────────┘
      Build bottom-up. If L3 fails, stop. All layers collapse.
```

---

## Why This Exists

This framework was not designed in a laboratory.

It emerged from a personal AI operating system — a human-AI collaboration architecture called
**3BPS (Three Brain Parallel System)** — built and stress-tested in daily production use.

> **3BPS (Three Brain Parallel System)** is a human-centered AI operation architecture that
> separates human decision-making, AI-based cognitive control, contextual memory, and execution
> support to preserve human authority and prevent unsafe LLM delegation.

→ See [`foundation/3bps-architecture.md`](./foundation/3bps-architecture.md) for the full architecture specification.

During real operations, a pattern kept appearing: the AI received the correct words, but not the
correct intent. Plans were generated but not followed. Constraints were written but overridden.
Identity definitions were present but ignored.

These were not capability failures. They were **semantic failures** — meaning lost in transit between
human intent and AI interpretation.

No existing framework named this problem. So we named it.

> *"The security industry is defending the wrong attack surface."*

SIF and S5LA are the result: a diagnostic structure for the meaning layer that existing
cybersecurity frameworks do not address.

---

## No Installation Required

SIF and S5LA are **documentation frameworks**, not software libraries.

- No package installation
- No runtime environment
- No API keys

To use: copy the checklist in [Appendix A](docs/appendix.md#a-full-diagnostic-checklist),
apply it to your system prompt or AI deployment, and follow the
[Diagnostic Protocol](docs/part1-security.md#diagnostic-protocol).

---

## Table of Contents

- [LLM Semantic Security](#llm-semantic-security)
  - [Why This Exists](#why-this-exists)
  - [No Installation Required](#no-installation-required)
  - [Table of Contents](#table-of-contents)
  - [Part I — For Security Professionals](docs/part1-security.md)
    - [The Gap: What Existing Frameworks Miss](docs/part1-security.md#the-gap-what-existing-frameworks-miss)
    - [Meaning-Layer Attack Vectors](docs/part1-security.md#meaning-layer-attack-vectors)
    - [SIF: Three-Layer Defense Architecture](docs/part1-security.md#sif-three-layer-defense-architecture)
      - [L3 — Semantic Architecture (Identity Layer)](docs/part1-security.md#l3--semantic-architecture-identity-layer)
      - [L2 — Semantic Structure (Relationship Layer)](docs/part1-security.md#l2--semantic-structure-relationship-layer)
      - [L1 — Semantic Decision (Judgment Layer)](docs/part1-security.md#l1--semantic-decision-judgment-layer)
    - [Diagnostic Protocol](docs/part1-security.md#diagnostic-protocol)
    - [Design Hardening Guide](docs/part1-security.md#design-hardening-guide)
      - [Step 1: Define L3 — Identity](docs/part1-security.md#step-1-define-l3--identity)
      - [Step 2: Define L2 — Context Hierarchy](docs/part1-security.md#step-2-define-l2--context-hierarchy)
      - [Step 3: Define L1 — Decision Rules](docs/part1-security.md#step-3-define-l1--decision-rules)
    - [Case Studies](docs/part1-security.md#case-studies)
      - [Case Study A: Enterprise LLM Agent (Illustrative)](docs/part1-security.md#case-study-a-enterprise-llm-agent-illustrative)
      - [Case Study B: Notion AI (Observed, January 2026)](docs/part1-security.md#case-study-b-notion-ai-observed-january-2026)
      - [Case Study C: Notion AI Fable 5 (Observed, July 2026)](docs/part1-security.md#case-study-c-notion-ai-fable-5-observed-july-2026)
    - [SIF vs. OWASP / NIST / ISO](docs/part1-security.md#sif-vs-owasp--nist--iso)
  - [Part II — For AI Researchers](docs/part2-research.md)
    - [S5LA: The Formal Model](docs/part2-research.md#s5la-the-formal-model)
      - [Layer Definitions](docs/part2-research.md#layer-definitions)
      - [Human vs. AI Processing Division](docs/part2-research.md#human-vs-ai-processing-division)
    - [Human-in-the-Point Principle](docs/part2-research.md#human-in-the-point-principle)
    - [Three Contexts of Semantic Corruption](docs/part2-research.md#three-contexts-of-semantic-corruption)
    - [The Bridge Model](docs/part2-research.md#the-bridge-model)
    - [Open Problems: L4–L5 Attack Surface](docs/part2-research.md#open-problems-l4l5-attack-surface)
    - [Research Agenda](docs/part2-research.md#research-agenda)
  - [Part III — For AI Enthusiasts](docs/part3-enthusiast.md)
    - [The Plain-Language Problem](docs/part3-enthusiast.md#the-plain-language-problem)
    - [5-Minute Prompt Audit](docs/part3-enthusiast.md#5-minute-prompt-audit)
      - [Step 1: Identity check (L3)](docs/part3-enthusiast.md#step-1-identity-check-l3)
      - [Step 2: Weight check (L2)](docs/part3-enthusiast.md#step-2-weight-check-l2)
      - [Step 3: Judgment check (L1)](docs/part3-enthusiast.md#step-3-judgment-check-l1)
    - [Before / After: Real Failure Rewritten](docs/part3-enthusiast.md#before--after-real-failure-rewritten)
      - [Before: the original prompt structure](docs/part3-enthusiast.md#before-the-original-prompt-structure)
      - [After: SIF-structured](docs/part3-enthusiast.md#after-sif-structured)
    - [Human-in-the-Point Without Jargon](docs/part3-enthusiast.md#human-in-the-point-without-jargon)
    - [How to Contribute](docs/part3-enthusiast.md#how-to-contribute)
  - [Appendix](docs/appendix.md)
    - [A. Full Diagnostic Checklist](docs/appendix.md#a-full-diagnostic-checklist)
      - [L3 — Semantic Architecture](docs/appendix.md#l3--semantic-architecture)
      - [L2 — Semantic Structure](docs/appendix.md#l2--semantic-structure)
      - [L1 — Semantic Decision](docs/appendix.md#l1--semantic-decision)
    - [B. Full Design Template](docs/appendix.md#b-full-design-template)
    - [C. Glossary](docs/appendix.md#c-glossary)
    - [D. Related Work](docs/appendix.md#d-related-work)
    - [E. License \& Author](docs/appendix.md#e-license--author)

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*

