# Planning pipeline 実行履歴

状態: Plot Planningの診断用実行履歴を、作品成果物と分離して保存するための正本

本書は [`plot-planning.md`](plot-planning.md) の標準pipelineを観測するための規約である。作品方針、確定設定、Planning、State、本文などの作品上の正本を置き換えない。

## 1. 目的

Plot Planningでは、初稿から最終成果物までに複数のagentが確認と修正を行う。

```text
Planner 初稿
→ Story Craft Challenger（初回確認）
→ Planner Revision
→ Story Craft Challenger（改訂後再判定）
→ Technical Reviewer
→ Finalizer
→ Story Craft Regression
→ 必要な作者確認
```

改訂後再判定が `WEAK / FAIL` の場合は、追加のPlanner Revisionとfresh Story Craft再判定を最大1回だけ行う。再判定が `PASS` するまでTechnical Reviewerへ進まない。

最終成果物だけを残すと、後から次を確認できない。

- 初回Challengerが何を問題視し、`PASS / WEAK / FAIL` のどれを返したか。
- Planner Revisionが何を採用、一部採用、不採用にしたか。
- 改訂後の成果物をfresh Challengerが独立にどう再判定したか。
- 初稿、各Revision後、Finalizer後で何が変わったか。
- Technical Reviewerが何件の必須修正を出したか。
- Story Craft Regressionが何を確認してPASS / FAILしたか。
- machine側がPASSした後に作者がどんな修正を求めたか。
- framework revision、executor version、configured agent / model、実際に動いたagent / modelの違いで結果がどう変わったか。

特に、**Story Craft Regressionの `PASS` と、改訂後Story Craft再判定の `PASS` は別の意味を持つ。** RegressionはTechnical Review / Finalizerによる回帰がないことを確認するstageであり、改訂済みPlotそのものを対象読者として新規評価するstageではない。

これらは `novel-maker` 自体を改善するための重要な観測情報である。一方、通常のPlannerやWriterへ渡すとcontextを汚染する。

そのため、**作品成果物には中間レビューを残さず、診断用実行履歴だけを `.novel-maker/runs/` へ隔離して保存する。**

## 2. 保存場所

1つの `plan` / `replan` 対象を、原則として1つのrunとして扱う。

`planning-changes.md` に従ってCanon・上位Planningを仮修正する場合も、現在の対象と変更一式を同じrunで記録する。保存先が上位という理由で別runを作り、改訂回数をリセットしない。

```text
.novel-maker/runs/<run-id>/
  run.json
  snapshots/
    01-planner-initial.md
    03-planner-revision.md
    05-planner-revision-attempt-2.md     # 追加Revisionを行った場合
    08-finalizer.md
  reviews/
    00-planning-readiness.md             # Overallで実行した場合
    02-story-craft-challenger.md
    04-story-craft-recheck.md
    05-planner-revision-attempt-2.md     # 追加Revisionを行った場合
    06-story-craft-recheck-attempt-2.md  # 追加再判定を行った場合
    07-technical-review.md
    09-story-craft-regression.md
    10-human-review.md                   # 後から追加してよい
```

番号は例であり、実際の処理順に合わせてよい。意味の正本は `run.json > stages` とする。

Story Craft再判定の追加attempt、Story Craft Regression失敗時の回復、Planning Readinessの再実行など、同じstageを複数回実行した場合は `-attempt-2` などを付けて別fileにする。

例:

```text
reviews/04-story-craft-recheck-attempt-1.md
reviews/06-story-craft-recheck-attempt-2.md
reviews/09-story-craft-regression-attempt-1.md
snapshots/10-finalizer-recovery.md
reviews/11-technical-review-recovery.md
reviews/12-story-craft-regression-attempt-2.md
```

## 3. run-id

run-idは一意であればよい。作品の意味を持たせない。

推奨形式:

```text
<UTC日時>-<target-kind>-<target-id>
```

例:

```text
20260930T020500Z-overall-overall
20260930T031200Z-arc-arc-01
20260930T041000Z-episode-episode-003
```

同じIDがすでに存在する場合は末尾へ連番を付ける。

