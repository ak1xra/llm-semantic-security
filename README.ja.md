# LLM Semantic Security

![Version](https://img.shields.io/badge/version-3.0-blue)
![Status](https://img.shields.io/badge/status-working%20paper-orange)
![License](https://img.shields.io/badge/license-MIT-green)
![S5LA](https://img.shields.io/badge/foundation-S5LA%20v1.0-green)
![Author](https://img.shields.io/badge/author-Akira%20Hayakawa%20%2F%203BPS-purple)

**注記:** Part I–III / Appendix / 3BPS の日本語版は [`docs/ja/`](docs/ja/) にあります。正典ソース（`foundation/s5la-*.md`, `framework/*`）は英語のままです。

> **"AI failures are semantic failures — and the security industry is defending the wrong attack surface."**

LLM システムの**意味層（meaning layer）** — ネットワークファイアウォール、エンドポイント検知、
アイデンティティ管理では到達できない攻撃面 — のための診断・設計フレームワークです。

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

## なぜ存在するのか

このフレームワークは研究室で設計されたものではありません。

日常の本番運用で構築・ストレステストされた、人間–AI 協働アーキテクチャ
**3BPS (Three Brain Parallel System)** — 個人向け AI オペレーティングシステム —
から生まれました。

> **3BPS (Three Brain Parallel System)** は、人間の意思決定、AI による認知制御、文脈メモリ、
> 実行支援を分離し、人間の権限を保全し、不安全な LLM への委任を防ぐ、
> 人間中心の AI 運用アーキテクチャです。

→ アーキテクチャ仕様の全文は [`docs/ja/3bps-architecture.md`](./docs/ja/3bps-architecture.md) を参照。

実運用の中で、同じパターンが繰り返し現れました。AI は正しい言葉を受け取っても、
正しい意図を受け取っていなかった。計画は生成されても守られない。制約は書かれても上書きされる。
アイデンティティ定義は存在するのに無視される。

これらは能力の失敗ではありません。人間の意図と AI の解釈のあいだで意味が失われる
**セマンティック失敗（semantic failures）** です。

既存のフレームワークはこの問題に名前を付けていませんでした。だから私たちは名付けました。

> *"The security industry is defending the wrong attack surface."*

SIF と S5LA はその結果です。既存のサイバーセキュリティフレームワークが扱わない
意味層のための診断構造です。

---

## インストール不要

SIF と S5LA はソフトウェアライブラリではなく、**ドキュメントフレームワーク**です。

- パッケージのインストール不要
- ランタイム環境不要
- API キー不要

使い方: [Appendix A](docs/ja/appendix.md#a-full-diagnostic-checklist) のチェックリストをコピーし、
システムプロンプトまたは AI デプロイに適用したうえで、
[診断プロトコル](docs/ja/part1-security.md#診断プロトコル) に従ってください。

---

## 目次

- [LLM Semantic Security](#llm-semantic-security)
  - [なぜ存在するのか](#なぜ存在するのか)
  - [インストール不要](#インストール不要)
  - [目次](#目次)
  - [Part I — セキュリティ専門家向け](docs/ja/part1-security.md)
    - [既存フレームワークが見落とすギャップ](docs/ja/part1-security.md#既存フレームワークが見落とすギャップ)
    - [意味層の攻撃ベクトル](docs/ja/part1-security.md#意味層の攻撃ベクトル)
    - [SIF: 3層防御アーキテクチャ](docs/ja/part1-security.md#sif-3層防御アーキテクチャ)
      - [L3 — Semantic Architecture (Identity Layer)](docs/ja/part1-security.md#l3--semantic-architecture-identity-layer)
      - [L2 — Semantic Structure (Relationship Layer)](docs/ja/part1-security.md#l2--semantic-structure-relationship-layer)
      - [L1 — Semantic Decision (Judgment Layer)](docs/ja/part1-security.md#l1--semantic-decision-judgment-layer)
    - [診断プロトコル](docs/ja/part1-security.md#診断プロトコル)
    - [設計ハードニングガイド](docs/ja/part1-security.md#設計ハードニングガイド)
      - [Step 1: Define L3 — Identity](docs/ja/part1-security.md#step-1-define-l3--identity)
      - [Step 2: Define L2 — Context Hierarchy](docs/ja/part1-security.md#step-2-define-l2--context-hierarchy)
      - [Step 3: Define L1 — Decision Rules](docs/ja/part1-security.md#step-3-define-l1--decision-rules)
    - [ケーススタディ](docs/ja/part1-security.md#ケーススタディ)
      - [Case Study A: Enterprise LLM Agent (Illustrative)](docs/ja/part1-security.md#case-study-a-enterprise-llm-agent-illustrative)
      - [Case Study B: Notion AI (Observed, January 2026)](docs/ja/part1-security.md#case-study-b-notion-ai-observed-january-2026)
      - [Case Study C: Notion AI Fable 5 (Observed, July 2026)](docs/ja/part1-security.md#case-study-c-notion-ai-fable-5-observed-july-2026)
    - [SIF vs. OWASP / NIST / ISO](docs/ja/part1-security.md#sif-vs-owasp--nist--iso)
  - [Part II — AI 研究者向け](docs/ja/part2-research.md)
    - [S5LA: 形式モデル](docs/ja/part2-research.md#s5la-形式モデル)
      - [層の定義](docs/ja/part2-research.md#層の定義)
      - [人間と AI の処理分担](docs/ja/part2-research.md#人間と-ai-の処理分担)
    - [Human-in-the-Point 原則](docs/ja/part2-research.md#human-in-the-point-原則)
    - [セマンティック腐敗の3つの文脈](docs/ja/part2-research.md#セマンティック腐敗の3つの文脈)
    - [Bridge Model](docs/ja/part2-research.md#bridge-model)
    - [Open Problems: L4–L5 Attack Surface](docs/ja/part2-research.md#open-problems-l4l5-attack-surface)
    - [研究アジェンダ](docs/ja/part2-research.md#研究アジェンダ)
  - [Part III — AI 愛好家向け](docs/ja/part3-enthusiast.md)
    - [平易な言葉で見る問題](docs/ja/part3-enthusiast.md#平易な言葉で見る問題)
    - [5分プロンプト監査](docs/ja/part3-enthusiast.md#5分プロンプト監査)
      - [Step 1: Identity check (L3)](docs/ja/part3-enthusiast.md#step-1-identity-check-l3)
      - [Step 2: Weight check (L2)](docs/ja/part3-enthusiast.md#step-2-weight-check-l2)
      - [Step 3: Judgment check (L1)](docs/ja/part3-enthusiast.md#step-3-judgment-check-l1)
    - [Before / After: 実際の失敗の書き換え](docs/ja/part3-enthusiast.md#before--after-実際の失敗の書き換え)
      - [Before: the original prompt structure](docs/ja/part3-enthusiast.md#before-the-original-prompt-structure)
      - [After: SIF-structured](docs/ja/part3-enthusiast.md#after-sif-structured)
    - [専門用語なしの Human-in-the-Point](docs/ja/part3-enthusiast.md#専門用語なしの-human-in-the-point)
    - [貢献する方法](docs/ja/part3-enthusiast.md#貢献する方法)
  - [Appendix](docs/ja/appendix.md)
    - [A. Full Diagnostic Checklist](docs/ja/appendix.md#a-full-diagnostic-checklist)
      - [L3 — Semantic Architecture](docs/ja/appendix.md#l3--semantic-architecture)
      - [L2 — Semantic Structure](docs/ja/appendix.md#l2--semantic-structure)
      - [L1 — Semantic Decision](docs/ja/appendix.md#l1--semantic-decision)
    - [B. Full Design Template](docs/ja/appendix.md#b-full-design-template)
    - [C. Glossary](docs/ja/appendix.md#c-glossary)
    - [D. Related Work](docs/ja/appendix.md#d-related-work)
    - [E. License \& Author](docs/ja/appendix.md#e-license--author)

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
