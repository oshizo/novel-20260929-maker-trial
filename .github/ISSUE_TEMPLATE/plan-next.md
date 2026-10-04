---
name: 次のPlanningを進める
about: 作品方針 / 確定設定と既存Planningから次のPlanning範囲を判定し、標準pipelineで進める
title: "次のPlanningを進める"
labels: ""
assignees: ""
---

## 目的

このstory repoの現在状態を読み、**今の執筆地点から次に必要な1つのPlanning範囲だけ**を判定して、`novel-maker` の標準pipelineで進める。

長編では全Arcを先に詳細化しない。`Overall → 現在Arc → そのArcのEpisode群 → 本文 → Arc境界確認 → 次Arc` のrolling方式を基本とする。

通常は1範囲につき1Episodeを扱う。ただし、物語冒頭で `opening-sequence.md` に従って主人公接続から最初の主要事件までを複数Episodeで連続設計する場合だけ、必要なEpisode群を**冒頭導入セット**として1つのPlanning範囲で確認できる。Episode数を2話へ固定しない。

## 最初に行うこと

1. repo rootの `AGENTS.md` を読む。
2. `story.yaml`、`story-direction.md`、`canon/`、`planning/`、`state/`、`manuscript/` の現在状態を確認する。
3. `.novel-maker/runtime/docs/planning-input.md`、`.novel-maker/runtime/docs/plot-planning.md`、`.novel-maker/runtime/docs/story-artifacts.md`、`.novel-maker/runtime/docs/story-craft.md`、`.novel-maker/runtime/docs/pipeline-run-trace.md` に従う。
4. 物語冒頭のArc / Episodeを扱う場合は `.novel-maker/runtime/docs/opening-sequence.md` も確認する。
5. `.codex/agents/` がある場合は、`AGENTS.md` の役割分担どおりsubagentを使う。modelの起動指定と許可するmodel fallbackは `.novel-maker/runtime/docs/codex-model-policy.md` を正本とする。configured agent / modelが起動できない場合は、その方針を適用し、許可された範囲でも起動できなければrunを `blocked` にする。親agentや `*-fallback` 等の別roleで代走しない。
6. `.novel-maker/runtime/docs/planning-changes.md` に従う。未確定の変更があれば `planning/pending-changes.md` で同じ作業の継続かを識別する。別操作では仮変更を確定済みとして使わない。作品入力が途中変更されていれば、旧計画の `inputs_revision` 等と比較して最上位の影響箇所を確認し、影響する既存計画をstaleにしてから次の範囲を選ぶ。古いreadyだけを根拠にしない。

## 次の範囲の判定

### 1. Overallが存在しない場合

`Planning Readiness → plan overall` を行う。

- 入力整理 / Planning Readinessを先に実行する。
- AIが決められる創作上の不足は作者へ質問せず進める。
- 作者側の目的そのものが不足し、runtime契約上 `needs-author-direction` になる場合だけ停止して、必要な作者入力を最小限報告する。
- `ready` ならOverallの標準pipelineへそのまま進む。

### 2. Overallが存在する場合

`planning/overall.md` のmetadataを確認する。

- `status: draft` かつ `review: pending` → machine pipelineが完了しrunが `awaiting-human` なら、既存Overallを勝手に上書きせず作者確認待ちとして停止する。machine側が未解決なら、その停止理由を報告する。同じ未確定作業の明示的な継続はplanning-changes.mdに従い、draft / pendingだけで作者待ちと決めない。
- `status: stale` → stale理由と上流変更を確認する。現在の執筆地点から必要なArc / Episodeを特定できる場合は、そのPlannerがOverallの必要箇所も仮修正して現在の対象まで作り、一式でレビューする。現在の下位範囲を決められない場合はOverallをreplanする。
- `status: ready` → OverallのArc順と現在のPlanning / 本文状態から、**現在進行中のArc**を判定する。

### 3. 現在Arcが未作成 / stale / 作者確認待ちの場合

