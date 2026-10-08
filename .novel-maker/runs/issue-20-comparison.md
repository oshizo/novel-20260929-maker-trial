# Issue #20: maker #155適用後のEpisode 000〜002の比較

## 結果と評価の範囲

Episode 000〜002のDesignと執筆指示はすべて `ready`。各話を初稿、独立した一回Challenger、fresh Plannerによる一回改訂、Technical Reviewer、Finalizer、Story Craft Regression、執筆指示生成、独立した執筆指示確認まで進めた。3初稿の判定はすべて `REVISE` だった。再Challengerは実行していない。

初稿の改善は一様ではない。001では熟練術者との習得速度の比較を初稿から場面へ置けた。000の記憶覚醒と002の救助の山場は、創作意図で宣言した強度に対して具体的な行動・感情差が薄く、Challenger後に具体化された。初稿の候補探索が広がって強い案を選べるようになった、とこの3runだけで結論づける材料は不足している。

これは生成完了後の診断記録であり、PlannerやWriterの通常入力に加えない。比較を理由に確定したDesign・執筆指示や上位入力を手直ししていない。本文執筆と後続Arcの詳細化は今回の対象外。

## 開始点と入力の分離

- 開始点はPR #17の固定SHA `ee1c4abac2684bc85131a744bbc0ce98bde437c9`。専用branchは `codex/issue-20-planner-exploration`。
- 除外baseline commitは `f0650a94db0c76837cc33a8a3646624257abb287`。親commitが固定SHAで、削除対象は旧Episode 000〜002のDesign・執筆指示の6ファイルだけ。
- maker #155のmerge commit `8e37227f6ec2f3a96ea7e6e5ba5f4d419a1b8045` へframework-syncしたcommitは `aabd1b10cf46419c6b1c2dc638b548d58d9ce2cf`。maker #155のhead `ff8b0cebcdc5280106751f5dd21a65bdf91ff9cd` とmerge commitのtreeが同一であることも確認した。
- bootstrap時の写しである `.codex/agents/` はsyncで暗黙更新されないため、Episode Planner、Challenger、Technical Reviewer、Finalizer、Regressionの5設定を今回のmaker revisionのtemplateへ明示的に更新した。
- Story Direction、Canon、Overall、Arc01、Style、State、Manuscriptは固定SHAから内容を変えていない。Overall v3・Arc01 v8は既存の作者承認を維持し、Episodeの作者確認は `not-required`。
- 各担当は履歴を引き継がないfresh agentとして起動し、確定した作品入力と現在工程に必要な正式出力だけを渡した。旧Episode・旧run・Human指摘は生成担当へ渡していない。
- rolling planningとして001には今回確定した000 Design、002には今回確定した000・001 Designを渡した。各runの入力commitと入力全文のsnapshotを保存した。
- PR #19のhead `b32a0104e40035c39181c87fda4d5b17a77ba3b1` は今回のbranchの祖先ではない。3話の生成と確認がすべて終わったcommit `8e0e2d5cb52eee484c23775b34fcfef2e6463803` の後に、旧runとPR #19を事後比較した。

## 比較資料と観測上の限界

旧runはPR #17固定SHAに保存された2026-10-04の3run、PR #19は上記固定headに保存された2026-10-05の3runを使った。初稿・初稿正式出力・Challenger・改訂案・改訂正式出力・Finalizer案・Finalizer正式出力・ready Design・ready執筆指示の実体を読み、内容を比較した。取得元SHAと各pathは [比較資料の索引](issue-20-comparison-sources.json) に保存した。

旧runのframework revisionは `f4491ce08c3c3be8f092ed20f48587e89e80ec87`、PR #19は `2e6aa815635cb9ea8c553ec0e4ad7dbab612038a`。PR #17保存時点の作品入力と今回固定した作品入力が一致していても、旧runが初稿時点で参照した入力、レビュー契約、直前Episodeは同一とは限らない。今回も直前Episodeは新規生成した版である。1作品・各話1runの事後比較なので、差をmaker #155だけの因果効果やframework全体の成功率として扱わない。

