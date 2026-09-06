# 3BPS — Three Brain Parallel System

## Definition

**3BPS（Three Brain Parallel System）** は、**decision**、**control**、**memory**、**execution** を
別コンポーネントに分離する、人間中心の AI 運用アーキテクチャである。

LLM を自律的な意思決定者としてではなく、人間の意思決定権限に接続された外部認知コンポーネントとして扱う。

3BPS の目的は、次を保証することで不安全な AI 運用を防ぐことである:

- 人間が最終意思決定権限を保持する、
- 制御層がリスクを検知し実行を規制する、
- メモリ層が決定せずに文脈を保全する、
- 実行層が明示的な人間の承認後にのみ行動する。

要するに:

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

3BPS は **Human-in-the-Point** の原則を中心に設計されている。

これは、人間が単なるレビュアーとして「ループの中」にいるだけではない、という意味である。

人間は、判断・責任・最終承認が錨を下ろす権限のポイントであり続ける。

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

LLM の失敗はモデル側の問題だけではない。

多くの失敗は、人間の認知、文脈、メモリ、実行が単一の不安定な流れに混ざることで起きる。

3BPS はこれらの機能を分離し、各層が狭い責任を持つようにする。

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

3BPS は次を分離することでこれを防ぐ:

```text
decision ≠ control
control ≠ memory
memory ≠ execution
execution ≠ approval

```

---

## Security Function

3BPS は LLM 運用のための実用的なセキュリティ境界を提供する。

次のリスクを低減する:

- prompt injection による意図しない実行、
- LLM による人間判断の上書き、
- memory contamination、
- context drift、
- 明示的承認なしの実行、
- 不明瞭な説明責任、
- AI 生成結論への過度の依存。

---

## Relationship to SIF / S5LA / SSF

3BPS は、セキュリティとアラインメントの原則を実運用に適用する **operational architecture** である。

| Framework | Role |
| --- | --- |
| SIF | Semantic integrity / meaning-layer defense |
| S5LA | Layered security and alignment structure |
| SSF | Safety specification / secure system framing |
| 3BPS | Human-centered operational architecture |

3BPS が答える実務上の問いは次である:

```text
Who decides?
Who controls?
Who remembers?
Who executes?
Who records?

```

---

## Minimal Rule Set

1. 人間が最終決定を行う。
2. 制御層は停止、警告、または再構成を行ってよい。
3. メモリ層は文脈を再構成してよいが、決定してはならない。
4. 実行層は出力を生成してよいが、自己実行してはならない。
5. 追跡可能性のために永続ログが必須である。
6. 明示的な人間の承認なしに外部アクションを実行しない。

---

## Short Definition

**3BPS は、不安全な委任を防ぎ人間の権限を保全するため、decision・control・memory・execution を分離する、人間中心の LLM 運用アーキテクチャである。**

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
