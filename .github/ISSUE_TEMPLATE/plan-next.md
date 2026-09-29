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

通常は1範囲につき1Episodeを扱う。ただし、現在Arcの先頭に `episode-000` と `episode-001` が並び、主人公導入と作品起動を連続して設計する場合だけ、`opening-sequence.md` に従って**冒頭導入セット**を1つのPlanning範囲として扱える。

## 最初に行うこと

1. repo rootの `AGENTS.md` を読む。
2. `story.yaml`、`story-direction.md`、`canon/`、`planning/`、`state/`、`manuscript/` の現在状態を確認する。
3. `.novel-maker/runtime/docs/planning-input.md`、`.novel-maker/runtime/docs/plot-planning.md`、`.novel-maker/runtime/docs/story-artifacts.md`、`.novel-maker/runtime/docs/story-craft.md` に従う。
4. 物語冒頭のArc / Episodeを扱う場合は `.novel-maker/runtime/docs/opening-sequence.md` も確認する。
5. `.codex/agents/` がある場合は、`AGENTS.md` の役割分担どおりsubagentを使う。親agentがPlanner / Reviewer等を兼任しない。

## 次の範囲の判定

### 1. Overallが存在しない場合

`Planning Readiness → plan overall` を行う。

- 入力整理 / Planning Readinessを先に実行する。
- AIが決められる創作上の不足は作者へ質問せず進める。
- 作者側の目的そのものが不足し、runtime契約上 `needs-author-direction` になる場合だけ停止して、必要な作者入力を最小限報告する。
- `ready` ならOverallの標準pipelineへそのまま進む。

### 2. Overallが存在する場合

`planning/overall.md` のmetadataを確認する。

- `status: draft` かつ `review: pending` → 既存Overallを勝手にreplan・上書きせず、作者確認待ちとして停止する。
- `status: stale` → stale理由と上流変更を確認し、runtime契約に従ってOverallをreplanする。
- `status: ready` → OverallのArc順と現在のPlanning / 本文状態から、**現在進行中のArc**を判定する。

### 3. 現在Arcが未作成 / stale / 作者確認待ちの場合

- 対応する `planning/arcs/<arc-id>.md` が存在しない → そのArcをplanする。
- `stale` → runtime契約に従ってそのArcをreplanする。
- `draft / pending` → 上書きせず作者確認待ちとして停止する。

一度に複数Arcを生成しない。

物語の最初のArcをplanするとき、作品方針または現在の作者指示が主人公導入を主要事件の前に独立して置くことを求めている場合は `opening-sequence.md` をArc Plannerへの入力に含める。導入セットを採用するなら、Episode一覧の先頭を `episode-000`（主人公導入）→ `episode-001`（最初の主要事件・出会い・作品起動）として、2話の接続が分かるようにする。Episode 0を全作品へ機械的に追加しない。

### 4. 現在Arcがreadyの場合

**次Arcへ進む前に、そのArcのEpisodeを具体化する。全Arcがreadyになるのを待たない。**

現在ArcのEpisode一覧を順に読み、最初にPlanningが必要なEpisodeを選ぶ。

#### 冒頭導入セットの例外

Episode一覧の先頭が `episode-000` と `episode-001` で、両者が主人公導入→作品起動の連続した役割を持つ場合は、`opening-sequence.md` に従う。

- 0と1がともに未作成なら、このIssueでは2話をまとめて**冒頭導入セット**として扱う。
- 片方だけ未作成 / staleなら、その1話だけを作成・再計画するが、もう片方を隣接Episodeとして必ず読み、0→1の接続を確認する。
- 0または1が作者確認待ちなら、勝手に上書きせず停止する。
- 冒頭導入セットを理由に `episode-002` 以降までまとめて先行生成しない。

冒頭導入セットに該当しない場合は、最初にPlanningが必要なEpisodeを1つだけ扱う。

- `planning/episodes/<episode-id>.design.md` が存在しない → そのEpisodeをplanする。
- Episode Designが `stale` → runtime契約に従ってそのEpisodeをreplanする。
- Episode Designが `draft / pending` → 作者確認checkpointなら停止する。
- Episode Designが `ready` でも執筆指示が未作成 / stale → 既存契約に従って執筆指示を生成・確認する。
- Episode Designと執筆指示が利用可能なら、次のEpisodeを確認する。

通常はこのIssueで新規に扱うEpisodeを1つだけとする。**Episode 0と1の冒頭導入セットだけを明示例外**とし、複数Episodeを一度に先行生成する一般ルールへ広げない。

### 5. 現在Arcの全Episode Planningが利用可能になった場合

**次Arcをplanしない。**