- 対応する `planning/arcs/<arc-id>.md` が存在しない → そのArcをplanする。
- `stale` → 現在必要なEpisodeを特定できる場合は、Episode Plannerが親Arcの必要箇所とEpisodeを一式で修正・レビューしてよい。そうでなければそのArcをreplanする。
- `draft / pending` → runが `awaiting-human` なら上書きせず作者確認待ちとして停止する。machine側の未解決は理由を報告し、同じ作業の明示的な継続はplanning-changes.mdに従う。

一度に複数Arcを生成しない。

現在の計画を成立させるためのCanon・上位Planningの仮修正は、無関係な複数Arcの先行生成とは区別する。現在のPlannerが必要な上位箇所も仮修正し、現在の対象と一式で標準pipelineへ渡してよい。同じ一式の下書き作成では上位がdraft / staleでも進めるが、別操作や本文へは渡さない。

**物語の最初のArcをplanするときは、作品方針や作者指示にEpisode 0の明示指定がなくても `.novel-maker/runtime/docs/opening-sequence.md` をArc PlannerとStory Craft Challengerへの入力に含める。**

Arc PlannerはEpisode一覧を確定する前に、まず冒頭導入型を選ぶ。

- 異世界転生で、Canon / Directionに別型を選ぶ明確な理由がなければ `reincarnation-standard` を既定とする。
- 今世で一定期間生きた後に前世記憶が戻る設定なら、Canon上の理由から `memory-awakening` を選ぶ。
- `current-first-early-reveal` は現在事件から始める具体的な作品上の理由がある場合に限る。
- `current-only-exception` は `story-direction.md` に明確な作者意図がある場合だけ使う。

そのうえで、独立導入を `採用 / Episode 1へ統合 / 不採用` のどれにするか判定する。

- `採用`: 選んだ導入型に必要な主人公接続を独立Episodeで行ってから、最初の主要事件へ進む。必要なら複数Episodeへ分けてよい。
- `Episode 1へ統合`: 最初の主要事件の前半に主人公接続を組み込む。主要事件を過密にしない。
- `不採用`: 最初の主要事件そのものだけで、選んだ導入型が要求する主人公接続まで十分に成立する場合に限る。

Episode 0を全作品へ機械的に追加しない。一方、異世界転生で前世・転生・記憶覚醒・前世と今世の自己接続を「前世由来の思考癖」だけへ圧縮し、現在の小仕事だけから始めることを王道扱いしない。

`opening_sequence_pattern`、採否、短い理由はrun traceへ記録し、PR本文にも残す。第2候補以下を選ぶ場合は、その型を選ぶCanon / Direction上の理由も残す。

### 4. 現在Arcがreadyの場合

**次Arcへ進む前に、そのArcのEpisodeを具体化する。全Arcがreadyになるのを待たない。**

現在ArcのEpisode一覧を順に読み、最初にPlanningが必要なEpisodeを選ぶ。

#### 冒頭導入セットの例外

最初のArcで、複数Episodeが `opening-sequence.md` に基づく主人公接続から最初の主要事件までの連続した役割を持つ場合は、それらを**冒頭導入セット**として扱う。

- 未作成の導入Episodeが複数ある場合、このIssueではそのセットをまとめて確認できる。
- 既に一部が作成済みなら、未作成 / staleのEpisodeだけを作成・再計画するが、接続する前後Episodeを隣接Episodeとして必ず読む。
- セット内のEpisodeが作者確認待ちなら、勝手に上書きせず停止する。
- Arc Plannerが必要と判断した導入範囲を越えて、通常Episodeまで便乗して先行生成しない。
- 「通常は1Episode」の例外は、選んだ冒頭導入型を成立させるために必要なEpisode群だけに限定する。

冒頭導入セットに該当しない場合は、最初にPlanningが必要なEpisodeを1つだけ扱う。