## 4. 保存する情報

### 4.1 snapshot

次の時点の対象Planning fileを、その時点の内容のまま保存する。

- Planner初稿後。
- 各Planner Revision後。追加Revisionを行った場合も別snapshotを残す。
- Finalizer後。
- Regression失敗時の回復でFinalizerが再修正した場合は、その回復後。

snapshotは差分や要約ではなく、**その時点の対象成果物全体**を保存する。これにより後から任意の2時点を比較できる。

冒頭導入セットなど複数fileを1範囲として扱う場合は、対象fileごとにsnapshotを分け、`run.json` の同じstageから複数pathを参照する。

仮変更がある場合は、現在の対象だけでなく、変更したCanon・上位Planningと `planning/pending-changes.md` も各時点で保存する。作業開始版のcommit・path、未commit入力のsnapshotを最初に保持し、確定後は表示を除いた最終fileと採否・既存計画への影響判定を残す。作業記録を削除する前に最後の記録を保存する。モデルの内部思考は保存しない。

### 4.2 review

次のagentが親agentへ返した**正式な出力**を保存する。

- Planning Readiness。
- Story Craft Challengerの初回確認。
- Planner Revisionの採否結果と採用した改善意図。
- Story Craft Challengerの改訂後再判定。追加再判定もattemptごとに保存する。
- Technical Reviewer。
- Story Craft Regression。
- Writer Brief Reviewerを同じ操作内で実行した場合はその確認結果。
- 作者確認結果を後から記録する場合はHuman Review。

要約だけに置き換えず、agentが親へ返した正式出力を残す。

ただし、モデルの内部思考、chain-of-thought、system prompt、認証情報、実行環境の秘密情報は保存しない。Planner Revisionで保存する理由も、親へ正式に返した短い採否理由だけでよく、内部推論を要求しない。

**改訂後再判定のreview fileを保存することと、それを次のfresh Challengerへ入力することは別である。** 再判定agentへは旧review、旧snapshot、過去runを通常contextとして渡さない。

### 4.3 executor / agent / model情報

run開始時に、実行に使うexecutorを取得できる範囲で記録する。

- `executor.name`: 例 `codex`。
- `executor.version`: 例 `codex-cli 0.159.3`。CLIで取得できる場合は `codex --version` 等の実測値を使う。

各stageには、設定上期待したroleと実際に動いたroleを分けて記録する。

- `configured_agent`: stageで起動する予定だったagent名。
- `configured_model`: そのstageで起動する予定のmodel名。executor方針による明示overrideがある場合は、適用後の値を記録する。指定しない場合は `null`。
- `actual_agent`: 実際に起動したagent名。起動前に失敗した場合は `null`。
- `actual_model`: 実際に確認できたmodel名。確認できない場合は `null`。
- `reasoning`: reasoning設定など比較に必要な公開設定。
- `result`: `completed / blocked / failed`。
- `failure_reason`: 正常完了なら `null`。起動失敗や契約違反なら短い理由。

framework共通契約は特定providerのmodel名を必須にしない。standard pipelineでは、executor方針が許可しない別role・別modelへ自動fallbackして成功扱いにしてはならない。

Codexのprimary modelと許可するmodel fallbackは [`codex-model-policy.md`](codex-model-policy.md) に従う。許可されたmodel fallbackで同じroleの正式出力契約を満たした場合は標準stageとして扱い、同文書に従って `model_fallback` とsummaryの `model_fallback_count` を記録する。model切替を理由にStory Craft判定や改訂回数をリセットしない。

actual modelをexecutorから確実に取得できない場合、推測してconfigured modelをコピーしない。`null` のままでもよい。ただしagent起動自体が成功したかは必ず判定する。

## 5. `run.json`

`run.json` は複数story repo / trialを横断して集計できる機械可読の索引とする。

初期templateは `.novel-maker/runtime/templates/story/run-trace/run.json` を参照する。

最低限、次を持つ。