旧000初稿はStyleも初回作成しているため、今回の固定済みStyleを使う生成とは条件が異なる。旧runとPR #19の6初稿正式出力には、今回の「Challengerへ渡す創作意図」5項目の独立した記録がない。過去のDesignから過去の選考過程や採用意図を補作しない。今回観測できるのも採用した短い意図と出来上がった場面であり、内部の候補探索の全履歴ではない。

## Episode 000: 記憶覚醒と同型の満足

### 初稿と短い創作意図

旧run初稿は、戸の受けの緩みを直す、昼休みに同級生のイヤホンの音切れを調べる、灯火で息の時機を変えて比べる、という三つの観察・切り分けを並べていた。一方、同級生の驚きと礼、当時の気まずさや嬉しさ、看病する家族の声や手の温度も初稿からあった。記憶覚醒の感情が全くなかったとは扱わない。

PR #19初稿は、日本の学校生活や道具を直す経験を要約し、現在の家族への応答と灯火の具体的な違いが薄かった。今回の初稿も、日本の暮らしを学校・会話・修理の列挙で示し、身近な初歩魔法の流れを感じて今後試すと決める段階にとどまる。旧run初稿にあった具体性を、初稿から安定して再構成できたとは言えない。

今回の短い意図は、二つの人生を引き受けることを中心に選び、「親しみ→断絶への心配→受容の安堵→学ぶ期待」を狙う。また上位の強度条件として、前世を分析癖の由来だけに縮めないと明記した。しかし増幅方向は戸の修理と魔法へ向かう同じ確認姿勢を結ぶもので、記憶覚醒固有の別の感情を初稿の具体的な場面へ十分に落としていない。

### Challengerと一回改訂

旧runは `PASS`。任意の改善候補として、イヤホンを調べた後の普通の高校生活のやり取りを提案し、改訂で授業や小テストの話が加わった。同型反復そのものへの要求はなかった。

PR #19は `REVISE`。前世の具体的な経験と現在の家族への応答、灯火の具体的な違いを要求し、改訂で同級生の傘の引っかかりを調べて直す記憶、家族への返事と水の受け取り、灯火の発現前の流れの観察が加わった。傘修理も戸修理と近い満足だった。

今回も `REVISE` で、前世の具体的な経験・現在の家族との感情変化と、身近な魔法で感じる差を要求した。改訂では、友人のシャープペンシルの詰まりを一緒に確かめ、友人の笑顔と一緒に歩いた帰り道を思い出す。目覚めたカイが家族の問いに答え、水を受け取り、前世の友人と今世の家族を順に思い浮かべる。灯火は同じ場所で息を吐く速さだけを変えて二度試す。

この改訂で、混乱から家族がそばにいる安堵への変化は行動と記憶に結びついた。ただし「戸修理→前世の道具修理→一条件の魔法比較」は残る。友人と帰る時間は修理以外の経験だが、旧runにも改訂で日常会話があったため、満足の種類が大きく広がったとは判断しない。今回のChallengerも反復を主要な改善要求に昇格させていない。

### Finalizer後Designと執筆指示

旧run Finalizerはイヤホンを完全修理した扱いを避け、灯火の比較結果を一つの原因へ断定しないまま、友人と家族の感情を保った。PR #19 Finalizerは覚醒直後の応答と回復後の受容を時間で分け、灯火は次の比較を選ぶところで終えていた。

今回のFinalizerは、戸の受け金具のねじ穴が広がり、木片で穴を埋めて同じねじで固定する因果を具体化した。灯火の二度目は発動までが短いように感じるが、病み上がりの感覚もあり原因は未確定。受容の感情と友人との経験は保たれ、Regressionは `PASS`。

