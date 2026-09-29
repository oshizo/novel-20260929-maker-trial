# Plot Planning 契約 v1

状態: Plot成果物と計画手順の正本

repositoryの所有境界は [`repository-contract.md`](repository-contract.md)、計画開始前の入力整理は [`planning-input.md`](planning-input.md)、読者向けの面白さを確認する観点は [`story-craft.md`](story-craft.md)、文章と言葉は [`language-policy.md`](language-policy.md) に従う。

本書と [`../templates/story/planning/`](../templates/story/planning/) はframework側の正本であり、実作品のPlotはstory repo側を正本とする。

## 1. 目的と原則

計画段階では作者側の設計意図を保持する。一方、本文執筆担当へは、本文に必要な観測可能情報と、指定された視点人物が直接知覚・意識できる情報だけを渡す。

成果物は次の順に具体化する。

```text
作品方針 / 確定設定 / 現在の作者指示
                    ↓
                 Overall
                    ↓
                   Arc
                    ↓
             Episode Design
                    ↓
                 執筆指示
                    ↓
               本文執筆担当
```

- Overallは作品全体の入口、最終出口、大きな因果と変化を持つ。遠い区間は粗くてよい。
- Arcは複数Episodeを束ねる区間の入口、出口、役割、主要な進展を持つ。
- Episode Designは本文執筆直前まで具体化する。
- 場面構成はEpisode Designに含め、独立した場面計画を増やさない。
- 執筆指示はEpisode Designを本文執筆担当向けに変換した成果物であり、作者側の設計理由をそのまま渡さない。
- 一時的な確認結果は原則として恒久fileへ保存せず、採用結果だけをPlotへ反映する。

### 長編ではrolling planningを基本とする

長編で上の階層をすべて詳細化してから本文へ進むことを必須にしない。標準運用は、**現在のArcを十分に具体化したら、そのArcのEpisodeと本文へ進み、実際に書いた結果を観測してから次Arcを具体化する**rolling方式とする。

```text
Overall（全体地図。遠いArcは粗くてよい）
        ↓
現在Arc
        ↓
現在ArcのEpisode Design / 執筆指示
        ↓
本文
        ↓
Arc境界でState / 確定設定 / Overallとの整合を確認
        ↓
必要な後続だけreplan
        ↓
次Arc
```

- `ready` な現在Arcがあるなら、後続Arcをすべて `ready` にする前にそのArcのEpisodeへ進んでよい。
- 現在ArcのEpisode Planningが揃ったら、本文より先に未執筆の後続Arcを詳細化することを標準にはしない。
- Overallは全体方向と主要因果を保持するが、遠いArcの事件詳細は `provisional / open` のままでもよい。
- 本文で確定した行動、関係、State、確定設定を次Arcの入力へ戻す。書いた結果として後続Plotを変えたくなることは異常ではなく、通常の `replan` として扱う。
- Arc境界で後続成果物への影響を意味で確認し、影響するものだけ `stale` にする。本文を書いたという理由だけで既存のreadyな後続成果物を機械的に捨てない。
- 作者が明示的に全Arc先行を望む場合は禁止しないが、自動の次範囲判定はrolling方式を既定とする。

## 2. 配置と共通項目

story repoでは次を標準配置とする。

```text
planning/
  overall.md
  arcs/
    <arc-id>.md
  episodes/
    <episode-id>.design.md
    <episode-id>.writer.md
```

各成果物は安定したIDを持つ。題名や表示順が変わっても同じ成果物には同じIDを使い、別物へ同じIDを使い回さない。

Markdown先頭のYAMLには最低限、次を持つ。

```yaml
---
id: episode-001
kind: episode-design
version: 1
status: draft
review: pending
parent: arc-01
parent_version: 1
---
```