```json
{
  "schema_version": 1,
  "run_id": "20260930T020500Z-overall-overall",
  "target": {
    "kind": "overall",
    "id": "overall",
    "path": "planning/overall.md"
  },
  "framework_revision": "<40文字commit SHA>",
  "executor": {
    "name": "codex",
    "version": "codex-cli 0.159.3"
  },
  "status": "awaiting-human",
  "started_at": "2026-09-30T02:05:00Z",
  "completed_at": null,
  "summary": {
    "initial_story_craft_verdict": "WEAK",
    "revised_story_craft_verdict": "PASS",
    "story_craft_recheck_attempt_count": 1,
    "planner_revision_count": 1,
    "planner_adopted_count": 2,
    "planner_partially_adopted_count": 1,
    "planner_rejected_count": 1,
    "technical_required_fix_count": 3,
    "regression_verdict": "PASS",
    "regression_attempt_count": 1,
    "recovery_count": 0,
    "human_review_status": "pending",
    "human_required_change_count": null
  },
  "stages": []
}
```

### 5.1 target

`kind` は少なくとも `overall`, `arc`, `episode` を使える。冒頭導入セットなど複数fileを1範囲とする場合は `kind: episode-set` とし、`paths` を追加してよい。

上位・Canonの仮変更がある場合は、`kind / id / path` は現在の対象を保ち、`paths` に変更一式のfileを追加してよい。stageの `snapshots` から各fileの時点別内容を辿れるようにする。新しいstageや状態値は増やさない。

### 5.2 status

run全体の状態には次を使う。

- `running`: AI内部pipelineの途中。
- `awaiting-human`: machine pipelineは完了し、作者確認待ち。
- `completed`: 必要な作者確認も含めて完了。
- `blocked`: 契約上これ以上進めない状態で停止。configured agent起動失敗、正式出力契約の欠落、Story Craft再判定の未解決を含む。
- `failed`: 実行自体が異常終了し、正規の停止記録まで作れなかった。

### 5.3 summary

集計用の値を置く。

- `initial_story_craft_verdict`: 最初のChallenger判定。`PASS / WEAK / FAIL / null`。
- `revised_story_craft_verdict`: Planner Revision後のfresh Story Craft再判定の**最後の正式判定**。`PASS / WEAK / FAIL / null`。Regression判定をここへ転記しない。
- `story_craft_recheck_attempt_count`: 改訂後再判定を実行した回数。標準pipelineでは1、追加Revisionを使った場合は最大2。
- `planner_revision_count`: Planner Revisionを実行した回数。標準pipelineでは最低1、追加Revisionを使った場合は最大2。
- `planner_adopted_count`: Planner Revisionが `採用` としたChallenger指摘件数。複数Revisionがある場合は正式出力から一意に集計できるときだけ合計する。
- `planner_partially_adopted_count`: `一部採用` とした件数。
- `planner_rejected_count`: `不採用` とした件数。
- `technical_required_fix_count`: 最初のTechnical Reviewで明確に数えられる必須修正件数。数えられない形式なら `null`。
- `regression_verdict`: 最後の**正式な**Story Craft Regression判定。Regressionが起動失敗または出力契約違反なら `null` のままとする。
- `regression_attempt_count`: Regressionの実行回数。
- `recovery_count`: Regression失敗後のbounded recoveryを実行した回数。
- `human_review_status`: `not-required / pending / approved / changes-requested / not-recorded`。
- `human_required_change_count`: 作者が明確に修正要求した項目数。数えられない場合や未確認なら `null`。

数字を推測して埋めない。正式出力から一意に数えられない場合は `null` とする。

`initial_story_craft_verdict: WEAK` かつ `regression_verdict: PASS` でも、`revised_story_craft_verdict` が `PASS` でなければ「最終的にStory CraftがPASSした」と扱わない。

### 5.4 stages

各stageを実行順に並べる。

初回Challengerの例:

```json
{
  "sequence": 2,
  "stage": "story-craft-challenger",
  "attempt": 1,
  "configured_agent": "story_craft_challenger",
  "configured_model": "gpt-6-sol",
  "actual_agent": "story_craft_challenger",
  "actual_model": "gpt-6-sol",
  "reasoning": "high",
  "result": "completed",
  "failure_reason": null,
  "verdict": "WEAK",
  "snapshots": [],
  "review": "reviews/02-story-craft-challenger.md"
}
```

