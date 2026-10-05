# Issue #18 事後比較

3話のfresh runがすべてcompletedになった後、親agentが固定した旧PR #17の成果物と実行履歴を読んで比較した診断記録。作品設定やPlanningの正本ではなく、新たなStory Craft判定でもない。

一回Challenger制の運用と、指摘に応じた局所的な具体化は確認できた。ただし、全話が旧版より良くなったとは判定しない。とくに001では旧版の強い習得速度比較を初稿から再構成できず、期待された「十分強い初稿をPASSで通す」弁別は未確認である。

## 固定した参照と入力境界

- maker [PR #153](https://github.com/oshizo/novel-maker/pull/153)のmerge commit: `2e6aa815635cb9ea8c553ec0e4ad7dbab612038a`。merge済みを確認し、PR headやmainではなくこのcommitをpinした。
- trial [PR #17](https://github.com/oshizo/novel-20260929-maker-trial/pull/17)の開始時head: `ee1c4abac2684bc85131a744bbc0ce98bde437c9`。専用branchはここから作成。PR #17はOPEN/Draftのまま、mergeしていない。
- 旧6成果物だけを削除したbaseline: `75bc7124c4be89f8383ea10bea215304af6f53bb`。
- framework同期commit: `c35c10251822af7d3259de3790fcd32b4b33f80d`。
- 000 run開始commit: `c35c10251822af7d3259de3790fcd32b4b33f80d`。
- 001 run開始commit: `aa806018c0392b5dd5435cf3f5020991888da2a2`。
- 002 run開始commit: `5dc275801d2d8f459866c99ea76d4c272c65f737`。
- 3run完了時commit: `f8cbe65bb586f74961827f88685b37a7e08a9bcb`。

Direction、Canon、Overall v3、Arc01 v8、Style、State、Manuscriptは固定headから変更していない。runtime 19file、custom agent 10file、Planning入口、AGENTS.mdはmaker pinの正本と一致する。

生成・確認の各roleは履歴を引き継がない新規実行で開始した。同じrunの技術修正や中断再開だけは当該agentへ継続を依頼した。旧EpisodeのDesign、Writer Brief、review、snapshot、Issueの期待値、Humanの診断は生成入力へ渡していない。001には今回readyとなった000の出口だけ、002には今回readyとなった先行Episodeの出口と最終場面の継続情報だけを渡した。Regressionへ渡した比較snapshotも、そのrunの一回Revision後の版だけに限定した。

旧比較資料は3run完了後に固定commitから取得し、作業ツリーにはコピーしていない。以下の比較は旧初稿・Revision差分・Challenger・再判定・Regression、旧最終Design/Writer Brief、新初稿・Revision・最終Design/Writer Briefを根拠とする。

## 新runの結果

| Episode | 初回 | Planner改訂 | 再Challenger | 技術必須修正 | Regression | 執筆指示確認回数 | 最後の執筆指示確認 |
| --- | --- | ---: | ---: | ---: | --- | ---: | --- |
| 000 | REVISE | 1 | 0 | 5 | PASS | 3 | PASS |
| 001 | REVISE | 1 | 0 | 6 | PASS | 4 | PASS |
| 002 | REVISE | 1 | 0 | 8 | PASS | 3 | PASS |

全runで `revised_story_craft_verdict: null`、`story_craft_recheck_attempt_count: 0`、`recovery_count: 0`。BLOCKEDは使っていない。Regression PASSはREVISE解消の再評価ではなく、基準snapshotから改善意図が失われていないという判定である。

6成果物はすべてversion 1、ready/not-required。Designのparent_versionは8、Writer Briefのsource_versionは1。作者確認checkpointはOverall/Arcのみで、今回は上位・Canonを変更していない。

- 000: [Design](../../planning/episodes/episode-000.design.md)、[執筆指示](../../planning/episodes/episode-000.writer.md)、[run](../runs/20261005T143908Z-episode-episode-000/run.json)。
- 001: [Design](../../planning/episodes/episode-001.design.md)、[執筆指示](../../planning/episodes/episode-001.writer.md)、[run](../runs/20261005T152805Z-episode-episode-001/run.json)。
- 002: [Design](../../planning/episodes/episode-002.design.md)、[執筆指示](../../planning/episodes/episode-002.writer.md)、[run](../runs/20261005T231636Z-episode-episode-002/run.json)。

各runは初稿、Revision後、Finalizer後、Writer Briefの各生成後、ready確定後の全文snapshotと正式出力を保存している。Story Craft基準は各runの `snapshots/03-planner-revision-attempt-1.md`。

## episode-000

旧版は戸の修理、イヤホンの不具合調査、灯火で息の時機を変えて戻す試行が続く。同型の「調べる、条件を変える、確かめる」が三つの場面にあり、前世の人物像も問題解決の好みに寄りやすい。旧ChallengerはPASSで、Revisionは同級生との小テストの会話を加える一部採用だけだった。

新初稿では、前世の経験が「道具を調べ、人と話した」という説明に薄まり、灯火も具体的な違いがない。Challengerはこの自己接続と発見の弱さをREVISEとした。改善要求2件を採用し、参考対案1件を一部採用した。傘について最初の見立てを相手の話で改め、布の挟まりを外す経験と、覚醒直後・解熱後の家族への応答を置いた。灯火では、手の内側から指先へ流れ、炎が現れる前に流れ方が変わるという観察から、次の比較条件を選ぶ。

Finalizerは未実施の比較を実施済みと読める出口などを直した。Regressionは具体的な前世経験、家族への応答、魔法の観察と次の選択の維持を確認した。最終Writer Briefにも、比較をまだ行わない境界と具体的な観察が残る。

旧版と比べると、灯火の完成した比較試行を外した分、同型反復は一段減り、覚醒前後と魔法の新しい見え方が分かれる。ただし戸と傘の修理はなお似た成功で、反復が全面的に解消したとは言わない。旧版にあった同級生との授業・小テストの普段の会話は新案にない。前世の日常の広がりまで含めて新案が優位とも言い切れない。

期待したREVISEとラベルは一致したが、Challengerが直接指摘したのは反復ではなく、新初稿の具体性の不足である。反復の縮小はPlanner初稿からの構成変更とRevisionの結果であり、「旧版と同じ反復をChallengerが再発見した」とは扱わない。

## episode-001

旧初稿には、カイの短い試行による改善、角獣討伐、通常の術者が同じ切り替えの安定に何週間も反復した経験と驚き、報酬と回復薬代の節約が揃っている。旧ChallengerはPASS、Creative Revisionは変更なしだった。旧最終Writer Briefにもこの比較がある。

新初稿にも、二頭の戦闘間で操作を変え、二頭目を少ない魔力で止める成果はある。一方、依頼人の疑いに対して結末は支払いと安堵に留まり、手伝いも変化が偶然ではないかと受け止める段階だった。Challengerは、その場にある疑いと成果の前後差、残せた魔力を次の選択へ使う点をREVISEとした。

Revisionは改善要求を二つに分けて採用し、参考対案を一部採用した。手伝いが二頭の動きを比較して理由を尋ね、商人がカイの安全判断を頼ってから荷車を動かす。カイは節約した魔力を、負傷者がいる可能性を調べて戻る余力として数える。Finalizerは二重支払い、通常視覚と魔力知覚の混同、戦闘後の安全、人数や救助可能性の先取りを直した。RegressionとWriter Brief最終確認はこれらの承認と判断の維持を確認した。

このREVISEは、新初稿に置いた疑いを成果の後まで変えないという具体的な機会損失に基づく。称賛をただ大きくする要求ではなく、現時点でChallenger過敏を主因とは判断しない。しかし新案は旧版と同程度の強い比較を再構成していないため、「強い001が不用意にREVISEされたか」の直接検証にもなっていない。

最終版は商人の頼り方と当日の二戦の比較が明確になった。一方、通常の術者との習得速度差は旧版より弱い。手伝いが消費減を認めることは、通常なら何週間も要る改善を短時間で定着させた比較の代わりにはならない。Challengerは感想でこの価値の伝わりにくさに触れたが、改善要求は反応と余力の利用へ寄り、Revisionでも速度比較は具体化されなかった。ここは「Challenger取り逃し」に近い残差である。親ArcとCanonには比較の条件があり、「作品正本の不足」とする根拠はない。

期待外れの主因はPlanner初稿の再構成が旧版より弱かったこと。Issueの四分類では直接対応する項目がなく、残った速度比較については取り逃しに近い。全3話がREVISEである以上、常時REVISE化がないことやPASS側の弁別まで成功したとは結論しない。新Writer Briefは未確認事項の反復も多く、旧版の簡潔な終盤に対する改善は確認できない。

## episode-002

旧初稿でも三人の働きと暫定同行は成立し、旧ChallengerはPASSだった。任意の改善候補を採用して追手が姉妹を押さえ続けられない経過を詳しくしたが、最終版は各敵の位置と対処が長く続く。研究への疑いと役割の委任はある一方、姉妹がカイの技術のどこを評価したかは結末で短くまとめられている。

新初稿は、エルナの知らせ、リシェルの炎、カイの介入を順につなぎ、三人の必要性と限定的な委任を既に置いていた。Challengerは、魔力の乱れを知覚したことが普通の剣士との差として攻撃時機へ十分つながらない点、技術への評価の具体性、初対面の魅力の説明寄りな点をREVISEとした。改善要求2件を採用し、参考対案1件を一部採用した。

Revisionでは、魔力が散りまだ成形へ進んでいないことから発現までの遅れを見当づけ、観察を切り、短い強化と剣で術者を止め、退路が開く結果へつないだ。戦闘後に姉妹が切り替えや踏み込みの時機を質問し、理由を聞いて役割の一部を任せる。二人の具体的な衣装・顔立ち・動きも加えた。

Finalizerは武器付与開始から切り替えの順序、姉妹が外から見た動きと本人の説明、術者が腕を打たれて成形動作を続けられず発動を断念する因果を通した。名前と姉妹関係を知る時期、症状、撤退命令も観測範囲へ揃えた。Regressionは基準時点の判断、姉妹の評価、魅力を保持したと確認した。

最終Writer Briefでは、感知した情報、攻撃阻止、退路、戦闘後の質問と委任を追える。旧版よりカイの固有の感知が行動へつながる点と、姉妹が認める対象は具体的になった。救助の結末も命令と撤退として明確になった。一方、場面4の個別の攻防は旧版の方が詳しいため、戦闘全体の迫力まで新案が常に上とは断定しない。研究への疑い、姉妹の主体性、恋愛を確定しない境界は両版とも維持されている。

002は増幅余地を局所変更で拾ったケースと判断する。旧版の因果全体を作り直すのではなく、初稿にある協力を保って判断と評価を具体化した。期待したREVISEだけを成功根拠にはしていない。

## 工程の効果と限界

- 旧3runは初回と再判定でStory Craft Challenger計6回、新3runは計3回。再判定を省く契約は実行と記録の両方で守られた。
- 旧001ではPASS後にも変更なしのRevisionと再判定があった。新契約のPASSならこれらを省く分岐は存在するが、今回の初稿はすべてREVISEのため実行例としては未検証。
- Writer Brief独立確認は旧版が2/3/2回、新版が3/4/3回。全stage数は旧36、新38。Challenger回数が減ったことを、全工程の手間や速度が減った証拠にはしない。
- 新Writer Briefの最初の必須修正は000が4件、001が8件、002が7件。修正で具体的事実が落ちたり順序が変わった箇所も再確認で拾い、最終確認は全話PASS。これは新たな創作Revisionではなく、Designからの変換修正である。
- 000のFinalizerはcapacityエラー、001のChallengerはusage limitで中断した。同じrole/model/入力で再開し、判定やrunの回数をリセットしていない。fallbackは全runで0。中断時間を含むため経過時間による速度比較はしない。
- Sol roleはspawn時に `gpt-6.1-sol/high`、Luna roleは `gpt-6-luna/medium` を設定した。起動応答から実model名を確認できないため `actual_model: null` を保ち、設定値を実測値として転記していない。

## 検証と停止地点

pinの `check-story`、3runの工程順・回数・正式出力・snapshot・参照版の検証、baselineの6file削除、固定作品入力と既存runの保持、Git差分検査を実施した。構造検査をStory Craftの合格や本文品質評価には用いていない。

保存した隣接抜粋2fileの余分な末尾空行だけを3run完了後に除いた。各runに渡したbytesのhashと、正確な抜粋を保持するcommit `f8cbe65bb586f74961827f88685b37a7e08a9bcb` を記録し、保存版のhashと区別した。002の初回Writer Briefにあった作者確認pendingの誤記も、親がstory.yamlに従いnot-requiredへ修正し、状態変更をrunに記録した。

Arc01の3話は執筆入口まで揃った。本文は書かず、後続Arcも詳細化していない。比較上の残差を埋めるためにChallengerやPlannerを追加で回すことはしていない。

## 旧比較元の実行履歴

固定head内の次の3runを使用した。既存runの判定・採否・回数は書き換えていない。

- [旧000 run](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T112650Z-episode-episode-000/run.json)
- [旧001 run](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T144510Z-episode-episode-001/run.json)
- [旧002 run](https://github.com/oshizo/novel-20260929-maker-trial/blob/ee1c4abac2684bc85131a744bbc0ce98bde437c9/.novel-maker/runs/20261004T153610Z-episode-episode-002/run.json)
