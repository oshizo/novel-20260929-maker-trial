Technical Review判定: **要修正（必須8件）**。採用した改善意図は、固定入力と両立しており、維持できます。新しいStory Craft案はありません。

1. **同じ出来事と変化の重複保存。** [Arc](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:38)の楽しみ欄には、救助の戦闘手順、姉妹の評価と相談、容姿への反応が詳しく書かれ、`このArcで進むこと` と `Episode一覧` にもほぼ同じ内容があります。仕込み欄、`役割と境界 > このArcより後へ残すこと` にも状態説明の再掲があります。楽しみの意図と対象人物は残し、具体的な出来事はEpisode一覧、状態変化は`このArcで進むこと`、先に示す内容は仕込み欄を正本として、一方向の参照へ直してください。

2. **Arcで固定する範囲を超える細部。** [Episode一覧](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:220)には、荷車の車輪へ石を置く手順、扇風機から外す部品、追手が足元を見る位置、一回限りの台詞が固定されています。覚醒前の人助け、前世の修理経験、追手の隙、姉妹による個別の承認という成立に必要な出来事と結果はArcに保持し、具体的な作業・観察・台詞はEpisode Designへ移してください。

3. **episode-000の移動方向が矛盾。** [役割](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:223)では「稽古を急ぐ帰り道」、[入口](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:225)では「剣の稽古へ向かう途中」です。同一の時点として統一してください。

4. **episode-002の依頼人の所在と報酬受領時期が矛盾。** [役割](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:248)では報酬を受け取り宿で夕食を済ませた翌朝に、依頼人を宿場へ先行させます。依頼の完了・報酬・依頼人との別れ・救援を聞く順序を一つにつなげ、入口と出口も合わせてください。

5. **魔力知覚の消耗条件が確定設定と食い違う。** [episode-002](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:248)では「短時間」使っただけで頭痛が出ます。[魔術体系](/home/oshizo/repo/novel-20260929-maker-trial/canon/magic.md)は長時間の酷使による症状とし、Arc本文も成人後のカイは短く使うと定めています。使用時間・負荷と頭痛の関係を整合させてください。

6. **姉妹が循環と父の研究の類似を見抜く根拠が不足。** [episode-003](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:261)で姉妹に見えるのは主に武器付与と戦闘判断ですが、直後にエルナが循環を尋ね、二人が父の研究との類似を疑います。循環を知る根拠と、父から覚えた断片とのつながりを、このEpisodeの確定事項として追えるようにしてください。また[仕込み欄](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:135)の「カイが研究を知っている理由」は、カイが研究を知らず独学したという記述と食い違うため、姉妹が実際に疑っている点へ直してください。

7. **仕込みのEpisode割当が実際の内容と一致しない。** [「グレンの研究とカイの魔力循環」](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:130)はepisode-000を「実際に示すEpisode」に含めていますが、episode-000は研究にも循環にも触れません。episode-000で示す内容がないなら、この割当から外してください。

8. **記憶覚醒時の日本語が今世の記憶喪失を示唆する。** [episode-001](/home/oshizo/repo/novel-20260929-maker-trial/planning/arcs/arc-01.md:236)の「今世の家族の声、…も戻る」は、覚醒前から持っていた今世の記憶まで失っていたように読めます。思い浮かぶ、思い出して確かめる等、今世の人格と記憶が続いていることを直接示す文に直してください。同様に「自分の攻撃を使える機会へ変えた読み」など、何を評価したのか取りにくい表現も、人物の判断と結果が普通に読める文へ直してください。

現時点の`draft / pending`、`parent_version: 2`、設定提案と依存の「なし」は整合しています。今回の意味変更を確定する際は、Arc自身の`version: 5`を最終成果物で**6へ一度だけ**増やし、Arcの作者確認待ちとして`draft / pending`を維持してください。下流Episode Designと執筆指示は未作成です。