| 項目 | 意味 |
|---|---|
| `kind` | `overall-arc`, `arc-plot`, `episode-design`, `writer-brief` |
| `version` | その成果物の意味が変わる改訂で増える整数 |
| `status` | `draft`, `ready`, `stale` |
| `review` | `pending`, `approved`, `not-required` |
| `parent` / `parent_version` | 直接の上位成果物と、参照した版。Overallでは不要 |
| `source` / `source_version` | 執筆指示が参照したEpisode Designとその版 |
| `stale_reason` | `stale` にした理由と再計画範囲 |

### versionの扱い

`version` はGit履歴でも、計画工程の通過回数でもない。

- 新規成果物は、同じ `plan` の中で初稿、面白さ確認後の改訂、技術確認、最終修正を経ても `version: 1` のままとする。
- 既存成果物を `plan` / `replan` する場合は、実行開始時と最終結果を比較する。
- 意味が変わった場合だけ、**1回の実行につき最大1回だけ +1** する。
- 中間の修正ごとにversionを増やさない。
- 意味が変わらなければversionを維持する。

下流の `parent_version` / `source_version` と `stale` 判定は、この実行単位で確定したversionを基準にする。

`review` は作者確認の状態だけを表す。AI内部の技術確認やStory Craft確認の状態を別項目として増やさない。

## 3. 各成果物の責務

| 成果物 | 主な責務 | 入れないもの |
|---|---|---|
| Overall | 作品の入口と最終出口、中心対立、長期変化、Arc順序、主要な仕込みと回収 | 場面詳細、完成台詞、本文file分割 |
| Arc | Arcの入口と出口、不可逆な変化、主要な進展、Episode一覧、仕込みと回収、後へ残す事項 | 本文演出、Episode Designの重複説明 |
| Episode Design | Episodeの役割、入口と出口、視点、人物の目的と圧力、場面順、情報と感情の変化、作者側の設計意図 | 読者向け本文、本文執筆担当への直接指示書 |
| 執筆指示 | 本文執筆担当が本文へ変換する観測可能情報、視点人物の具体的内部状態、知識差、必要な継続条件 | 主題、作者側の意味づけ、人物評価、読ませ方、視点人物から見えない本心 |

テンプレートの空欄を埋めるためだけに同じ内容を言い換えて増やさない。情報がなければ不要な行を削るか、必要に応じて `なし` とする。

## 4. 状態と下流の無効化

- `draft`: 作成・確認・確定設定反映の途中。本文生成の正本にしない。
- `ready`: 必要な確認が終わり、下流が利用できる。
- `stale`: 上流変更で前提が古くなった。再計画材料として残すが本文生成へ使わない。

上流が変わったとき、下流を機械的に全部捨てない。直接の子ごとに意味上の影響を確認する。

- 影響がある子は `stale` にする。
- 影響がない `ready` な子は、互換性を確認したうえで `parent_version` だけを新しい親versionへ合わせる。子自身の意味が変わらないため、子の `version` は増やさない。
- 執筆指示の `source_version` が変わる場合は一旦 `stale` にし、§7の情報境界確認を再実行する。内容が変わらなければ参照元versionだけを合わせ、内容が変わるなら執筆指示自身のversionも増やす。

既存ManuscriptはPlot変更だけを理由に書き換えない。過去本文まで変える必要がある場合は、本文改訂または確定設定変更として別の実行にする。

## 5. `plan` の標準手順

Overall開始時は、先に `planning-input.md` に従って入力整理とPlanning Readinessを完了する。

各Plotは次の順で作る。

```text
入力整理 / Planning Readiness
        ↓
Planner 初稿
        ↓
Story Craft Challenger
        ↓
同じ範囲のPlannerが改訂
        ↓
Technical Reviewer
        ↓
Finalizer
        ↓
Story Craft Regression
        ↓
作者確認
        ↓
ready化と下流影響判定
```

1. **入力と変更禁止条件を確定する**  
   作品方針、上位Plot、関連する確定設定 / State、現在の作者指示を読む。OverallではPlanning Readinessも通す。

