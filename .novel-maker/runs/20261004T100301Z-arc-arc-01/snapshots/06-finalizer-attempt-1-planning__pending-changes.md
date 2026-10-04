# 未確定の変更

- run-id: 20261004T100301Z-arc-arc-01
- 継続元run-id: 20261004T021235Z-arc-arc-01
- 今回の作業開始版: 497e738e11b7f1b8e0cb21297ba7ba2bd554915d (`planning/arcs/arc-01.md`, `planning/overall.md`, `planning/pending-changes.md`, `story-direction.md`, `canon/`)
- C1提案前の版: 9a08dbc64cf2886d8414185e120ca003ee27cce7 (`planning/arcs/arc-01.md`, `planning/overall.md`, `story-direction.md`, `canon/characters.md`, `canon/world.md`)
- 旧計画が参照した作品入力: be66ea585ac4987f364919f8cd05d0646ec96826
- 継続時のmetadata: Overall version 2 draft / pending、Arc version 7 draft / pending、parent_version 2。C1は未採用。
- 継続の根拠: 作者の2026-10-04明示指示とmaker PR #151の契約変更。旧runはblockedのまま保持する。
- 現在の対象: `planning/arcs/arc-01.md`
- 変更禁止条件: `story-direction.md` のmemory-awakening導入指定と必ず守る方向、既存Canonの成立済み事実。既存本文は未作成。
- 一式としてレビューする範囲: `canon/characters.md`、`canon/world.md`、`canon/relationships.md`、`canon/magic.md` の時点表記、`planning/overall.md` の入口・主人公の開始状態・arc-01の役割と境界・読者の楽しみの参照整理、`planning/arcs/arc-01.md` 全体。
- 必要な作者確認: 仮変更を含む `planning/overall.md` と `planning/arcs/arc-01.md` を作品として採用するか。一式の内部確認が終わるまで採用・ready化しない。

## C1：記憶覚醒の導入

- 修正先: `planning/overall.md > 入口と最終出口 > 入口`、`中心となる対立と長期変化 > 主人公 > 物語冒頭の状態・途中で変わること`、`Arc一覧 > arc-01 > 役割・入口`。`planning/arcs/arc-01.md > 役割と境界 > 役割・入口`、`Episode一覧 > episode-000`。
- 変更種別: 既存の救助から始まる入口を置換。OverallとArcの役割を改訂。Arcのepisode-000の役割と境界を置換。
- 理由: 作品方針が指定する記憶覚醒導入を、覚醒前のカイの行動から始める。十代半ばのカイと前世のカイを示し、成人後の旅と救助へつなぐ。
- 変更前の参照: 9a08dbc64cf2886d8414185e120ca003ee27cce7 の同じfileと見出し。これは今回の作業開始版497e…とは異なる。旧入力の参照はbe66ea585ac4987f364919f8cd05d0646ec96826。
- 依存する計画: Arc 01の入口、冒頭導入とepisode-000、成人後を描くepisode-001、姉妹救助を描くepisode-002。
- 影響する既存計画: 9a08…にはArc 01がversion 7 draft / pendingで存在した。今回の一式で改訂する。Episode Designは未作成。Overall内のarc-02以降は、Arc 01の暫定同行からarc-02の共同生活へつながるため内容の再生成は不要。詳細な後続Arcは未作成。
- 元のmetadata: 9a08…のOverallはversion 2 ready / approved、Arc 01はversion 7 draft / pending、parent_version 2。Overallの中間的なstale扱いはC1提案前のmetadataではない。今回の開始版497e…では両者draft / pendingであり、確定まではversionとparent_versionを維持する。

## C2：助けを必要とする人を見捨てたくないカイの気持ち