- `planning/episodes/<episode-id>.design.md` が存在しない → そのEpisodeをplanする。
- Episode Designが `stale` → runtime契約に従ってそのEpisodeをreplanする。
- Episode Designが `draft / pending` → 作者確認checkpointなら停止する。
- Episode Designが `ready` でも執筆指示が未作成 / stale → 既存契約に従って執筆指示を生成・確認する。
- Episode Designと執筆指示が利用可能なら、次のEpisodeを確認する。

通常はこのIssueで新規に扱うEpisodeを1つだけとする。冒頭導入セットは明示例外であり、複数Episodeを一度に先行生成する一般ルールへ広げない。

### 5. 現在Arcの全Episode Planningが利用可能になった場合

**次Arcをplanしない。**

そのArcは本文執筆へ進める状態なので、「現在ArcのEpisode Planningは揃った。次はこのArcの本文執筆」と報告して停止する。

長編では、実際に本文を書いた結果として人物関係、作品の重心、後続展開を変えたくなることを正常なものとして扱う。未執筆の後続Arcを、現在Arcの本文より先に詳細化しない。

### 6. 現在Arcの本文が完了した後

次のPlanning開始時は、まずArc境界確認を行う。

- 実際の `manuscript/`、`state/`、確定設定を読む。
- Overallの大きな方向 / 読者への約束に対して、実際に書いた結果がどこまで進んだか確認する。
- 後続Arcの前提が変わった場合だけ、runtime契約に従って影響する成果物を `stale` / replan対象にする。
- 既存のreadyな後続成果物を、本文を書いたという理由だけで機械的に捨てない。
- 境界確認後、Overall上の次Arcを現在Arcとしてplanする。

## 実行pipeline

対象範囲が決まったら、`AGENTS.md` とruntime契約に定義された標準pipelineを省略せず実行する。

実際に対象Plannerを起動する直前に `.novel-maker/runtime/docs/pipeline-run-trace.md` に従って `.novel-maker/runs/<run-id>/` を開始する。Codex等のexecutorを使う場合、取得できるexecutor名・versionを `run.json` へ記録する。各stageではconfigured agent / modelとactual agent / model / resultを記録する。

**standard pipelineはfail-closedとする。** configured agent / modelが起動できない場合はmodel方針を適用する。許可されたmodel fallbackで同じroleの正式出力契約を満たした場合は、切替をrun traceへ記録して続行できる。それでも起動できない、許可されない別roleや別modelへ切り替えた、親agentが代走した、または正式出力契約を満たさない場合、そのstageとrunを `blocked` にして停止する。許可されない代走の結果をconfigured roleの正式な `PASS / WEAK / FAIL` やRegression PASSとして記録しない。

Story Craft Challengerの初回確認と改訂後再判定は、どちらも正式出力として少なくとも `読者としての感想`、`報酬トレース`、`判定`、`Planner Revisionへ渡す内容` が必要。Story Craft Regressionは `比較結果` と `判定` を返し、`PASS` 一語だけの出力は正式なRegression結果として扱わない。

run traceは診断用であり、過去runをPlanner / Writerの通常contextへ入れない。**fresh Story Craft再判定にも、初回Challenger全文、Planner Revisionの採否一覧、旧snapshot、過去runを渡さない。**

物語の最初のArcでは、Arc Plannerが返した `opening_sequence_pattern`、冒頭導入の `採用 / Episode 1へ統合 / 不採用`、短い理由を `run.json` に記録する。Story Craft Challengerがその判断を不足として覆した場合は、Planner Revision後の最終判断が追えるようにstage出力も残す。

Overall / Arc / 通常のEpisode Designでは次を行う。

```text
対象Planner 初稿
→ Story Craft Challenger（初回確認）
→ fresh Planner runで改訂
→ fresh Story Craft Challenger（改訂後再判定）
→ WEAK / FAILなら追加Planner Revision + 再判定を最大1回
→ PASSならPlot Reviewer
→ Plot Finalizer
→ Story Craft Regression
→ 必要な作者確認
```

