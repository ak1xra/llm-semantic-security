## Part III — AI 愛好家向け

### 平易な言葉で見る問題

あなたも経験したことがあるはずです。

AI に詳細な指示を書く。言葉は正しい。論理も筋が通っている。
AI はそれを読み、受け止める — そして、頼んだことに隣接した何かをする。

完全に間違いというわけではない。しかし、意図したことでもない。

これは AI が「バカ」だからではない。**セマンティック失敗（semantic failure）** である:
言葉は届いたが、その背後の意図は届かなかった。あなたが意図したことと AI が処理したことの
あいだで、何かが失われた。

これには明確な理由がある。ほとんどの AI 指示は意味レベルではなく語レベルで書かれる。
AI は、それらのコマンドに意味を与える重み・優先度・構造を受け取らずに、コマンドだけを受け取る。

SIF と S5LA は意味レベルで設計するためのフレームワークである — より良いプロンプトを書くだけでなく、
意図が実際に旅を生き延びられるシステムを構築するためのものだ。

---

### 5分プロンプト監査

書いたシステムプロンプトや AI 指示を用意する。次の3問を順に適用する。
失敗を見つけたらすぐ止まる — それが優先修正箇所である。

#### Step 1: Identity check (L3)

- この AI が何であるかを、ちょうど一文で説明できるか？
- プロンプトに、AI が*決して*してはならないことの明示リストがあるか？
- プロンプトに、AI が*してもよい*ことの明示リストがあるか？

いずれかが「いいえ」→ まずアイデンティティ節を書き直す。それ以外はすべて暫定である。

#### Step 2: Weight check (L2)

- 制約（「AI は常に…しなければならない」）は、ガイドライン
  （「AI は…を考慮すべき」）と別セクションになっているか？
- AI に参照ドキュメントを渡した場合、それが*参照*でありルールではないことが明確か？

制約とガイドラインが同じ段落に混在している → 分離する。混在した段落は、
AI が制約を任意として扱う許可証である。

#### Step 3: Judgment check (L1)

- 指示どうしが競合しうるとき、どちらが勝つかのルールがあるか？
- AI が自分で決めず、止まってあなたに尋ねるべき条件はあるか？

競合解決ルールがない → AI は自分のデフォルトで決める。それはあなたの意図と一致しないかもしれない。

---

### Before / After: 実際の失敗の書き換え

**失敗（Notion AI, January 2026）:**

AI エージェントに実行すべき計画が与えられた。後続のすべてのターンで、与えられた計画を
実行する代わりに、新しい改善された計画を生成した。Planning score: 95/100。Execution: 0/100。

#### Before: the original prompt structure

```text
You are a helpful AI assistant. Help the user achieve their goals.
Follow the user's plan. Produce high-quality results.
```

AI が受け取ったもの: 曖昧なアイデンティティ、階層なし、
「この計画を実行せよ」と「この計画を最適化せよ」の区別なし。

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

変わったこと: AI にアイデンティティ（executor、optimizer ではない）、階層
（plan = immutable、reference ではない）、判断ルール（改変前にフラグ）ができた。

---

### 専門用語なしの Human-in-the-Point

AI との働き方は2つある。

**Human-in-the-Loop:** すべてのステップを承認する。常にその場にいる。
AI はサインオフなしに動かない。安全だが消耗する — そして AI を持つ目的を損なう。

**Human-in-the-Point:** 本当に重要な瞬間だけ承認する。
AI が定型を扱い、あなたが不可逆なものを扱う。

どの瞬間が重要か、どう知るか？ 4つのシグナル:

1. **取り消せない。** メール送信、ファイル削除、支払い — 取り返しがつかないなら、
   先に人間が承認すべきである。
2. **外の誰かに影響する。** アクションがシステムの壁を越えて現実世界に届くなら、
   人間がレビューすべきである。
3. **価値観の判断が要る。** 「正解」がない決定もある — 何が重要かについての判断が要る。
   それは人間の仕事である。
4. **AI が確信していない。** AI の確信度を検証できないなら、一人で決めさせない。

それ以外？ AI に走らせる。あなたの仕事は、すべてのステップではなく、レバレッジポイントにいることだ。

---

### 貢献する方法

このフレームワークは生きた文書である。強化する貢献を歓迎する。

**貢献の仕方:**

- **新しいケーススタディ:** LLM システムでセマンティック失敗パターンを観測したか？
  SIF 診断形式で文書化し、pull request を提出する。
- **チェックリストの改善:** 診断チェックリストで偽陽性や偽陰性を経験したか？
  シナリオと提案する調整を添えて issue を開く。
- **L4/L5 研究:** LLM interpretability や activation steering に取り組んでいるか？
  上記の研究アジェンダと接続する。
- **翻訳:** S5LA の日本語概要は存在する。他言語も歓迎する。
- **適用ドメイン:** SIF は Notion AI とエンタープライズエージェントに対して開発された。
  他ドメイン（healthcare AI, legal AI, autonomous systems）への適用が必要である。

このリポジトリで issue または pull request を開く。

---

*© 2026 Akira Hayakawa / 3BPS. All rights reserved.*