今回のready執筆指示には、友人の笑顔と帰り道、家族へ答えて水を受け取る行動、二度の灯火と未解明の原因が残る。旧版と同様、設計上の報酬評価を本文へ渡す必要はない。初回の執筆指示には、寝ているカイが看病を見た扱いや知識を持つ時期の混同があり、独立確認で修正した。その後の日本語の局所修正で助詞の不備が残り、確認は計4回。最終版には候補探索、短い創作意図、Challengerの批評を含めていない。

新run: [初稿と創作意図](20261008T005053Z-episode-episode-000/reviews/01-planner-initial-attempt-1.md)、[Challenger](20261008T005053Z-episode-episode-000/reviews/02-story-craft-challenger-attempt-1.md)、[一回改訂](20261008T005053Z-episode-episode-000/reviews/03-planner-revision-attempt-1.md)、[Finalizer案](20261008T005053Z-episode-episode-000/snapshots/05-finalizer-attempt-1-planning__episodes__episode-000.design.md)、[ready執筆指示](20261008T005053Z-episode-episode-000/snapshots/99-ready-planning__episodes__episode-000.writer.md)。

## Episode 001: 短時間の習得と熟練者との比較

### 初稿と短い創作意図

旧run初稿は、同程度の討伐で魔力を残せた成果と、町の術者が自分は師の指導で何週間も同じ切り替えを繰り返したという具体的な経験を並べていた。術者は戦闘には同行せず、討伐後の報告を聞いて速さに驚く。

PR #19初稿にも通常なら長い反復が必要という文は報酬欄と情報欄にあった。ただし場面中の反応は、商人の契約履行への承認と手伝いの動きの違いへの気づきが中心で、熟練した術者自身の経験とカイの速さを並べる場面はなかった。改訂で商人が道の安全判断を頼り、手伝いが二度の戦いを比較したが、長い反復との比較を同じ強さで場面へ戻してはいなかった。

今回の初稿は、知識・魔力量・護衛経験でカイに勝る同行術者を置き、カイの知覚と定着速度だけが特異であることを守る。短い失敗と試し直しを挟み、その日の護衛戦闘で改善を使い、熟練者が通常なら長い反復を要すると認める。単なる商人の称賛へ弱めていない点は、PR #19との差として確認できる。

短い意図でも「通常なら長い反復→カイはその日に実戦投入」「熟練者の知識・経験を保つ」「改善前後の消費と追加強化を比較」を中心に選んだ。ただし初稿の最初の戦闘では、どの動作に追加強化が必要だったかが場面から十分に分からず、後の追加不要という説明を同じ仕事で比較しにくかった。

### Challengerと一回改訂

旧runは `PASS`、改善要求・候補なし。旧契約のRevisionは実質変更なしだった。

PR #19は `REVISE`。依頼人と手伝いの反応や頼り方、節約した魔力と救助判断のつながりを要求した。読後感では習得速度の価値の弱さに触れているが、その比較を要求として明確に扱っていなかった。

今回も `REVISE` だが、要求は改善前後を同じ働きで比較できる場面へ向けられた。地形で一体ずつ相手にできる効果と、省魔力の効果を区別し、熟練者の承認を目撃した差へ結びつける。上位の比較条件と採用した意図の未達を優先し、同行術者を弱くする要求はしていない。

改訂では、最初の襲撃で武器付与から身体強化へ移る際に集め直す遅れがあり、荷車の前へ戻るため追加の強化が要る。短い試行後の近い道幅・距離の戻りでは追加強化を使わずに間に合い、同じ切り替えを実戦で繰り返す。同行術者が二度の呼吸・動作の差を見て、カイの説明と合わせて速さを認める。比較の喜びとカイの自信が、報酬の見出しだけでなく出来事に結びついた。

### Finalizer後Designと執筆指示