2. **Plannerが初稿を作る**  
   対象範囲のPlannerが `draft` を作る。初稿時点から、その範囲に許可されたStory Craft観点を使う。

3. **Story Craft Challengerが面白さを確認する**  
   別agentが、読者向けの面白さについて、残す点、改善候補、仮説を返す。技術的な不整合確認を兼任しない。

4. **同じ範囲のPlannerが改訂する**  
   初稿を作った会話を継ぎ足すのではなく、新しい実行として改訂する。Challengerの提案を全部採用せず、採用・一部採用・不採用を判断する。採用した改善意図だけを当該実行内で親agentへ返す。この中間工程ではversionを進めない。

5. **Technical Reviewerが技術上の問題を確認する**  
   作品方針、上位Plot、確定設定、因果、視点、知識差、人物の主体性、継続性、仕込みと回収、状態管理を確認する。Story Craftの新しい改善案は出さない。

6. **Finalizerが必要な修正を閉じる**  
   必須修正を優先し、必要なら置換・統合・削除で直す。説明を足すだけで済ませない。採用した改善意図は、上位条件や技術的正しさと両立する限り保つ。

7. **Story Craft Regressionで改善意図の消失だけを確認する**  
   Technical Review前の改訂版とFinalizer後を比較し、採用した改善意図が弱くなっていないかだけを見る。ここで新しいアイデアを追加しない。

8. **必要な作者確認を行う**  
   `story.yaml` で設定された確認地点があれば、作者側の採否を確認する。技術上の未解決問題を作者の好みとして免除しない。

9. **ready化と下流影響判定を行う**  
   必要な確認をすべて通過したら `ready` にする。上流変更時は§4に従って影響する下流だけを `stale` にする。

作者確認待ちでは `draft / pending` で止める。作者確認不要なら `review: not-required`、採用されたら `review: approved` とする。

### 当該実行の中だけで渡す情報

次はstory repoの恒久成果物へ保存しない。

- Story Craft Challengerの指摘全文
- Plannerが各指摘を採用・不採用にした理由
- 採用したStory Craft上の改善意図
- Technical Reviewerの一時的な指摘
- Story Craft Regressionの一時的な指摘

Plotへ残すのは採用後の結果だけとする。

### Story Craft Regression失敗時の一度だけの回復

Story Craft RegressionがFAILした場合だけ、次を最大1回行う。

```text
失われた改善意図を特定
        ↓
Finalizerが最小限だけ復元
        ↓
Technical Reviewerが復元差分だけを再確認
        ↓
必要ならFinalizerが必須修正だけを再適用
        ↓
Story Craft Regressionを1回だけ再実行
```

- PlannerやStory Craft Challengerを再起動しない。
- 新しい改善案を追加しない。
- 2回目もFAILなら `craft-regression-unresolved` として `draft` のまま停止する。
- 未解決のsystem側問題を作者へ判断委譲しない。

### 範囲ごとの入出力

| 実行 | 主な入力 | 主な出力 | 開始条件 |
|---|---|---|---|
| `plan overall` | 作品方針、確定設定、現在の作者指示、既存Overall。必要ならStateと主要Manuscript結果 | `planning/overall.md`、必要な確定設定更新 | 入力整理とPlanning Readinessが完了し `ready` |
| `plan arc <id>` | `ready` なOverall、関連する確定設定 / State、隣接Arc、既存対象 | `planning/arcs/<id>.md`、必要な確定設定更新 | 親Overallが `ready` |
| `plan episode <id>` | `ready` なOverall / 親Arc、関連する確定設定 / State、直近本文、既存対象 | Episode Design、必要な確定設定更新、執筆指示 | 親Arcが `ready` で執筆入口のStateが特定できる |

Episodeでは、Story Craft Regressionを通過するまで執筆指示を作らない。