改訂後再判定の例:

```json
{
  "sequence": 4,
  "stage": "story-craft-recheck",
  "attempt": 1,
  "configured_agent": "story_craft_challenger",
  "configured_model": "gpt-6-sol",
  "actual_agent": "story_craft_challenger",
  "actual_model": "gpt-6-sol",
  "reasoning": "high",
  "result": "completed",
  "failure_reason": null,
  "verdict": "PASS",
  "snapshots": [],
  "review": "reviews/04-story-craft-recheck.md"
}
```

configured agentが起動前に失敗した例:

```json
{
  "sequence": 2,
  "stage": "story-craft-challenger",
  "attempt": 1,
  "configured_agent": "story_craft_challenger",
  "configured_model": "gpt-6-sol",
  "actual_agent": null,
  "actual_model": null,
  "reasoning": "high",
  "result": "blocked",
  "failure_reason": "configured modelをこのexecutorで起動できなかった",
  "verdict": null,
  "snapshots": [],
  "review": null
}
```

`stage` は少なくとも次を使える。

- `planning-readiness`
- `planner-initial`
- `story-craft-challenger`
- `planner-revision`
- `story-craft-recheck`
- `technical-review`
- `finalizer`
- `story-craft-regression`
- `writer-brief-generator`
- `writer-brief-reviewer`
- `human-review`

追加Revisionや再判定、回復処理でも同じstage名を使い、`attempt` と処理順で区別する。

### 5.5 standard pipelineのfail-closed

standard pipelineではconfigured role contractを満たした実行だけを正規stageとして扱う。

次の場合、そのstageを `completed` にせずrunを `blocked` にする。

- 許可されたmodel fallbackを含めてもconfigured agentが起動できない。
- configured modelが起動前に失敗し、executor方針で許可されたmodel fallbackでも解決できない。
- 親agentがconfigured subagentを使わず自分で代行した。
- `*-fallback` 等の別roleへ自動切替した。
- executor方針で許可されないmodel fallbackを行った。
- role固有の正式出力契約を満たさない。
- 改訂後Story Craft再判定が、許可された追加Revision後も `WEAK / FAIL` のまま解消しない。

executor方針で許可されないfallbackを使って診断を続けたい場合は、standard pipelineとは別の実験として明示する。その結果をconfigured roleの `PASS / WEAK / FAIL`、Technical Review完了、Regression PASSとして記録しない。

role固有の出力契約について、少なくとも次を確認する。

- Story Craft Challengerの初回確認 / 改訂後再判定: `読者としての感想`、`報酬トレース`、`判定`、`Planner Revisionへ渡す内容` がある。再判定も同じ正式出力契約を満たす。
- Story Craft Regression: `比較結果` と `判定` があり、判定は `PASS / FAIL` のどちらかである。`PASS` 一語だけは正式出力としない。

## 6. 親agentの記録手順

標準Plot pipelineを管理する親agentが記録責任を持つ。Planner / Challenger / Reviewer / Finalizer / Writerへ過去runの読み書きを担当させない。