旧run Finalizerは魔法操作と術者の登場時期を整え、数週間との比較を保持した。PR #19 Finalizerは二重の報酬支払い、知覚と視覚の混同、獣を退けた後の確認を修正し、商人の安全判断の委任と次の調査へ使える魔力を保持した。

今回の初回Finalizerでも、比較の攻防と熟練者の目撃は保持された。一方、終盤に武装者二人を先に目撃する出来事と新しい接近判断が加わった。Regressionはこれを新しい創作上の前提・出口変更として `FAIL` にした。採用意図を保つ限定したRecoveryを1回行い、終盤・出口・カイの終了状態だけを修正した。敵の人数と負傷者の所在は未確定のまま、林の手前の見通せる位置へ確認に向かう出口へ戻した。差分Technical Reviewerの追加必須修正は0件、2回目の正式Regressionは `PASS`。再Challengerや追加Planner Revisionは行っていない。

ready執筆指示では、余剰魔力の循環の一般知識と、今回初めて成功した戻す時機を区別し、同行術者が観察できる呼吸・動作と本人の説明だけを承認の根拠にする。初回確認の3指摘を直し、2回目で指摘0件。選考や批評は渡さず、通常の術者との比較は登場人物の経験・発話内容として残した。これが設計分析の漏れとはならない。

新run: [初稿と創作意図](20261008T010649Z-episode-episode-001/reviews/01-planner-initial-attempt-1.md)、[Challenger](20261008T010649Z-episode-episode-001/reviews/02-story-craft-challenger-attempt-1.md)、[一回改訂](20261008T010649Z-episode-episode-001/reviews/03-planner-revision-attempt-1.md)、[初回Regression](20261008T010649Z-episode-episode-001/reviews/06-story-craft-regression-attempt-1.md)、[回復後Finalizer案](20261008T010649Z-episode-episode-001/snapshots/07-finalizer-attempt-2-planning__episodes__episode-001.design.md)、[ready執筆指示](20261008T010649Z-episode-episode-001/snapshots/99-ready-planning__episodes__episode-001.writer.md)。

## Episode 002: 救助の決め手と姉妹の扱いの差

### 初稿と短い創作意図

旧run初稿は、姉妹の風・炎とカイの短い魔法をつなぐ救助、異なる容姿と旅装、警戒した姉妹が守りや進路判断を一部任せる変化を置いていた。追手が退く因果は短いが、衣装や妹を守る役などの具体的な内容は初稿からあった。

PR #19初稿では、カイだけなら側面から挟まれる状況を姉妹が崩し、術者の攻撃を止めて脱出する。警戒から限定的な委任への差もある。ただし知覚した乱れがカイ固有の勝機へつながる一手、姉妹の技術への評価、身体的な魅力は説明寄りだった。

今回の短い意図も中心を「三人の働き→姉妹の限定的な評価」に選び、カイ単独では攻撃へ移れない状況から姉妹の情報・攻撃を使う増幅を採る。初稿に危機を脱する安堵はあるが、決着は乱れを捉える・一体ずつ退けるという要約が多い。姉妹が何を言い、何を任せるかも抽象的な説明が中心だった。

今回の初稿で、救出のカタルシスを独立して増幅した選択や、追手の侮り・加害意図への反撃を選んだ記録は見つからない。上位の4つの報酬を並べ、三人の協力と限定的な評価の範囲を具体化する案で、PR #19から報酬の種類が広がったとは言えない。これらの別方向をすべて採用すべきという評価ではなく、採用した方向と場面の広がりの観測である。

### Challengerと一回改訂

旧runは `PASS`。追手が捕縛を続けられなくなる因果を任意候補にし、改訂では武器を構え直す間、風で乱れた足場、炎を避けた進路によって三人が斜面を抜けられる結果を加えた。

PR #19は `REVISE`。カイだけが知覚した発現の遅れから構えた腕を打ち、退路を開く一手と、姉妹の質問・任せ方を具体化した。初対面の衣装と身体の使い方も改訂で強めた。