```text
Episode Planner 初稿
→ Story Craft Challenger
→ Episode Planner 改訂
→ Technical Reviewer
→ Finalizer
→ Story Craft Regression
→ 執筆指示生成
→ 執筆指示確認
→ 必要なら作者確認
```

Story Craft上の指摘や「読者をこう感じさせる」という設計理由を執筆指示へ直接渡さない。

## 6. 設定提案と依存関係

Plot作成中に新設定を提案してよい。ただし、確定した設定をPlot内へ重複定義し続けない。

`設定提案` の `確定状況` は次を使う。

| 値 | 意味 |
|---|---|
| `open` | 未解決。遠方計画では保持できるが、依存する下流を `ready` にする前に解決する |
| `promoted` | 確定設定の正本へ反映済み |
| `plot-local` | 当該Plot範囲だけの将来案として保持 |
| `rejected` | 不採用。依存記述を削除または修正 |
| `replaced` | 別案へ置換済み |

### `ready` 前の依存確認

Overall / Arc / Episode Designを `ready` にする前に、対象自身と、実際に読んだ上位Planning成果物の**全設定提案**を候補として確認する。

各提案について、その提案が存在しない、または未確定なら、対象の入口、出口、主要因果、場面成立、本文執筆担当へ渡る事実のどれかが変わるかを見る。

- 変わるなら `設定提案への依存` に列挙する。
- 変わらないなら依存行を作らない。
- **確定状況と依存の有無は別に判定する**。`promoted` / `plot-local` 等へ解決済みでも、対象の確定因果が依存するなら行を残す。
- 確認した候補集合と依存表を**双方向に照合**する。
- Finalizerは各候補の確認結果を残す。必要な行が欠けていれば、解決済みの提案でも `BLOCKED` とする。
- 依存がなければ、全候補を確認した後で `なし` と記す。

執筆指示は `設定提案への依存` を持たず、依存表を転記・新設しない。元のEpisode Designが `ready` で、その依存関係が解決済みであることを確認してから執筆指示を作る。

### #49 CP-03 依存表完全性の回帰fixture

過去実験のfixtureは `experiments/` に保存する。本runtime文書では次の回帰条件だけを保持する。

- 解決済みでも実際に依存する設定提案は依存表から省略しない。
- 依存しない提案を過剰列挙しない。
- CP-03のような必要行の欠落は `BLOCKED` とする。
- fixtureそのものを成功扱いにするため書き換えない。

## 7. 執筆指示へ渡す情報の境界

Episode Designは作者側の意味・解釈・設計理由を持ってよい。執筆指示では、それらを本文執筆担当が必要とする具体情報へ変換する。

執筆指示へ残せるのは次の4種類である。

1. **本文で観測できる事実**  
   人物、場所、物、状態、行動、発話内容、時刻、入口・出口など。

2. **視点人物が直接意識する具体的な内部状態**  
   感情、欲求、意図、記憶、疑念、推測、判断。作者による人物の総評は含めない。

3. **知識差と開示時期**  
   誰が何を知っているか、何が未確認か、いつ情報差が変わるか。

4. **継続上の必須条件**  
   視点、時間順、確定設定 / State、既定の因果、人物の主体性、Episode境界を壊さないために必要な条件。

執筆指示へ入れないもの:

- 主題や作者側の設計理由
- 読者にどう感じてほしいかという指示
- 人物をどう評価すべきかという説明
- 場面の役割という作者向けラベル
- 視点人物から見えない本心・真の目的の抽象説明
- 台詞、立ち位置、細かな手順まで固定する疑似脚本

### 完成済み台詞の扱い

Episode Designに完成済みの台詞があっても、その**文言そのもの**を固定する必要がなければ執筆指示へコピーしない。

文言を固定してよいのは、たとえば次の場合である。

- 合言葉や特定呼称など、文言そのものが作中事実である。
- 過去本文や確定設定との継続上、同じ文言を維持する必要がある。
- 現在の作者指示で台詞そのものが明示的に固定されている。

