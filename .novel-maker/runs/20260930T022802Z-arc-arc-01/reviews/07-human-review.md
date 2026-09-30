# 作者確認

- PR: https://github.com/oshizo/novel-20260929-maker-trial/pull/8#pullrequestreview-5360934947
- 確認日時: 2026-09-30T02:51:12Z
- 結果: changes-requested
- 明確な修正要求: 3件

方向性とrun trace導入は良いですが、作者確認としては修正してからmergeしたいです。

## 1. arc-01で姉妹の異性的・身体的魅力が完全に消えています

作品方針では、カイはリシェル/エルナの容姿、露出、距離を異性として自然に認識し、特にリシェルは肩・背中・腹・脚を出す旅装/舞台衣装にも慣れている、という約束があります。

一方このArcは二人との**初対面**を扱うのに、Arc全体で二人の美しさや身体的魅力をカイが認識する記述が一度もありません。Overallの「美しい姉妹との旅がもたらす異性的な近さ」の `実現するArc` がarc-02以降だからといって、arc-01で異性としての認識自体まで消す意図ではありません。

ここでは強い親密さや性的回収は不要ですが、少なくとも episode-001 の初対面で、危機の最中でもカイが二人を魅力的な成人女性として自然に認識することは入れてください。Arc固有の小さな読者報酬にするか、「後の楽しさのために先に示すこと」とするかはPlannerに任せます。

これは今後のEpisode Plannerが主人公を無性的・鈍感にしてしまうのを防ぐ意味でも重要です。

## 2. 不自然な日本語がまだ残っています

前にmaker側で問題にした種類の表現が少しあります。最低限、次は普通の日本語へ直してください。

- `魔法を必要な瞬間へ使って` → `必要な瞬間に使って` 等
- `グレンの研究との似通い` → `グレンの研究との共通点` / `グレンの研究と似ている点` 等
- `父が扱っていた感覚との似通い` → 何が似ているのかを直接書く（例: `父が研究していた魔力の流れと似た特徴`）

文字列だけ直すのではなく、同種の不自然な名詞・動詞の組み合わせがないかArc全体を一度見直してください。

## 3. 今回のHuman Review自体をrun traceへ残してください

これはrun trace導入後の最初の実運用なので、今回のレビューを観測データとして捨てたくありません。

既存run `20260930T022802Z-arc-arc-01` へHuman Reviewを追加し、`human_review_status: changes-requested` と修正要求件数を更新してください。そのうえで、このHuman指摘を反映するArcのreplanは契約どおり**別run**として開始してください。元runのmachine PASSを後から書き換えないでください。

これにより今回から、`machine Regression PASS → Humanが追加問題を発見 → replan` という、maker改善に特に欲しかったデータがそのまま残ります。

## run trace自体の確認

ここは良好です。

- 初稿 / Planner Revision後 / Finalizer後の全文snapshotあり
- Challenger / Planner採否 / Technical Review / Finalizer / Regressionの正式出力あり
- `run.json` の PASS、採用1件、Technical必須修正1件、Regression PASS が保存内容と一致
- framework revisionも maker #121 merge commit `8090fbf...` を記録
- Arc Plannerも `実現するArc` と長期進展を分離した新版になっている

なお `run.json` の各stageの `model` がすべて `null` なのは観測上やや惜しいです。.codex/agents側には設定modelが分かるので、actual modelが取得不能なら将来的には `configured_model` と `actual_model` を分ける等をmaker側で検討したいです。これは今回のArc内容のblockerとはしません。