今回も `REVISE`。要求は上位の身体的な魅力、採用した中心報酬の具体的な攻防、姉妹それぞれの言動による扱いの差の順。カイの知覚を使わなければ止められなかった攻撃と、開いた退路まで求め、001の習得速度比較をこの回で再証明する要求にはしていない。三人の働きと限定的な評価という選択を捨てさせてもいない。

改訂では片肩と腹部を見せる踊り手風のリシェルと、首元まで覆うエルナの旅装・表情を短く置く。エルナが側面の危険を伝え、リシェルが正面の注意を引き、カイが攻撃直前の腕側への魔力の偏りを捉えて踏み込む位置を変え、剣で攻撃を止めて通り道を保つ。救助後はリシェルが妹の傷を確かめる間の反対側の警戒を頼み、エルナが二つの道と追跡の兆候を共有して進路判断を頼む。警戒して問いかけた初対面との行動の差が明確になった。

この改善はPR #19の改訂と同じく、選んだ方向の具体化である。初稿で広い報酬候補から新しい中心を選べた証拠としては扱わない。

### Finalizer後Designと執筆指示

旧run Finalizerは追手三人の位置と退路を詳細化し、魔力知覚を一回に揃え、姉妹が見た切り替えと父の訓練との比較を会話へ移した。PR #19 Finalizerは成形の身振りに使う腕を打って発動できなくなる因果、名前と姉妹関係を知る時期、見聞きできる撤退命令を整えた。

今回のFinalizerは介入前に追手三人・魔物二体と退路を確かめる順序を整え、姉妹の内面を戦闘中に断定せず視線の移動と後の発話で示した。父グレンとの関係は戦闘後に伝え、追手が何を求めているかは明かさない。局所的な人数の具体化はRegressionで新しい主要前提には当たらないと判定され、正式Regressionは `PASS`。一回目の起動は利用上限で正式出力がなく、作者の「再開して」を受けて同じrole・modelで再実行した。創作上のRecoveryは0回である。

ready執筆指示は、知覚から攻撃阻止、余剰魔力の循環と切り替え、姉妹の自発的な支援、具体的な委任を保持する。旧runのような全員の位置を細かく追う長い攻防に比べ、必要な因果を残しながら細かい剣の動作は本文へ委ねた。

初回執筆指示には負傷者をカイにも広げる誤読、姉妹の安堵をカイの感情欄へ入れる混同、抽象的な評価、不要な細かい剣の動作があり、確認と修正を繰り返した。後続案では「姉妹に追われる」という主語の誤りと説明の重複、不要な同義反復を直し、4回目で指摘0件。最終版で姉妹の安堵はカイが見た表情・動作、評価は実際の頼み方として残る。盗用を疑われたカイの不快はCanonの人物関係に根拠があり、選考過程やChallenger批評を移したものではない。早い案まで漏れや混同がなかったとは主張しない。

新run: [初稿と創作意図](20261008T013441Z-episode-episode-002/reviews/01-planner-initial-attempt-1.md)、[Challenger](20261008T013441Z-episode-episode-002/reviews/02-story-craft-challenger-attempt-1.md)、[一回改訂](20261008T013441Z-episode-episode-002/reviews/03-planner-revision-attempt-1.md)、[Finalizer案](20261008T013441Z-episode-episode-002/snapshots/05-finalizer-attempt-1-planning__episodes__episode-002.design.md)、[再開後Regression](20261008T013441Z-episode-episode-002/reviews/07-story-craft-regression-attempt-2.md)、[ready執筆指示](20261008T013441Z-episode-episode-002/snapshots/99-ready-planning__episodes__episode-002.writer.md)。

## 工程の確認

