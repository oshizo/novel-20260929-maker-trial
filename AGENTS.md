# 作品リポジトリ

このrepositoryは `novel-maker` で作る1作品のstory repoです。利用するframework revisionの正本は `story.yaml` です。

- 共通の実行規約は `.novel-maker/runtime/` を読む。作品固有ruleとして直接編集しない。
- 文章と言葉の規約は `.novel-maker/runtime/docs/language-policy.md` を読む。説明文は原則として自然で平易な日本語で書き、不要な英語ラベルや比喩的な造語を増やさない。
- Planning開始前は `.novel-maker/runtime/docs/planning-input.md` を読む。
- Plot操作は `.novel-maker/runtime/docs/plot-planning.md`、成果物の責務は `.novel-maker/runtime/docs/story-artifacts.md` を読む。
- Planning中の仮修正と作品入力の途中変更は `.novel-maker/runtime/docs/planning-changes.md` を読む。未確定の変更は `planning/pending-changes.md` で識別し、同じ作業内の下書き作成だけに使う。
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
- 作品入力の意味が途中変更されていれば、旧計画が参照した入力との互換性を確認してから範囲を選ぶ。古いreadyや参照版だけを根拠にしない。
- 下位Plannerは必要な上位・確定設定を仮修正して現在の案まで作れる。既存レビューと必要な作者確認へ一式を渡し、採用後に影響する既存計画だけをstaleにする。
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

Overall / Arc / Episode Designの工程順、Story Craftの再判定、回復条件、執筆指示へ進む条件は、pin済みの `.novel-maker/runtime/docs/plot-planning.md` の標準手順を正本とする。ここへ工程順を複製せず、そのrevisionの手順に従う。

親agentは処理順の管理を担当し、Planner / Challenger / Reviewer / Finalizer / Regressionを自分で兼任しない。

各roleへ渡す入力と正式出力の条件もpin済みruntimeに従う。診断用に保存した過去runを、Planner / Writerの通常入力へ追加しない。

作者確認の境界は `story.yaml` に従う。技術上の不備や未解決の内部確認を作者の好みとして判断委譲しない。

`.codex/agents/` はbootstrap時点のCodex用設定の写しであり、`.novel-maker/runtime/` の管理対象ではない。`framework-sync` で暗黙更新しない。


## Codexのmodel起動方針

primary model、reasoning、許可するmodel fallback、切替の記録は、pin済みの `.novel-maker/runtime/docs/codex-model-policy.md` を正本とする。既存agent TOMLのmodel値だけから起動方針を決めない。
