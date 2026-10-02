# Codex model 選択方針

状態: Codexでstandard pipelineを実行するときのmodel選択とfallbackの正本

この文書は作品内容の契約ではなく、Codex executorで各roleを起動するときの運用契約である。Plot Planningの役割分離、入力境界、Story Craft判定基準は他のruntime契約を変更しない。

## 1. 基本方針

高品質な判断・reviewを担当するSol系roleは、standard pipelineでは **`gpt-6.1-sol` をprimary** として起動する。

上位Plotからの具体化や執筆指示生成を担当するLuna系roleは、従来どおり `gpt-6-luna` を使う。

| role | primary model | reasoning |
| --- | --- | --- |
| `planning_readiness` | `gpt-6.1-sol` | high |
| `overall_planner` | `gpt-6.1-sol` | high |
| `arc_planner` | `gpt-6-luna` | medium |
| `episode_planner` | `gpt-6-luna` | medium |
| `story_craft_challenger` | `gpt-6.1-sol` | high |
| `plot_reviewer` | `gpt-6.1-sol` | high |
| `plot_finalizer` | `gpt-6.1-sol` | high |
| `story_craft_regression` | `gpt-6.1-sol` | high |
| `writer_brief_generator` | `gpt-6-luna` | medium |
| `writer_brief_reviewer` | `gpt-6.1-sol` | high |

Codexがsubagent生成時のmodel指定を受け付ける場合、Sol系roleはcustom agentに保存されたmodel値へ暗黙に任せず、**spawn時に `gpt-6.1-sol` を明示する**。

既存の `.codex/agents/*.toml` に `model = "gpt-6-sol"` が残っているstory repoでも、standard pipelineのprimaryはこの文書のspawn時指定を優先する。agent TOMLの `gpt-6-sol` は、古いstory repoとの互換と下記fallbackの安全な既定値として扱う。

## 2. 許可するmodel fallback

`gpt-6.1-sol` をprimaryとして起動したSol系roleについてだけ、次の1段fallbackを許可する。

```text
gpt-6.1-sol
  ↓ primary modelが実行環境で利用できず起動不能
同じcustom role / 同じinstructions / 同じreasoning / 同じ入力境界で gpt-6-sol
```

fallbackは **同じstageにつき最大1回** とする。

fallbackしてよいのは、primary modelそのものを使えないことが起動時に確認できた場合だけである。例:

- modelがunsupportedと返る。
- modelへのアクセスがまだ有効でない、またはrollout対象外と返る。
- executorが当該modelを利用不可として起動を拒否する。

次はfallback理由にしない。

- Story Craftが `WEAK / FAIL` を返した。
- Technical Reviewerが修正を要求した。
- role固有の正式出力契約を満たさなかった。
- 実行途中で一般的な処理エラーが起きた。
- primaryの回答品質が気に入らない。
- 親agentが自分で代行したい。

fallback時もroleを変えない。`story_craft_challenger-fallback` のような別roleは作らず、同じcustom roleをmodel override `gpt-6-sol` で新しい実行として起動する。

`gpt-6-sol` でも起動できない場合、そのstageとrunは `blocked` にする。Lunaや他modelへさらにfallbackしない。

## 3. fail-closedとの関係

standard pipelineのfail-closedは維持する。ただし、**本書で明示した `gpt-6.1-sol -> gpt-6-sol` の1段fallbackだけは、想定内のmodel fallbackとしてstandard pipeline完了を妨げない。**

次は引き続きfail-closedとする。

- 別roleへの自動切替。
- 親agentによるPlanner / Challenger / Reviewer / Finalizer / Regressionの代行。
- `gpt-6-sol` 以外へのSol系fallback。
- Luna系roleの別model fallback。
- model起動失敗以外を理由にしたmodel切替。
- fallback後もrole固有の正式出力契約を満たせない場合。

## 4. run trace

各stageではprimaryと実際に完了したmodelを区別する。

fallbackなし:

```json
{
  "configured_agent": "story_craft_challenger",
  "configured_model": "gpt-6.1-sol",
  "actual_agent": "story_craft_challenger",
  "actual_model": "gpt-6.1-sol",
  "model_fallback": null
}
```

fallbackあり:

```json
{
  "configured_agent": "story_craft_challenger",
  "configured_model": "gpt-6.1-sol",
  "actual_agent": "story_craft_challenger",
  "actual_model": "gpt-6-sol",
  "model_fallback": {
    "from": "gpt-6.1-sol",
    "to": "gpt-6-sol",
    "reason": "primary model unavailable at startup"
  }
}
```

`reason` はexecutorが返した事実を短く要約し、推測しない。

run summaryの `model_fallback_count` は、想定内fallbackを実際に使ったstage数とする。fallbackしなければ `0`。

## 5. 既存story repo

`framework-sync` は `.codex/agents/` を更新しない。このため既存story repoでは、次のどちらでもよい。

1. Sol系agent TOML自体を `gpt-6.1-sol` へ更新する。
2. agent TOMLは `gpt-6-sol` のまま保持し、standard pipeline起動時に本書どおり `gpt-6.1-sol` を明示model overrideする。

次回実行から確実に新方針を使う必要がある場合は、2を使えば既存agent snapshotの一括更新を待たずに切り替えられる。