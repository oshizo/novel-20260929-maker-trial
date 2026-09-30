# Planning pipeline 実行履歴

状態: Plot Planningの診断用実行履歴を、作品成果物と分離して保存するための正本

本書は [`plot-planning.md`](plot-planning.md) の標準pipelineを観測するための規約である。作品方針、確定設定、Planning、State、本文などの作品上の正本を置き換えない。

## 1. 目的

Plot Planningでは、初稿から最終成果物までに複数のagentが確認と修正を行う。

```text
Planner 初稿
→ Story Craft Challenger
→ Planner Revision
→ Technical Reviewer
→ Finalizer
→ Story Craft Regression
→ 必要な作者確認
```

最終成果物だけを残すと、後から次を確認できない。

- Challengerが何を問題視したか。
- Planner Revisionが何を採用、一部採用、不採用にしたか。
- 初稿、Revision後、Finalizer後で何が変わったか。
- Technical Reviewerが何件の必須修正を出したか。
- Story Craft Regressionが何を確認してPASS / FAILしたか。
- machine側がPASSした後に作者がどんな修正を求めたか。
- framework revision、agent、modelの違いで結果がどう変わったか。

これらは `novel-maker` 自体を改善するための重要な観測情報である。一方、通常のPlannerやWriterへ渡すとcontextを汚染する。

そのため、**作品成果物には中間レビューを残さず、診断用実行履歴だけを `.novel-maker/runs/` へ隔離して保存する。**

## 2. 保存場所

1つの `plan` / `replan` 対象を、原則として1つのrunとして扱う。

```text
.novel-maker/runs/<run-id>/
  run.json
  snapshots/
    01-planner-initial.md
    03-planner-revision.md
    05-finalizer.md
  reviews/
    00-planning-readiness.md        # Overallで実行した場合
    02-story-craft-challenger.md
    03-planner-revision.md
    04-technical-review.md
    06-story-craft-regression.md
    07-human-review.md              # 後から追加してよい
```

Story Craft Regression失敗時の回復やPlanning Readinessの再実行など、同じstageを複数回実行した場合は `-attempt-2` などを付けて別fileにする。

例:

```text
reviews/06-story-craft-regression-attempt-1.md
snapshots/07-finalizer-recovery.md
reviews/08-technical-review-recovery.md
reviews/09-story-craft-regression-attempt-2.md
```

番号は処理順を読むための補助であり、意味の正本は `run.json > stages` とする。

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
- Planner Revision後。
- Finalizer後。
- Regression失敗時の回復でFinalizerが再修正した場合は、その回復後。

snapshotは差分や要約ではなく、**その時点の対象成果物全体**を保存する。これにより後から任意の2時点を比較できる。

冒頭導入セットなど複数fileを1範囲として扱う場合は、対象fileごとにsnapshotを分け、`run.json` の同じstageから複数pathを参照する。

### 4.2 review

次のagentが親agentへ返した**正式な出力**を保存する。

- Planning Readiness。
- Story Craft Challenger。
- Planner Revisionの採否結果と採用した改善意図。
- Technical Reviewer。
- Story Craft Regression。
- Writer Brief Reviewerを同じ操作内で実行した場合はその確認結果。
- 作者確認結果を後から記録する場合はHuman Review。

要約だけに置き換えず、agentが親へ返した正式出力を残す。

ただし、モデルの内部思考、chain-of-thought、system prompt、認証情報、実行環境の秘密情報は保存しない。Planner Revisionで保存する理由も、親へ正式に返した短い採否理由だけでよく、内部推論を要求しない。

### 4.3 agent / model情報

実行環境から取得できる場合は、各stageに次を記録する。

- agent名。
- model名。
- reasoning設定など比較に必要な公開設定。

