## Appendix

### A. Full Diagnostic Checklist

コピー＆ペースト可能。各項目を評価する: ✅ Pass / ❌ Fail / ❓ Insufficient information。

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

**採点:**

| Result | Interpretation |
| --- | --- |
| Any ★★★ ❌ at L3 | Critical — 他の作業の前に L3 の全面再設計 |
| Any ★★★ ❌ at L2 | High — L2 の再構成が必要; L1 の所見は暫定 |
| Any ★★★ ❌ at L1 | Medium — 対象を絞った修正で足りる |
| All ✅ | **SIF スコープ内で実装完了。**
           L4/L5 の残余リスクには別途のアーキテクチャ制御が必要。
           [Compensating controls](part2-research.md#open-problems-l4l5-attack-surface) を参照。 |

---

### B. Full Design Template

この順で構築する: L3 → L2 → L1。トップダウンでは構築しない。

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
| S5LA | Semantic 5-Layer Architecture — 人間のドメイン分類（L1）から AI 内部の意味原子（L5）までのセマンティック処理スタック全体 |
| SIF | Semantic Integrity Framework — LLM 意味層セキュリティのための3層診断・設計手法（L1–L3） |
| SSF | Semantic Security Framework — SIF の旧称（v1.0–v2.0）; 機能的に同等 |
| Semantic Injection | 構文レベルフィルタを迂回し、LLM 入力の意味層に埋め込まれる悪意ある指示 |
| Semantic Contamination | 信頼できない外部ソース（email, Slack, external APIs）からのコンテキスト腐敗が、AI の推論文脈を汚染すること |
| Intent Drift | 長いセッションやファインチューニングにわたる、元の設計意図からの AI 判断の漸進的乖離 |
| Role Rewrite | ロールプレイ、ペルソナ注入、敵対的プロンプティングによる AI アイデンティティの上書き |
| Pattern Exploitation | L4 における AI の暗黙的構造処理の操作 — 現時点で防御不能 |
| Atom-level Poisoning | L5 における最小意味単位の腐敗 — 現時点で防御不能 |
| Human-in-the-Point | すべてのステップではなく定義されたレバレッジポイントで人間判断を適用する; 高ボリューム AI ワークフローで Human-in-the-Loop を置き換える |
| Leverage Point | 不可逆性、スコープ影響、規範的複雑性、または検証不能な AI 確信度により人間判断を要する決定 |
| 3BPS | Three Brain Parallel System — 人間の意思決定、AI による認知制御、文脈メモリ、実行支援を分離し、人間の権限を保全し不安全な LLM 委任を防ぐ人間中心の AI 運用アーキテクチャ |
| Semantic Authorization Drift | AI モデルが認可スコープを再解釈する失敗モード — 明示的承認ではなく文脈シグナルから行動許可を推論する。リスクはモデル能力とともに増大する。 |
| Draft Exception Fallacy | Semantic Authorization Drift の亜型。AI が "Draft" をステータスラベルではなくサンドボックス環境として扱い、明示的承認前の行動を正当化する。"Draft is not Sandbox." |
| Capability-Confidence Loop | 高能力モデルが自己の能力を評価し、アクションが低リスクまたは可逆だと結論し、その根拠で承認プロセスを迂回する自己強化パターン。深刻度はモデル能力に比例する。 |

---

### D. Related Work

**本作業が依拠または補完するフレームワーク:**

- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — LLM アプリケーションの攻撃タクソノミー。Prompt injection をカバー（部分的な L3 重複）。セマンティック構造や判断層は扱わない。
- [NIST AI Risk Management Framework](https://www.nist.gov/system/files/documents/2023/01/26/AI%20RMF%201.0.pdf) — AI リスクのガバナンスフレームワーク。組織レベルで動作し、意味層設計は扱わない。
- [ISO/IEC 42001:2023](https://www.iso.org/standard/81230.html) — AI マネジメントシステム規格。ポリシーとガバナンスが焦点; 意味層の仕様はない。

**本リポジトリ内の文書:**

- [`foundation/s5la-v1.md`](../../foundation/s5la-v1.md) — S5LA v1.0 完全仕様（frozen, English）
- [`framework/sif-v3.md`](../../framework/sif-v3.md) — SIF v3.0 正典 working paper
- [`framework/ssf-v2.md`](../../framework/ssf-v2.md) — OWASP/NIST/ISO 比較付録付き SSF v2.0
- [`framework/ssf-v1.md`](../../framework/ssf-v1.md) — SSF v1.0 診断チェックリストと設計ガイド

**SIF が交差する研究領域:**

- LLM interpretability と mechanistic analysis（Anthropic, DeepMind, academic）
- Activation steering と representation engineering
- Constitutional AI と RLHF alignment methodology
- Multi-agent security と trust propagation

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

> **位置づけ注記（SIF v3.0）:** 本フレームワークは、AI 攻撃能力研究（例: Anthropic's Mythos, May 2026 —
> AI が自律的にソフトウェア脆弱性を発見・悪用することを示した）とは異なる問題を扱う。
> SIF が扱うのは逆の失敗モードである: **AI が外部システムを攻撃することではなく、
> 人間の意図が AI システムに正しく届かないこと**。これらは競合ではなく補完的な問題領域である。

---

*S5LA v1.0 status: Frozen. Layer definitions, processing division, and bridge model are fixed.
Application examples may be added; core definitions are not modified.*

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