それ以外は「何を伝えるか」「どんな発話をするか」だけへ粗くする。具体的な語尾、言い回し、間、割り込み、前後の応酬は本文執筆担当へ任せる。

これは鉤括弧等を禁止する単純な文字列規則ではない。意味で判定する。

## 8. Technical ReviewerとFinalizer

Technical ReviewerはStory Craft Challengerの代わりに新しい面白さ改善案を出さない。

主な確認対象:

- 作品方針 / 確定設定 / 親Plot / 現在の作者指示への違反
- 因果
- 視点と知識差
- 人物の主体性
- 継続性
- 仕込みと回収
- 設定提案の扱い
- version / status / reviewの整合
- 人間が読んで意味を追える日本語

必須修正は、根拠、問題箇所、解消条件、壊してはいけない既存要素を明示する。

Finalizerは指摘ごとに文章を足すのではなく、必要なら既存記述を置換・統合・削除する。技術上不要な細部や、同じ意味の重複を増やさない。

`ready` にする前に、少なくとも次を確認する。

1. 上位入力へ違反していない。
2. 必須修正が解消している。
3. 修正による新しい矛盾がない。
4. 同じ判断を複数の節で言い換えていない。
5. 執筆指示なら§7を通過している。
6. 設定提案の依存関係が完全である。
7. Story Craft RegressionがPASSしている。
8. 必要な作者確認が終わっている。

## 9. 作者確認

作者確認の境界は `story.yaml` で設定する。

```yaml
planning:
  human_review:
    checkpoints:
      - overall
      - arc
```

有効な確認地点は `overall`, `arc`, `episode`。空ならPlotの作者確認を必須にしない。実行時の明示指定で当該実行だけ変更してよい。

作者確認は作者側の採否・好みを確認する工程であり、技術的な不整合診断を作者へ要求する工程ではない。

## 10. `replan`

作者の方向修正や上位変更で既存Plotを直す場合も、確認手順を省略しない。

変更が必要な**最上位の成果物**から標準手順を再実行する。

```text
Planner 改訂初稿
→ Story Craft Challenger
→ Planner 改訂
→ Technical Reviewer
→ Finalizer
→ Story Craft Regression
→ 必要な作者確認
```

その後、直接の子ごとに影響を判定する。

- 意味上影響する子は `stale` にして再計画する。
- 影響しない `ready` な子は互換性を確認して親versionだけ合わせる。
- 無関係なArcやEpisodeまで作り直さない。
- 執筆指示は元のEpisode Designが変わったら一旦 `stale` にし、§7を再確認する。
- 執筆済みManuscriptは未来のPlotへ合わせて暗黙に書き換えない。

## 11. `write-next` の開始条件

本文生成前に次を満たす。

- 対象の執筆指示が `ready`。
- 元のEpisode Designが `ready` で `source_version` が一致する。
- Episode DesignがTechnical Reviewer / FinalizerとStory Craft Regressionを通過済み。
- 親Arc / Overallが `stale` ではない。
- 執筆指示に影響する設定提案が解決済み。
- 設定された作者確認が完了している。

本文執筆担当へ渡す情報は、readyな執筆指示、選択されたStyle資料、必要な直近本文に限定する。Episode Design、作品方針、Story Craftの一時指摘、Technical Reviewerの一時指摘を本文執筆担当へ直接渡さない。

## 12. 文章と用語

本書、Plot成果物、確認結果の説明文は [`language-policy.md`](language-policy.md) に従う。

- 原則として自然な日本語で書く。
- machine-readableなfield値、agent名、file名等は変更しない。
- 一般的な日本語で説明できる概念を英語ラベルへ置き換えない。
- 比喩的な造語や、その文書だけで通じる略語を増やさない。
- 「誰が、何を観察・判断・実行し、その結果どうなるか」を直接書く。