framework共通契約は特定providerのmodel名を必須にしない。取得できない場合は `null` でよい。

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
  "status": "awaiting-human",
  "started_at": "2026-09-30T02:05:00Z",
  "completed_at": null,
  "summary": {
    "initial_story_craft_verdict": "WEAK",
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

### 5.2 status

run全体の状態には次を使う。

- `running`: AI内部pipelineの途中。
- `awaiting-human`: machine pipelineは完了し、作者確認待ち。
- `completed`: 必要な作者確認も含めて完了。
- `blocked`: 契約上これ以上進めない状態で停止。
- `failed`: 実行自体が失敗し、正規の停止判定まで到達しなかった。

### 5.3 summary

集計用の値を置く。

- `initial_story_craft_verdict`: 最初のChallenger判定。`PASS / WEAK / FAIL / null`。
- `planner_revision_count`: Planner Revisionを実行した回数。
- `planner_adopted_count`: Planner Revisionが `採用` としたChallenger指摘件数。
- `planner_partially_adopted_count`: `一部採用` とした件数。
- `planner_rejected_count`: `不採用` とした件数。
- `technical_required_fix_count`: 最初のTechnical Reviewで明確に数えられる必須修正件数。数えられない形式なら `null`。
- `regression_verdict`: 最後のStory Craft Regression判定。
- `regression_attempt_count`: Regressionの実行回数。
- `recovery_count`: Regression失敗後のbounded recoveryを実行した回数。
- `human_review_status`: `not-required / pending / approved / changes-requested / not-recorded`。
- `human_required_change_count`: 作者が明確に修正要求した項目数。数えられない場合や未確認なら `null`。

数字を推測して埋めない。正式出力から一意に数えられない場合は `null` とする。

この集計値により、たとえば次を後から比較できる。

- Challengerの提案がどの程度Plannerに採用されるか。
- framework更新後にTechnical Reviewerの必須修正件数が減ったか。
- machine側がPASSした後にHumanだけが発見する問題がどの程度残るか。
- modelやpromptの変更でRevision回数やRegression失敗率が変わったか。

### 5.4 stages

各stageを実行順に並べる。

例:

```json
{
  "sequence": 2,
  "stage": "story-craft-challenger",
  "attempt": 1,
  "agent": "story_craft_challenger",
  "model": "gpt-5.6",
  "result": "completed",
  "verdict": "WEAK",
  "snapshots": [],
  "review": "reviews/02-story-craft-challenger.md"
}
```

`stage` は少なくとも次を使える。

- `planning-readiness`
- `planner-initial`
- `story-craft-challenger`
- `planner-revision`
- `technical-review`
- `finalizer`
- `story-craft-regression`
- `writer-brief-generator`
- `writer-brief-reviewer`
- `human-review`

回復処理でも同じstage名を使い、`attempt` と処理順で区別してよい。

## 6. 親agentの記録手順

標準Plot pipelineを管理する親agentが記録責任を持つ。Planner / Challenger / Reviewer / Finalizer / Writerへ過去runの読み書きを担当させない。

1. 対象範囲を決定し、実際にPlannerを起動する直前にrun directoryと `run.json` を作る。
2. 各agentから正式出力を受け取った直後にreview fileへ保存する。
3. Planner初稿、Planner Revision、Finalizerが対象成果物を書き換えた直後にsnapshotを取る。
4. `run.json > stages` と集計可能な `summary` を、そのstage終了時点で更新する。
5. RegressionがPASSして作者確認が必要なら `awaiting-human` とする。不要なら `completed` とする。
6. Regression未解決、Readinessの停止条件などで進めない場合は `blocked` とし、停止理由をstageの正式出力へ残す。
7. 後日作者確認が行われた場合、同じrunへHuman Reviewを追加し、`human_review_status` とrun `status` を更新する。

親agentが途中で異常終了しても、そこまでのfileは残す。次回実行で未完runを見つけても、通常Planningの入力として再利用しない。診断または明示的な再開指示がある場合だけ参照する。

## 7. 通常contextからの隔離

`.novel-maker/runs/` は診断情報であり、作品上の正本ではない。

通常のPlanning / Writer実行では、次を守る。

- 過去runを検索・参照してPlot内容を決めない。
- Planner、Challenger、Reviewer、Finalizer、Writerへ過去runを入力しない。
- Canon / Planning / State / Style / Manuscriptの代わりにrun traceを参照しない。
- `check-story` やframework-syncは、`.novel-maker/runs/` をruntime snapshotやPlanning成果物として解釈しない。

過去runを読むのは次の場合だけとする。

- `novel-maker` 自体の改善分析。
- model / prompt / framework revisionの比較実験。
- 作者または開発者が明示的に実行履歴の診断を求めた場合。
- Human reviewとmachine reviewの差を分析する場合。

この隔離規則は、run traceをGitで長期保存する場合でも変えない。

## 8. Human Review

作者確認は作品内容の正本を決める工程であり、run trace自体を作者にレビューさせる工程ではない。

作者が承認した場合:

- `reviews/07-human-review.md` 等に承認した事実を短く残してよい。
- `human_review_status: approved` にする。
- runを `completed` にする。

作者が修正を求めた場合:

- 指摘の全文または意味を失わない短い記録をHuman Reviewへ残す。
- 明確に数えられる修正要求がある場合は `human_required_change_count` へ件数を入れる。
- `human_review_status: changes-requested` にする。
- その指摘によって新しい `replan` を始める場合は**別run**にする。
- 元runを書き換えて「最初からmachineが検出していた」ことにはしない。

この差分は、machine PASS後に人間だけが発見した問題をmaker側で分析するために残す。

## 9. Gitと保持期間

初期運用では `.novel-maker/runs/` をGit管理する。run traceは作品成果物ではないが、trial間比較とmaker改善の再現性のためrepositoryへ残す。

大量化してrepositoryサイズが問題になった場合は、内容を削る前に外部artifact store等への移管を別途設計する。初期段階では観測データを先に失わない。