| 対象 | 初回Challenger | Planner改訂 | 再Challenger | 技術必須修正 | 正式Regression | 創作上のRecovery | 執筆指示確認 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 000 | REVISE | 1回 | 0回 | 3件 | PASS・1回 | 0回 | 4回・最終0件 |
| 001 | REVISE | 1回 | 0回 | 6件 | FAIL→PASS・2回 | 1回 | 2回・最終0件 |
| 002 | REVISE | 1回 | 0回 | 5件 | PASS・1回 | 0回 | 4回・最終0件 |

002の `regression_attempt_count: 2` は利用上限で終了した起動を含む実行回数。正式な判定は1回だけ。失敗したstageは `executor-blocked` と理由を保存し、レビュー・snapshot・判定を捏造せず空にした。`execution_resumptions` に作者の再開指示と同じrole・modelで再実行する根拠を記録した。

Plannerと執筆指示生成は `gpt-6-luna / medium`。Challenger、Technical Reviewer、Finalizer、Regression、執筆指示確認はpin済みmodel policyに従い `gpt-6.1-sol / high` を指定した。model fallbackは全runで0回。executorが実際のmodel telemetryを返さないため、`actual_model` は `null` のまま、その観測限界を各runへ明記した。

Challengerが上位条件と採用意図の未達を扱い、001の熟練者や002の主体的な姉妹を保って局所修正したことは確認できる。一方、今回の初稿には強い侮りや性的脅威などを中心に選んだ案がないため、大胆・王道・俗っぽい方向を無難化しない契約を十分に試したとは言えない。3件のREVISEを固定した期待値にも、失敗率や成功率にも使わない。

最終の執筆指示は、具体的な行動・発話内容・事実、カイの内部状態、情報を知る時期、継続条件だけを残した。通常の術者との比較や姉妹の委任は作中の出来事として必要なので保持する。候補探索、5項目の創作意図、読者報酬の採点、Challengerの感想と改善要求は含めない。独立した執筆指示確認の最終正式出力とready snapshotを全話保存した。

## 検証と残課題

[run・来歴の検証結果](issue-20-run-validation.json) は、固定SHAからの分岐、6削除のbaseline、PR #19を祖先に含めないこと、固定入力の不変、各入力snapshotの対応commitとの一致、工程数と正式出力、再開記録、ready snapshotと正本の一致、metadataを確認した結果で `PASS`。創作の満足を自動テストで採点した結果ではない。Story Craftと執筆指示の内容は独立roleの正式確認による。

