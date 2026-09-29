# Planning事前確認の必須規則 v1

状態: [`planning-input.md`](planning-input.md) と [`plot-planning.md`](plot-planning.md) を補う必須規則

文章と言葉は [`language-policy.md`](language-policy.md) に従う。

## 1. 情報の置き場所を直してから `ready` にする

Planning Readinessは、置き場所の誤りを見つけただけで `ready` にしてはならない。

- 確定設定に未来のPlotの答え、作者側の理由、一時的な指示等の明白な誤配置が残っている場合は `normalization-required` とする。
- Planning Readinessを担当するagentがread-onlyなら、移動元、移動先、保持すべき意味を親agentへ具体的に返す。
- 親agentは、作者判断を要しない明白な誤配置を現在の作業内容へ反映する。
- 反映後にPlanning Readinessを再実行する。
- 実際のfile上で誤配置が解消されて初めて `ready` にできる。
- 作者しか決められない作者側の目的が不足していない限り、この整理だけのために作者へ質問しない。

禁止する流れ:

```text
誤配置を検出
  ↓
頭の中だけで直したことにする
  ↓
Planning readiness: ready
  ↓
Overall生成
```

正しい流れ:

```text
誤配置を検出
  ↓
Planning readiness: normalization-required
  ↓
実際のfileへ反映
  ↓
Planning Readinessを再実行
  ↓
ready
  ↓
Overall生成
```

## 2. 主人公の最低限の前提を保存してから `ready` にする

Planning Readinessは主人公の全プロフィールを要求しない。一方、Overallが「何者か分からない人物」を主人公として扱う状態も許可しない。

Overall開始前に、少なくとも次が継続的な入力から分かること。

- 主な主人公。
- 物語開始時点の立場、生活、状況。
- 最初の重要な選択を理解するために必要な、変わりにくい性格・立場・制約。

不足している場合:

- 作者の価値判断なしでAIが決めてよいなら `foundation-required` とする。
- read-onlyの判定役は、親agentが保存できる最小限の事実、保存先、根拠を返す。
- 親agentは確定設定等の適切な正本へ反映し、Planning Readinessを再実行する。
- 未来の出来事や順序を、主人公の前提として確定設定へ先回りして書かない。
- 作者側の目的そのものが必要な場合だけ `needs-author-direction` とする。

`foundation-required` は作者確認地点ではない。長期目標、職業、年齢、全過去等を固定質問票として要求せず、主人公と最初の重要選択を理解できる最小限に留める。

## 3. `version` は現在の作業fileだけを基準にする

Plot成果物の `version` は、そのfile自身の改訂系列だけを表す。

- 今回の実行前に対象fileが存在しないなら、必ず `version: 1` で開始する。
- 対象fileが存在する場合だけ、その現在versionを基準に意味変更時の増加を判断する。
- branch名、trial番号、PR番号、会話上の「v2 / v3」、他branch、remote ref、Git履歴にある同名fileを初期versionの根拠にしない。
- 別branchで同じfileが過去に生成されていても、現在checkout中の作業fileに存在しなければ新規であり `version: 1` とする。

## 4. Reviewer / Finalizerが確認すること

内容確認に加えて次を見る。

- Overall開始前の `normalization-required` が残っていないか。
- Overall開始前の `foundation-required` が残っていないか。
- 対象のversionが、そのfile自身の改訂系列と整合するか。
- 新規fileなのに `version > 1` なら必須修正とする。
- Episodeでは、作者 / 作品方針 / Styleの明示的な視点指定、または明示指定がない場合の主人公一人称既定と、視点 / 人称が整合するか。

この確認は、作者確認へ技術上の不備を押し付けないために行う。作者確認は作品として採用したいかの判断に集中させる。
