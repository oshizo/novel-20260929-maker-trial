# 作品リポジトリ

このrepositoryは `novel-maker` で作る1作品のstory repoです。利用するframework revisionの正本は `story.yaml` です。

- 共通の実行規約は `.novel-maker/runtime/` を読む。作品固有ruleとして直接編集しない。
- 文章と言葉の規約は `.novel-maker/runtime/docs/language-policy.md` を読む。説明文は原則として自然で平易な日本語で書き、不要な英語ラベルや比喩的な造語を増やさない。
- Planning開始前は `.novel-maker/runtime/docs/planning-input.md` を読む。
- Plot操作は `.novel-maker/runtime/docs/plot-planning.md`、成果物の責務は `.novel-maker/runtime/docs/story-artifacts.md` を読む。
- 作品固有の正本は `story-direction.md`、`canon/`、`planning/`、`style/`、`state/`、`manuscript/` に置く。
- repository内のtextはUTF-8として扱い、shellの既定encodingへ依存しない。
- 作者確認は作品として採用するかを作者が判断する工程である。技術的な不整合はReviewer / Finalizer側で解消する。

## Planning開始

作品方針 / 確定設定を用意した後の定型Planning入口は `.github/ISSUE_TEMPLATE/plan-next.md` とする。

- `次のPlanningを進める` Issueを受け取ったら、Issue本文の判定規則に従い、repoの現状態から次の1範囲を決める。
- Overallが未作成ならPlanning Readinessから開始し、readyならOverall生成へ進む。
- 長編はrolling planningを基本とし、readyな現在Arcがある場合は未作成の次Arcより先に、そのArcの未作成 / stale Episodeを1つだけ扱う。
- 現在ArcのEpisode Planningが揃ったら、本文より先に後続Arcを詳細化せず、本文執筆へ進める状態だと報告して停止する。
- Arc境界では `manuscript/`、`state/`、確定設定を確認し、実際に書いた結果の影響がある後続成果物だけをstale / replan対象にする。
- `draft / pending` の作者確認待ち成果物を勝手に上書きしない。
- 範囲判定後のPlanner / Challenger / Reviewer / Finalizer / Regressionの詳細はruntime契約を正本とし、Issue本文へ書かれていない工程も省略しない。

作品固有ruleが必要なら、このfileへ追加する。


## Codexでの役割分担

`.codex/agents/` が存在する場合、CodexはPlot Planningの役割を対応するagentへ委譲する。

- Planning Readiness: `planning_readiness`
- Overall Planner: `overall_planner`
- Arc Planner: `arc_planner`
- Episode Planner: `episode_planner`
- Story Craft Challenger: `story_craft_challenger`
- Plot Reviewer: `plot_reviewer`
- Plot Finalizer: `plot_finalizer`
- Story Craft Regression: `story_craft_regression`
- 執筆指示生成: `writer_brief_generator`
- 執筆指示確認: `writer_brief_reviewer`

Overall / Arc / Episode Designは原則として次の順で処理する。

```text
Planning Readiness（Overall開始時）
        ↓
対象範囲のPlannerが初稿を作る
        ↓
story_craft_challenger
        ↓
同じ範囲のPlannerが改訂する
        ↓
plot_reviewer
        ↓
plot_finalizer
        ↓
story_craft_regression
        ↓
作者確認
```

親agentは処理順の管理を担当し、Planner / Challenger / Reviewer / Finalizer / Regressionを自分で兼任しない。

Plannerの改訂は、初稿を作った会話へ指摘を継ぎ足すのではなく、必要な入力を明示した新しいrunとして始める。Challengerの指摘を `採用 / 一部採用 / 不採用` に分け、採用した改善意図だけを親agentへ返す。

Challengerの指摘全文、Plannerの採否理由、採用した改善意図、Technical Reviewerの指摘、Story Craft Regressionの指摘は、その実行中だけ受け渡す。story成果物へreview logとして保存しない。

`plot_reviewer` には改訂済みPlot、変更してはいけない上位条件、技術上の規約、採用した改善意図を渡す。`plot_finalizer` は必須修正を解消しながら、採用した改善意図を可能な限り保つ。`story_craft_regression` はTechnical Review前の改訂済みPlotと最終Plotを比較し、新しい改善案を追加しない。

Story Craft RegressionがFAILした場合は、Finalizerによる最小復元、復元差分だけのTechnical Reviewer再確認、必要なら必須修正だけの再適用を行い、Regressionを1回だけ再実行する。2回目もFAILなら対象を `draft` のまま停止し、作者確認へ進めない。

Episode DesignはStory Craft Regression通過後にだけ、次の順で執筆指示へ進める。

```text
writer_brief_generator
        ↓
writer_brief_reviewer / 執筆指示へ渡す情報の境界を確認
        ↓
必要なら作者確認
```

作者確認の境界は `story.yaml` に従う。技術上の不備や未解決のStory Craft Regressionを作者の好みとして判断委譲しない。

`.codex/agents/` はbootstrap時点のCodex用設定の写しであり、`.novel-maker/runtime/` の管理対象ではない。`framework-sync` で暗黙更新しない。