- 同期後の `python3 scripts/novelctl.py check-story /home/oshizo/repo/novel-20260929-maker-trial` は成功。
- `git diff --check` は成功。
- maker #155 headの `test_story_craft_recheck_contract.py` は16件すべて成功。
- maker #155 headの全体テストは155件中28件失敗。同期前revision `2e6aa815635cb9ea8c553ec0e4ad7dbab612038a` は152件中26件失敗。失敗名の集合で26件が共通、追加は `test_agents_use_japanese_artifact_terms` と `test_planning_issue_template_uses_japanese_visible_terms` の2件。全体テストを成功扱いにはしない。
- 追加2件はEpisode Planner templateとkickoffで「Writer Brief」を可視語として追加した箇所が日本語ラベルの既存契約に抵触していた。既存26件とは分けて [maker #156](https://github.com/oshizo/novel-maker/issues/156) に切り出した。

初稿の宣言と場面の強度の差、000の同型反復、002の採用方向が親Arcの見出しの範囲に留まる点は、[maker #157](https://github.com/oshizo/novel-maker/issues/157) に切り出した。改善前の初稿や指摘をrunから消さず、story固有の親agentによる手修正で隠さない。

## 旧版の工程別参照

| 保存版・対象 | 初稿 | 初稿報告 | Challenger | 改訂案 | Finalizer案 | ready執筆指示 |
| --- | --- | --- | --- | --- | --- | --- |
| PR17/episode-000 | [Design](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T112650Z-episode-episode-000/snapshots/01-planner-initial-attempt-1-planning__episodes__episode-000.design.md) | [報告](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T112650Z-episode-episode-000/reviews/01-planner-initial.md) | [確認](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T112650Z-episode-episode-000/reviews/02-story-craft-challenger.md) | [改訂](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T112650Z-episode-episode-000/snapshots/03-planner-revision-attempt-1-planning__episodes__episode-000.design.md) | [確定前](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T112650Z-episode-episode-000/snapshots/07-finalizer-attempt-2-planning__episodes__episode-000.design.md) | [執筆指示](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/planning/episodes/episode-000.writer.md) |
| PR17/episode-001 | [Design](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T144510Z-episode-episode-001/snapshots/01-planner-initial-attempt-1-planning__episodes__episode-001.design.md) | [報告](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T144510Z-episode-episode-001/reviews/01-planner-initial.md) | [確認](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T144510Z-episode-episode-001/reviews/02-story-craft-challenger.md) | [改訂](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T144510Z-episode-episode-001/snapshots/03-planner-revision-attempt-1-planning__episodes__episode-001.design.md) | [確定前](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T144510Z-episode-episode-001/snapshots/06-finalizer-attempt-1-planning__episodes__episode-001.design.md) | [執筆指示](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/planning/episodes/episode-001.writer.md) |
| PR17/episode-002 | [Design](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T153610Z-episode-episode-002/snapshots/01-planner-initial-attempt-1-planning__episodes__episode-002.design.md) | [報告](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T153610Z-episode-episode-002/reviews/01-planner-initial.md) | [確認](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T153610Z-episode-episode-002/reviews/02-story-craft-challenger.md) | [改訂](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T153610Z-episode-episode-002/snapshots/03-planner-revision-attempt-1-planning__episodes__episode-002.design.md) | [確定前](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T153610Z-episode-episode-002/snapshots/06-finalizer-attempt-1-planning__episodes__episode-002.design.md) | [執筆指示](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/planning/episodes/episode-002.writer.md) |
| PR19/episode-000 | [Design](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T143908Z-episode-episode-000/snapshots/01-planner-initial-attempt-1.md) | [報告](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T143908Z-episode-episode-000/reviews/01-planner-initial-attempt-1.md) | [確認](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T143908Z-episode-episode-000/reviews/02-story-craft-challenger-attempt-1.md) | [改訂](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T143908Z-episode-episode-000/snapshots/03-planner-revision-attempt-1.md) | [確定前](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T143908Z-episode-episode-000/snapshots/05-finalizer-attempt-1.md) | [執筆指示](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/planning/episodes/episode-000.writer.md) |
| PR19/episode-001 | [Design](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T152805Z-episode-episode-001/snapshots/01-planner-initial-attempt-1.md) | [報告](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T152805Z-episode-episode-001/reviews/01-planner-initial-attempt-1.md) | [確認](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T152805Z-episode-episode-001/reviews/02-story-craft-challenger-attempt-1.md) | [改訂](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T152805Z-episode-episode-001/snapshots/03-planner-revision-attempt-1.md) | [確定前](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T152805Z-episode-episode-001/snapshots/05-finalizer-attempt-1.md) | [執筆指示](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/planning/episodes/episode-001.writer.md) |
| PR19/episode-002 | [Design](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T231636Z-episode-episode-002/snapshots/01-planner-initial-attempt-1.md) | [報告](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T231636Z-episode-episode-002/reviews/01-planner-initial-attempt-1.md) | [確認](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T231636Z-episode-episode-002/reviews/02-story-craft-challenger-attempt-1.md) | [改訂](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T231636Z-episode-episode-002/snapshots/03-planner-revision-attempt-1.md) | [確定前](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/.novel-maker/runs/20261005T231636Z-episode-episode-002/snapshots/05-finalizer-attempt-1.md) | [執筆指示](https://github.com/oshizo/novel-20260929-maker-trial/blob/b32a0104e40035c39181c87fda4d5b17a77ba3b1/planning/episodes/episode-002.writer.md) |