1. 対象範囲を決定し、実際にPlannerを起動する直前にrun directoryと `run.json` を作る。executor名とversionを取得できる場合はこの時点で記録する。
2. stage開始前にconfigured agent / modelを `run.json > stages` へ記録する。
3. configured agentを起動する。model起動失敗時はexecutor方針が許可するmodel fallbackを適用し、切替理由と結果を記録する。許可された範囲で起動できなければstageを `blocked`、runを `blocked` として停止する。別roleや親agentで代行しない。
4. agentから正式出力を受け取ったらrole固有の出力契約を確認する。不足していればstage / runを `blocked` として停止する。
5. 正式出力を受け取った直後にreview fileへ保存する。
6. Planner初稿、各Planner Revision、Finalizerが対象成果物を書き換えた直後にsnapshotを取る。
7. 初回Challenger終了時に `initial_story_craft_verdict` を更新する。改訂後再判定の各attempt終了時に `story_craft_recheck_attempt_count` と `revised_story_craft_verdict` を更新する。Regression結果で `revised_story_craft_verdict` を上書きしない。
8. 最初の改訂後再判定が `WEAK / FAIL` なら、追加Planner Revisionと再判定を最大1回だけ行う。2回目も `PASS` でなければrunを `blocked` にする。
9. `revised_story_craft_verdict: PASS` 後にTechnical Reviewer / Finalizer / Regressionへ進む。
10. Regressionが正式な `PASS` を返し、作者確認が必要なら `awaiting-human` とする。不要なら `completed` とする。
    仮変更がある場合は、変更した上位も含めた確認対象を一式で提示する。確認不要または承認後に `planning-changes.md` の確定処理を終えてから `completed` とする。未確定のCanonや古い参照版を残して完了にしない。
11. Regression未解決、Readinessの停止条件などで進めない場合は `blocked` とし、停止理由をstageへ残す。
12. 後日作者確認が行われた場合、同じrunへHuman Reviewを追加し、`human_review_status` とrun `status` を更新する。

親agentが途中で異常終了しても、そこまでのfileは残す。次回実行で未完runを見つけても、通常Planningの入力として再利用しない。診断または明示的な再開指示がある場合だけ参照する。

## 7. 通常contextからの隔離

`.novel-maker/runs/` は診断情報であり、作品上の正本ではない。

通常のPlanning / Writer実行では、次を守る。

- 過去runを検索・参照してPlot内容を決めない。
- Planner、Challenger、Reviewer、Finalizer、Writerへ過去runを入力しない。
- Canon / Planning / State / Style / Manuscriptの代わりにrun traceを参照しない。
- `check-story` やframework-syncは、`.novel-maker/runs/` をruntime snapshotやPlanning成果物として解釈しない。

Story Craft改訂後再判定でもこの隔離は維持する。fresh Challengerへは、**現在の改訂済みPlotと現在の正本入力だけ**を渡し、初回Challenger全文、Planner Revisionの採否一覧、旧snapshot、過去runを渡さない。

同じ作業内の仮修正は、`planning-changes.md` に従って変更後の案を入力する。Technical ReviewerやRegressionへ必要な比較資料は親が今回の対象として明示して渡す。agentに過去runの自由探索を許すことにはしない。未確定の変更の継続は作業記録で識別し、別操作で診断snapshotを作品正本の代用にしない。

過去runを読むのは次の場合だけとする。

- `novel-maker` 自体の改善分析。
- model / prompt / framework revisionの明示的な比較実験。
- 作者または開発者が明示的に実行履歴の診断を求めた場合。
- Human reviewとmachine reviewの差を分析する場合。

この隔離規則は、run traceをGitで長期保存する場合でも変えない。

## 8. Human Review

作者確認は作品内容の正本を決める工程であり、run trace自体を作者にレビューさせる工程ではない。

作者が承認した場合:

- `reviews/10-human-review.md` 等に承認した事実を短く残してよい。
- `human_review_status: approved` にする。
- runを `completed` にする。

作者が修正を求めた場合:

- 指摘の全文または意味を失わない短い記録をHuman Reviewへ残す。
- 明確に数えられる修正要求がある場合は `human_required_change_count` へ件数を入れる。
- `human_review_status: changes-requested` にする。
- その指摘によって新しい `replan` を始める場合は**別run**にする。
- 元runを書き換えて「最初からmachineが検出していた」ことにはしない。

Human Reviewから得た作品固有の作者判断を次回以降も守る場合は、maker改善時にDirection / Canon / Planning等の適切な正本へ反映してよい。診断ログそのものを次のPlannerへ渡さない。

## 9. Gitと保持期間

初期運用では `.novel-maker/runs/` をGit管理する。run traceは作品成果物ではないが、maker改善の診断材料としてrepositoryへ残す。

大量化してrepositoryサイズが問題になった場合は、内容を削る前に外部artifact store等への移管を別途設計する。初期段階では観測データを先に失わない。
