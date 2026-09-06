## Part II — AI 研究者向け

### S5LA: 形式モデル

**Semantic 5-Layer Architecture (S5LA)** は、人間の意図と LLM 出力のあいだのセマンティック処理スタック全体を
モデル化する。SIF の理論的基盤である。

```text
(L1) Semantic Category    — Domain classification ("what world is this?")
(L2) Semantic Layer       — Functional hierarchy (responsibility separation)
(L3) Semantic Object      — Concrete specifications, policies, documents
(L4) Semantic Pattern     — Structural templates [AI-processed / human-invisible]
(L5) Semantic Atom        — Minimum meaning units [AI-internal / human-invisible]
```

ボトムアップで読む: L5 が基盤、L1 が最上位の分類である。

#### 層の定義

**L1 — Semantic Category (Domain Layer)**
会話が属する世界の最上位分類。
L1 がなければ、AI システムは要求ドメインに正しく定位できない。
*例:* Semantic Architecture, Cognitive Architecture, Execution Architecture

**L2 — Semantic Layer (Functional Hierarchy Layer)**
カテゴリ内の内部機能階層。責任と文脈を分離する。
意味レベルで「これは誰の責任か？」を定義する。
*例:* Alignment Layer, Reasoning Layer, Planning Layer, Memory Layer, Action Layer

**L3 — Semantic Object / Spec (Document Layer)**
具体的実体: 仕様、ポリシー、ルールセット、設定ドキュメント。
人間の設計者が明示的決定を行う実装単位。
*例:* Meta-Instruction Spec, Alignment Policy, Execution Rulebook

**L4 — Semantic Pattern (Structural Template Layer)**
人間が無意識に適用し、AI が暗黙に処理する反復的な構造テンプレート。
パターンが崩れると、AI システムは構造的アンカーを失う。
*例:* "Current state → Goal → Problem → Solution"; "List → Manage → Activate → Recovery"

**L5 — Semantic Atom (Minimum Meaning Unit Layer)**
内部で処理される、分割不可能な最小の意味単位。
通常運用では人間には不可視。ここが腐敗すると、上位のすべての層に影響する。
*例:* Command names, role words, modality markers, intent flags, labels

#### 人間と AI の処理分担

| Layer | Primary Processor | Human Visibility | Defensive Access |
| --- | --- | --- | --- |
| L1 Category | Human | Explicit | ✅ Full |
| L2 Layer | Human | Explicit | ✅ Full |
| L3 Object | Human | Explicit | ✅ Full |
| L4 Pattern | AI | Implicit | ⚠️ Indirect only |
| L5 Atom | AI | Invisible | ❌ None currently |

**重要な洞察:** 人間は意識的に L1–L3 で動作する。AI は人間の可視性なしに L4–L5 で動作する。
効果的な人間–AI 協働には、L4–L5 処理が意図した結果を生むよう、L1–L3 での意図的な設計が必要である。

---

### Human-in-the-Point 原則

S5LA は、Human-in-the-Loop からの転換である **Human-in-the-Point** 原則に直結する。

区別:

| Model | Assumption | Cognitive Cost | Control |
| --- | --- | --- | --- |
| Human-in-the-Loop | 人間がすべてのステップをレビューする | High | High（ただし持続不可能） |
| **Human-in-the-Point** | 人間はレバレッジポイントでレビューする | Low | Targeted — 重要なところで |

**次のいずれかが真であるとき、その決定はレバレッジポイントとみなす:**

1. アクションが不可逆である（削除、送信、金融取引）
2. アクションがシステムの定義スコープ外の当事者に影響する
3. アクションがシステムの制約セットに捕捉されていない規範的判断を必要とする
4. AI 判断の確信度を独立に検証できない

Human-in-the-Point は、結果の重い決定に対する意味のある人間の権限を維持しつつ、認知負荷を下げる。
判断の頻度は減り、判断の精度は上がる。

**なぜレバレッジポイントで AI ではなく人間の判断か？**

人間社会では、説明責任を負えるのは人間だけだからである。

AI は最適化、拡張、反復を大規模に行える。しかし説明責任 — 社会的・法的・倫理的構造のなかで
決定の帰結に責任を負う能力 — は人間に属する。これは現行 AI の技術的限界ではない。
いかなる能力向上も変えない、人間社会の構造的特徴である。

したがって Human-in-the-Point は、AI が改善されるまでの一時的な回避策ではない。永続的な
アーキテクチャ原則である: AI は L4–L5（pattern と atom）で動作し、人間は L1–L3
（category, layer, object）で動作し、説明責任を委任できないポイントで判断を保持する。

SIF はこれを L1 Escalation ルールで運用化する: AI が定義された制約セット内で決定を解決できないとき、
行動にデフォルトするのではなく、人間の判断へルーティングする。

---

### セマンティック腐敗の3つの文脈