- 初回Challengerの判定は `initial_story_craft_verdict` に記録する。
- 改訂後再判定の最後の正式判定は `revised_story_craft_verdict` に記録する。
- Regression PASSを `revised_story_craft_verdict: PASS` の代わりにしない。
- 2回目の改訂後再判定でも `PASS` でなければ `story-craft-unresolved` としてrunを `blocked` にし、Technical Reviewerへ進まない。

Overall開始時はその前にPlanning Readinessを行う。

### 冒頭導入セットのpipeline

複数Episodeを冒頭導入セットとして扱う場合も、役割分離を崩さない。

```text
Episode Planner: 導入セット先頭Episode 初稿
→ Episode Planner: 次の導入Episode 初稿（直前Episodeを隣接Episodeとして参照）
→ 必要な導入Episodeまで繰り返す
→ Story Craft Challenger: 導入セット全体を初回確認
→ fresh Episode Planner: 必要なEpisodeを改訂
→ fresh Story Craft Challenger: 改訂後の導入セット全体を独立に再判定
→ WEAK / FAILなら追加Planner Revision + 再判定を最大1回
→ PASSならPlot Reviewer: Episode間の境界・重複・因果・知識差をまとめて確認
→ Plot Finalizer: 必須修正を各Episodeへ反映
→ Story Craft Regression: 主人公接続から最初の主要事件までの意図が失われていないか確認
→ 必要な作者確認
```

Challengerには `opening-sequence.md` と導入セット内のEpisode Designを渡す。異世界転生では特に、**前世の主人公から今世の主人公へ読者が接続し、転生/記憶覚醒によって世界を見直したうえで、最初の主要事件までに作品の主要な面白さが起動するか**を確認させる。

改訂後再判定には改訂済み導入セットと現在の正本入力だけを渡し、初回Challengerの指摘を答え合わせ用に渡さない。

Episode Designは改訂後Story Craft再判定 `PASS` とStory Craft Regression `PASS` の両方を通過後、runtime契約どおり執筆指示の生成・確認へ進める。冒頭導入セットでも執筆指示はEpisodeごとに生成・確認する。

- Challenger / Reviewer / Regressionの一時出力を作品成果物へ恒久保存しない。親agentは正式出力をrun traceへ隔離保存する。
- 設定提案 / 設定提案への依存はruntime契約どおり扱う。
- 必要なCanon・上位Planningはplanning-changes.mdに従って仮修正する。変更前、変更表示、依存先と影響先を残し、対象範囲だけの局所設定で隠さない。作品方針、作者の明示指定、既存本文の事実は守る。
- Story Craft確認には変更後の一式と各階層の必要な観点を渡す。fresh再判定へ仮変更の採否理由・旧snapshot・差分・作業記録全文を渡さない。Technical Reviewerには変更前・差分・変更後・影響先を渡す。Finalizer / Regressionも一式を対象にする。
- Story Craft RegressionがFAILした場合はruntime契約の一度だけの回復手順に従い、そのattemptも同じrun traceへ記録する。Regression回復では新しいStory Craft Challengerを起動しない。

## 作者確認checkpoint

`story.yaml` の `planning.human_review.checkpoints` を正本にする。

対象範囲または仮変更した上位Planningがcheckpointなら:

- AI内部pipeline完了後も `draft / pending` で残す。
- PRを作成して作者確認待ちで停止する。
- run traceを `awaiting-human` とし、作者確認結果を後から同じrunへ関連付けられる状態にする。
- 作者承認を捏造しない。
- 確認対象の変更を一式で提示する。上位を先に確定するためだけの途中待ちを増やさず、意味を変えた上位へ旧承認を引き継がない。

checkpointでなければruntime契約に従って `ready / not-required` へ進め、run traceを `completed` にする。

