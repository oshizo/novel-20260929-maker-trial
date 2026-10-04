**Technical Review判定：必須修正11件。うち重複整理は2件（9・10）です。** 以下はすべて、既存の物語内容を保ってFinalizerで解消できます。新しいStory Craft案や作者判断は必要ありません。

1. **作者確認のmetadata** — [Episode002 Design](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:6) の `review: pending` は、作者確認地点がOverallとArcだけという `story.yaml` と合いません。`not-required` に直してください。新規 `version: 1`、`parent_version: 8`、`inputs_revision` は整合しています。

2. **親Arcの成果物参照** — [Arc01](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:238) のEpisode002成果物は「未作成」のままです。一式の確定時に、実在するDesignと執筆指示への参照へ更新してください。物語内容の改訂やArcの意味上の版上げは不要です。

3. **入口・出口の境界** — [Episode002 Design](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:16) の入口は、場面1で起きる「負傷者を見つける」まで先取りしています。Episode001の出口から続く、発見直前の状況にそろえてください。出口は場面5で暫定同行を選んで移動を始める境界が追える形にしてください。役割欄の状態説明も、場面順や人物変化の再説明にならないよう整理してください。

4. **魔力知覚の回数** — 親ArcのEpisode002は「一度の集中した観察」を指定していますが、[場面2](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:179) で知覚を切った後、[場面3](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:201) で再び知覚しています。知覚を用いる場面を親Arcと一致させ、頭痛・視野狭窄を踏まえて切り上げる判断を保ってください。敵の人数と位置は通常の観察とエルナの報告で把握する条件も維持します。

5. **姉妹が疑う根拠** — [場面5](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:242) は、姉妹がカイの「武器付与を止めて身体強化へ移る」様子を見たとします。しかし場面3・4には、その切替が起き、姉妹から見える記述がありません。親Arcで決まっている切替と姉妹の観察を戦闘場面に接続し、父の訓練の記憶から**疑う**ところまでを追えるようにしてください。姉妹がカイの体内魔力を知覚したことにはしません。

6. **追手三人の位置と退路** — [場面1](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:164) で追手は可視二人と追加一人になります。[場面4](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:221) では背後に回る追手への対処と、三人が斜面へ逃げられる因果が追いにくくなっています。各追手の位置と妨害された動き、残る追手が退路を塞げない理由を明確にしてください。全員を倒す必要はありません。

7. **後の楽しさへの仕込み** — [仕込み欄](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:97) は、カイが姉妹の魔法の発動と結果を観察し、まだ仕組みを理解しないことを場面3・4で示すとしています。現状の場面は姉妹の行動を説明していますが、カイが何を見て、何をまだ判断できないかは追いにくい状態です。既存の戦闘行動に沿って、カイの観察範囲と理解の限界を場面順へ接続してください。

8. **一人称で知り得る情報** — [場面1](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:173) の「姉妹は……本来の戦闘力を出せない」は、初対面のカイには比較対象がなく、この場面で明らかになる情報としては断定できません。見える負傷・疲労と動きの制約として書くか、後に根拠を得て分かる情報へ移してください。

9. **重複整理①：楽しみ欄** — [四つの楽しみ](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:25) の説明には、場面1〜5の具体的な行動・判断・結果を再作文した箇所があります。各楽しみの意図と対象人物は残し、具体的な出来事は場面順、残る変化は人物欄を正本として、一方向の参照でつないでください。

10. **重複整理②：人物欄・場面内補助欄・作者意図** — [人物の出口変化](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:119)、各場面の「人物の判断」「明らかになる情報」「感情や関係の変化」、[作者側の意図](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:256) で、場面順の行動や結果を言い換えて再保存しています。人物欄には変化を残し、判断は場面を参照させてください。場面内の補助欄と作者意図も、必要な固有情報だけを残すか正本位置を参照させてください。

11. **日本語の明確さ** — [楽しみ欄](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:64) の「露出を抑えた旅装に整った顔立ちと落ち着きが表れる」、[場面4](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:222) の「武器へ踏み込みを合わせて攻撃を崩し」「追手は武器を構え直すまでの隙を作られ」は、何が見え、誰が何をしたかを一読で取りにくい表現です。人物・動作・結果を普通の日本語で直接書いてください。[場面5](/home/oshizo/repo/novel-20260929-maker-trial/planning/episodes/episode-002.design.md:245) の「単独で進む」も、姉妹二人だけで進む意味に明確化してください。

作業開始版との差分は空で、`planning/pending-changes.md` と仮変更表示はありません。Canon六文書、Direction、Style、Overall v3、Arc01 v8、先行Episode000・001との互換を確認しました。設定提案は対象・上位とも「なし」で、依存表の欠落はありません。先行計画を執筆済みの事実としては扱っていません。品質改善の任意提案と作者の好みに委ねる事項はありません。