- 修正先: `canon/characters.md > カイ・フェルド`、`planning/overall.md > Arc一覧 > arc-01 > 役割`、`planning/arcs/arc-01.md > 役割と境界 > 役割`、`このArcで進むこと > カイの目的と選び方`、`Episode一覧 > episode-001 > 役割`。
- 変更種別: CanonとOverallへ追加。Arcの役割、カイの状態変化、episode-001の既存文を置換。
- 理由: 救助へ向かう判断に、危険を避ける理由があっても困っている人を見捨てたくない本人の気持ちを示す。冒頭で家族に役立とうとする行動と成人後の救助判断をつなぐ。
- 変更前の参照: 497e738e11b7f1b8e0cb21297ba7ba2bd554915d のCanon・Overall・Arc。C1の元参照9a08…とは別。
- 依存する計画: Overallのarc-01役割、Arc 01のカイの選び方とepisode-001の救助へ向かう判断、episode-002の救助。
- 影響する既存計画: OverallとArc 01を同じ変更一式で確認する。Arc 02以降の詳細とEpisode Designは未作成。
- 元のmetadata: 今回の開始版497e…ではOverall version 2 draft / pending、Arc version 7 draft / pending、parent_version 2。Canonにmetadataなし。

## C3：冒頭と姉妹救助時の区別

- 修正先: `canon/characters.md > カイ・フェルド、リシェル・エルシア、エルナ・エルシア、ダリオ`、`canon/world.md > 姉妹がカイと出会う前に起きたこと`、`canon/relationships.md > カイと姉妹が出会う前の三人`、`canon/magic.md > 成人したカイが姉妹を救助する時点の主要人物、一般的な術者との比較基準`。`planning/overall.md > 中心となる対立と長期変化 > 主人公・能力と研究・生活と選択肢`。`planning/arcs/arc-01.md > このArcで進むこと`。
- 変更種別: 既存の「物語開始時点」「開始時点」とその見出しを、十代半ばの物語冒頭と二十代前半の姉妹救助時点に分けて置換。Arcの状態欄も両時点に分けて整理。
- 理由: 成人後の一人旅や姉妹の負傷・逃走・ローデン城撤退を、十代半ばの冒頭以前の事実と誤読させない。
- 変更前の参照: 497e738e11b7f1b8e0cb21297ba7ba2bd554915d の上記fileと見出し。C1以前の比較には別途9a08…を使う。
- 依存する計画: Overallの主人公と主要人物の開始状態、Arc 01の状態変化、episode-000からepisode-001・002への時間の経過。
- 影響する既存計画: OverallとArc 01を一式で修正。arc-02以降の入口は救助後のままで互換。詳細な後続Arc、Episode Design、本文、Stateは未作成。
- 元のmetadata: Overall version 2 draft / pending、Arc version 7 draft / pending、parent_version 2。Canonにmetadataなし。

## C4：Overall内の技術的な重複整理

- 修正先: `planning/overall.md > 入口と最終出口 > 最終出口`、`読者に何を楽しませるか` の各楽しみと実現Arc欄。
- 変更種別: 最終出口から攻略決定の再掲を削除。各楽しみの「作品全体でどう発展するか」を削除し、進展の正本であるArc一覧と長期変化への一方向の参照に統合。異性的な近さの実現Arc欄を割当に整理。
- 理由: 同じ進展を楽しみ欄とArc一覧の両方で説明していたため。楽しみ、対象、前提、実現するArc、結果の参照は保持する。
- 変更前の参照: 497e738e11b7f1b8e0cb21297ba7ba2bd554915d:planning/overall.md の該当見出し。
- 依存する計画: OverallのArc一覧と長期変化、Arc 01の引き継ぎ元・前提参照。
- 影響する既存計画: Arc 01の参照先を照合して修正。Arc 02以降の詳細、Episode Design、本文、Stateは未作成。
- 元のmetadata: Overall version 2 draft / pending。Arc 01 version 7 draft / pending、parent_version 2。

変更本文には対応する仮変更表示を残す。C1とC2の採用は未決定で、C3とC4も一式の作者確認より前に表示を外さない。