そのArcは本文執筆へ進める状態なので、「現在ArcのEpisode Planningは揃った。次はこのArcの本文執筆」と報告して停止する。

長編では、実際に本文を書いた結果として人物関係、作品の重心、後続展開を変えたくなることを正常なものとして扱う。未執筆の後続Arcを、現在Arcの本文より先に詳細化しない。

### 6. 現在Arcの本文が完了した後

次のPlanning開始時は、まずArc境界確認を行う。

- 実際の `manuscript/`、`state/`、確定した設定を読む。
- Overallの大きな方向 / 読者への約束に対して、実際に書いた結果がどこまで進んだか確認する。
- 後続Arcの前提が変わった場合だけ、runtime契約に従って影響する成果物を `stale` / replan対象にする。
- 既存のreadyな後続成果物を、本文を書いたという理由だけで機械的に捨てない。
- 境界確認後、Overall上の次Arcを現在Arcとしてplanする。

## 実行pipeline

対象範囲が決まったら、`AGENTS.md` とruntime契約に定義された標準pipelineを省略せず実行する。

Overall / Arc / 通常のEpisode Designでは概ね次を行う。

```text
対象Planner 初稿
→ Story Craft Challenger
→ fresh Planner runで改訂
→ Plot Reviewer
→ Plot Finalizer
→ Story Craft Regression
→ 必要な作者確認
```

Overall開始時はその前にPlanning Readinessを行う。

### 冒頭導入セットのpipeline

`episode-000` と `episode-001` を冒頭導入セットとして扱う場合も、役割分離を崩さない。

```text
Episode Planner: episode-000 初稿
→ Episode Planner: episode-001 初稿（episode-000を隣接Episodeとして参照）
→ Story Craft Challenger: 0と1をセットで確認
→ fresh Episode Planner: 必要なEpisodeを改訂
→ Plot Reviewer: 0と1の境界・重複・因果・知識差をまとめて確認
→ Plot Finalizer: 必須修正を各Episodeへ反映
→ Story Craft Regression: 0→1の導入意図が失われていないか確認
→ 必要な作者確認
```

Challengerには `opening-sequence.md` と2つのEpisode Designを渡し、0だけの派手さではなく、**0で主人公へ接続し、1までに作品の主要な面白さが起動するか**を確認させる。

Episode DesignはStory Craft Regression通過後、runtime契約どおり執筆指示の生成・確認へ進める。冒頭導入セットでも執筆指示はEpisodeごとに生成・確認する。

- Challenger / Reviewer / Regressionの一時出力をstory成果物へ恒久保存しない。
- 設定提案 / 設定提案への依存はruntime契約どおり扱う。
- 上位Planningの意味を勝手に変更しない。上位変更が必要なら対象範囲の生成で隠さず、停止理由を報告する。
- Story Craft RegressionがFAILした場合はruntime契約の一度だけの回復手順に従う。

## 作者確認checkpoint

`story.yaml` の `planning.human_review.checkpoints` を正本にする。

対象範囲がcheckpointなら:

- AI内部pipeline完了後も `draft / pending` で残す。
- PRを作成して作者確認待ちで停止する。
- 作者承認を捏造しない。

checkpointでなければruntime契約に従って `ready / not-required` へ進める。

## Git / PR

実際にPlanning成果物を変更する場合:

1. このIssue専用branchを作る。
2. 対象範囲と必要最小限の確定設定更新だけをcommitする。
3. PRを作る。
4. PR本文に次を短く書く。
   - 自動判定した対象範囲
   - 冒頭導入セットを使ったかどうか
   - 参照した親Planning version（ある場合）
   - 標準pipeline完了状況
   - Story Craft Regression結果
   - 設定提案 / 確定設定更新の有無
   - 作者確認待ちかどうか
   - 変更file一覧

Planning成果物を変更せず停止した場合は、不要なbranch / PRを作らない。

## 完了条件

- [ ] repo状態から次のPlanning範囲を上記規則で判定した
- [ ] Overall開始ならPlanning Readinessを通した
- [ ] readyな現在Arcがある場合、未作成の次Arcより先にそのArcのEpisodeを確認した
- [ ] 対象範囲の標準pipelineを省略していない
- [ ] 作者確認待ちの既存成果物を勝手に上書きしていない
- [ ] 通常は一度に複数Arc / 複数Episodeへ進んでいない
- [ ] 冒頭導入セットを使った場合、Episode 0と1だけをセット扱いし、2以降へ広げていない
- [ ] 現在ArcのEpisode Planningが揃った段階で、本文より先に次Arcを詳細化していない
- [ ] checkpoint設定に従った
- [ ] 変更がある場合はPRを作成した