仮変更がある場合は、必要な確認を終えてから一括確定する。採用した入力の `inputs_revision`、各versionと参照版、設定提案への依存を揃える。影響する既存計画だけをstaleにし、影響しない計画と本文を維持する。仮変更の一部を戻す場合は依存する計画も修正・再確認し、表示と `planning/pending-changes.md` は確定処理の最後に除く。

## Git / PR

実際にPlanning成果物を変更する場合:

1. このIssue専用branchを作る。
2. 対象範囲、必要なCanon・上位Planningの変更、未確定なら `planning/pending-changes.md`、当該実行の `.novel-maker/runs/<run-id>/` をcommitする。無関係な既存変更は含めない。
3. PRを作る。
4. PR本文に次を短く書く。
   - 自動判定した対象範囲
   - 物語の最初のArcなら、`opening_sequence_pattern`、冒頭導入の `採用 / Episode 1へ統合 / 不採用`、短い理由
   - 第2候補以下の導入型なら、その型を選んだCanon / Direction上の理由
   - 参照した親Planning version（ある場合）
   - executor名 / versionとconfigured stagesが完走したか
   - initial / revised Story Craft verdict
   - 標準pipeline完了状況
   - Story Craft Regression結果
   - run-id
   - 設定提案 / 確定設定更新の有無
   - 作者確認待ちかどうか
   - 変更file一覧

Planning成果物を変更せず停止した場合でも、Planning Readiness等の診断runを保存したなら、そのrun traceだけを残してよい。不要なPRを作る必要はない。

## 完了条件

- [ ] repo状態から次のPlanning範囲を上記規則で判定した
- [ ] Overall開始ならPlanning Readinessを通した
- [ ] readyな現在Arcがある場合、未作成の次Arcより先にそのArcのEpisodeを確認した
- [ ] 対象範囲の標準pipelineを省略していない
- [ ] 当該pipelineのrun traceを `.novel-maker/runs/<run-id>/` に残した
- [ ] executor名 / versionとconfigured / actual agent・modelを取得できる範囲で記録した
- [ ] 未解決のconfigured agent起動失敗、許可されないfallback、role出力契約欠落を通常の成功扱いにしていない
- [ ] model方針が許可するfallbackを使った場合、その理由と結果をrun traceへ記録した
- [ ] Story Craft Challenger初回確認 / 改訂後再判定 / Regressionの正式出力契約を確認した
- [ ] `initial_story_craft_verdict` と `revised_story_craft_verdict` を別々に記録した
- [ ] 改訂後再判定はfresh Challengerとして実行し、初回reviewや旧runを通常contextへ入れていない
- [ ] 改訂後再判定が `PASS` してからTechnical Reviewerへ進んだ
- [ ] `WEAK / FAIL` の追加Revision + 再判定を最大1回に制限した
- [ ] Regression PASSを改訂後Story Craft PASSと混同していない
- [ ] 物語の最初のArcなら、導入型・採否・理由をArc Planner / Challengerで確認しrun traceへ残した
- [ ] 異世界転生で第2候補以下を選んだ場合、Canon / Direction上の理由を確認した
- [ ] 作者確認待ちの既存成果物を勝手に上書きしていない
- [ ] 通常は一度に複数Arc / 複数Episodeへ進んでいない
- [ ] 冒頭導入セットを使った場合、選んだ導入型に必要なEpisodeだけをセット扱いし、通常Episodeへ広げていない
- [ ] 現在ArcのEpisode Planningが揃った段階で、本文より先に次Arcを詳細化していない
- [ ] checkpoint設定に従った
- [ ] 作品入力の途中変更とAIの仮変更を区別し、最上位の影響箇所を見落としていない
- [ ] 仮変更がある場合、変更一式・影響先をレビューし、採用前に別操作や本文へ渡していない
- [ ] 確定時にinputs_revision・参照版・依存関係を揃え、影響する既存計画だけをstaleにした
- [ ] 変更がある場合はPRを作成した