SIF は、セマンティック完全性が侵害されうる3つの文脈にわたって動作する:

| Context | Description | Primary Risk |
| --- | --- | --- |
| **Input Context** | ユーザー入力、外部ドキュメント、参照データ | 悪意ある指示; 信頼できないソースからのコンテキスト汚染 |
| **Reasoning Context** | LLM 内の内部解釈と優先順位付け | セマンティック混合; 隠れた優先度ドリフト; 暗黙パターンによる判断基準の上書き |
| **Output Context** | 最終応答、実行されたアクション、生成アーティファクト | 元の意図から乖離する不安全・不整合・不正形式の出力 |

3つの SIF 層は、各文脈で**何を保全しなければならないか**を定義する。
3つの文脈は、**セマンティック腐敗が起きうる場所**を定義する。
完全な評価は両次元を調べる: 3 layers × 3 contexts = 9 evaluation cells。

---

### Bridge Model

S5LA は、人間の意図と AI 処理のあいだの **bridge** — 翻訳層ではなく構造的コネクタ —
として理解するのが最善である。

| Bridge Function | Description |
| --- | --- |
| Load distribution | 人間は大きな意味ブロックを担い、AI は高ボリュームのマイクロ単位を担う |
| Traffic direction | Human → intent / values / judgment; AI → expansion / optimization / iteration |
| Fall prevention | 意味の誤ルーティング、層の吸収、phantom existence（機能なき表示）を防ぐ |

**崩壊パターン:** Semantic Category は正しいが Semantic Layer が誤ルーティングされ、
Semantic Object が配線を交差し、Semantic Pattern が文脈をまたいで共有され、Semantic Atoms が
可視フィールドから離れるとき — 結果は最悪の失敗モードである:
*「正しく理解したのに、それでも罰せられた。」*

---

### Open Problems: L4–L5 Attack Surface

SIF は防御境界を明示的に認める: **L4 と L5 は現時点で防御不能である。**

これは設計ギャップではない。LLM の解釈可能性と制御可能性の研究の現状を反映している。
これらの層での保護を主張することは誤解を招く。

**L4 — Pattern Exploitation:**
AI の構造テンプレートは暗黙的であり、外部から観測できない。特定モデルが構造パターンを
どう処理するかを理解した敵対者は、明示的ルールを一切破らずに、意図しない仕方でモデルの
パターン処理を通る入力を細工できる。
候補研究: L3→L4 境界で L4 パターンを推定する外部プロキシとしての
**APD / Semantic Graph Defense**（防御の主張ではない）。

**L5 — Atom-level Poisoning:**
LLM 内部で処理される最小意味単位は、現行の API 面からはアクセスできない。
ファインチューニング、RLHF poisoning、原子表現を狙うプロンプトレベル注入は、
すべて現行の防御アクセスの外側で動作する。
候補研究: L5 対応研究のプロキシとしての APD Semantic Components
（**APD Semantic Component ≠ S5LA Semantic Atom**）。

**補償制御（アーキテクチャレベル）:**

- Sandboxing: エージェント権限を必要最小スコープに制約する
- Capability restriction: デプロイ時に未使用権限を除去する
- Output monitoring: エージェント出力の行動異常検知
- Human-in-the-Point: L1–L3 準拠にかかわらず、結果の重い決定を人間判断へルーティングする

---

### 研究アジェンダ

次の領域での貢献を歓迎する:

- **Empirical validation:** 多様な LLM デプロイにわたる、SIF 準拠 vs. 非準拠システムの
  脆弱性率の定量測定
- **L4 interpretability:** 構造テンプレート内省の基盤としての transformer attention patterns の
  機構的分析; 候補の外部観測経路としての APD / Semantic Graph Defense
  （`framework/sif-v3.md` H1–H5 を参照）
- **L5 access:** 原子レベル防御アクセスへの経路としての activation steering と
  representation engineering; L5 対応研究のプロキシ（同一性ではない）としての APD Semantic Component
- **Human-in-the-Point operationalization:** 組織文脈、役割、リスク許容度にわたる閾値校正
- **SIF extension:** セマンティック腐敗がエージェント境界を越えて伝播しうる
  マルチエージェントパイプラインへの適用
- **APD connection hypotheses (H1–H5):** Graph↔L4 対応; Component↔L5 マッピング;
  Semantic Routing attacks; 言語横断的完全性; graph-constructor 敵対的ロバストネス
  （研究アジェンダのみ — SIF v3 Future work を参照）
- **APD Semantic Graph Defense integration:** Adversarial Prompt Disentanglement
  (Fang & Fang, AAAI 2026) を L3→L4 境界検査研究として — 外部 Semantic Graph 構築が
  L4 Semantic Pattern 観測の間接プロキシになりうるかを評価するプロトタイプ
  Semantic Boundary Inspector (SBI)。
  参照: https://doi.org/10.1609/aaai.v40i5.37389

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
