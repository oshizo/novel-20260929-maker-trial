# Story repository 契約

状態: frameworkと作品repositoryの所有境界の正本

文章と言葉の方針は [`language-policy.md`](language-policy.md) に従う。

## 1. 基本形

本番運用は原則として **1作品 = 1 Git repository** とする。

```text
oshizo/novel-maker
  共通framework

oshizo/<story-repo>
  1作品の正本
```

`novel-maker` に複数作品の確定設定、Planning、文体資料、現在状態、本文を同居させない。framework内に置く作品データは、framework自身のtest、fixture、再利用可能な実験に限る。

## 2. 何をどちらが所有するか

「framework側の正本」は共通形式・共通規約を意味し、「story repo側の正本」は実際の作品内容を意味する。

| 対象 | framework | story repo |
|---|---|---|
| `AGENTS.md` | framework開発時の規約 | 作品実行時の入口と作品固有指示 |
| `story.yaml` | schema、template、互換性規則 | 利用するframework revisionと作品設定 |
| 作品方針 | 入力契約とtemplate | `story-direction.md` が作品の方向性の正本 |
| 確定設定 | 共通の配置・扱い方 | 人物・世界・用語・成立済み事実の正本 |
| Overall / Arc / Episode Design | 共通契約とtemplate | その作品のPlot正本 |
| 執筆指示 | 生成規約とtemplate | 本文執筆担当へ渡す内容の正本 |
| Planning kickoff Issue template | 次範囲判定とCodex実行入口のtemplate | `.github/ISSUE_TEMPLATE/plan-next.md` のbootstrap snapshot |
| 文体資料 | 配置・利用規約 | 参照文、採用本文、`profile.md` |
| 現在状態 | 共通契約 | 現在状態と未回収事項 |
| 本文 | 配置・順序・出力規約 | 読者向け本文 |
| `plan`, `write-next`, `replan`, `check` 等 | 共通操作の意味 | 実行時入力と作品固有の差分 |
| framework version情報 | revision/tagとcontract versionの発行元 | `story.yaml` で利用revisionを固定 |

同じ事実をframeworkとstory repoの両方で正本にしない。

story repoへ同期したframework fileは実行用snapshotであり、共通ruleそのものの正本ではない。一方、そのsnapshotを使って生成した作品については、story commitに含まれるpinとsnapshotが再現性の基準になる。

## 3. framework revisionを固定する

story repoは最新 `main` へ暗黙依存しない。利用するtagまたは完全なcommit SHAを `story.yaml` に記録する。

```yaml
framework:
  repository: oshizo/novel-maker
  revision: <tag-or-full-commit-sha>
  resolved_commit: <full-commit-sha>
  contract_version: 1
```

- `revision` はtagまたは完全なcommit SHAとする。branch名だけを固定値として使わない。
- tagを使う場合も `resolved_commit` に完全なcommit SHAを保存する。
- `contract_version` はstory workspace形式の大きな互換性区分であり、revisionの代わりではない。
- framework側の更新だけで既存storyの挙動を変えない。
- revision更新は明示的なframework update / migrationとして行う。

## 4. runtime contractの同期

標準方式は、**framework revisionを固定し、実行に必要なruntime contractだけをstory repoへ同期する**方式とする。

```text
story-repo/
  AGENTS.md
  story.yaml
  .novel-maker/
    runtime/
      ... pinしたrevisionから同期した実行規約 ...
    sync.yaml
    migrations.md
```

同期対象は [`framework/runtime-manifest.json`](../framework/runtime-manifest.json) を唯一の正本とする。

`.novel-maker/sync.yaml` には少なくとも、同期元repository、要求revision、解決済みcommit、contract version、同期fileとhashを記録する。

`.novel-maker/migrations.md` にはframework更新履歴を短く残す。大量の自動logを保存せず、どのrevisionからどこへ移し、手動判断や未解決事項があったかだけを追えるようにする。

framework全文をstory repoへ複製する方式は標準にしない。開発用資料・過去実験まで作品contextへ混入するためである。

## 5. `novelctl` の責務

標準操作:

```bash
python scripts/novelctl.py init-story <story-path> [--revision <tag-or-full-sha>]
python scripts/novelctl.py framework-sync <story-path> --revision <tag-or-full-sha>
python scripts/novelctl.py check-story <story-path>
```

### `init-story`

- 未存在path、空directory、またはbootstrapで許可された最小fileだけを持つdirectoryを初期化する。
- Git repository内のrootで使う場合は `.git` を既存内容として数えない。
- 作品方針 / 確定設定を整えた後にCodexが次のPlanning範囲を自動判定できるよう、pinしたrevisionにPlanning kickoff templateがあれば `.github/ISSUE_TEMPLATE/plan-next.md` へ配置する。
- Planning kickoff templateはOverall未作成ならPlanning ReadinessからOverallへ、ready Overallがあれば最初の未作成 / stale Arcへ進むためのexecutor入口とする。作者確認待ちの `draft / pending` は上書きしない。
- 同じpinで既に初期化済みなら、再生成せず検証する。既存のIssue templateやstory固有内容を上書きしない。
- 別revisionへ変更したい場合は `framework-sync` を使う。

### `framework-sync`

- 同じ `contract_version` の範囲でruntime snapshotを更新する。
- `.codex/agents/` はbootstrap時のexecutor設定snapshotであり、`framework-sync` では暗黙更新しない。
- `.github/ISSUE_TEMPLATE/plan-next.md` もbootstrap時のexecutor入口snapshotであり、`framework-sync` では暗黙更新しない。
- story固有に変更したagent設定やIssue templateを上書きしない。
- contract versionが異なる場合は、自動で大規模migrationせず停止する。

### `check-story`

LLMを使わず、metadata、pin、runtime manifest、file hash、UTF-8、pinしたGit objectとの一致を検証する。

Planning kickoff templateはexecutor補助でありruntime contract本体ではないため、旧revision互換のため `check-story` の必須fileにはしない。

## 6. sibling repositoryは任意

ローカル開発では次の配置を許可する。

```text
repo/
  novel-maker/
  my-novel/
```

ただし `../novel-maker` は開発上の便利機能であり、story実行の必須依存にはしない。

- siblingがなくても、同期済みruntime contractだけで実行規約を読めること。
- siblingの現在branchや未commit変更を暗黙採用しないこと。
- 同期元として使う場合も `story.yaml` が固定したrevisionを基準にすること。

## 7. framework更新

framework更新は次を一つのreview可能な変更として扱う。

1. 移行先revisionを決める。
2. runtime contractの差分と作品固有ruleへの影響を確認する。
3. `.novel-maker/runtime/` と `.novel-maker/sync.yaml` を更新する。
4. 必要なstory成果物migrationを行う。
5. `story.yaml` のpinを更新する。
6. `check-story` 等で検証し、同じcommitまたはPRへ含める。

framework更新だけを理由に既存本文や確定設定を暗黙に書き換えない。bootstrap時のexecutor補助snapshotである `.codex/agents/` やPlanning kickoff Issue templateも暗黙更新しない。

## 8. story固有rule

共通ruleはframework、作品固有ruleはstory repoが所有する。

story固有ruleはstory repoの `AGENTS.md` または参照先fileへ置く。同期済みruntime fileを直接編集して、framework由来ruleと作品固有ruleを混同しない。

作者から得た継続的な判断は、意味に応じて 作品方針 / 確定設定 / Planning / 文体資料 / 現在状態 へ置く。置き場所のない横断的判断だけ `state/decisions.md` を使う。

## 9. Issue / PRの担当範囲

`novel-maker` で扱うもの:

- 共通成果物・共通操作の契約
- 再利用可能なPlanner / 本文執筆担当 / Reviewerの規則
- story template、bootstrap、sync、migration tooling
- Planning kickoff Issue templateのような再利用可能なexecutor入口
- framework自身のfixtureと横断的な検証

story repoで扱うもの:

- 作品固有のOverall / Arc / Episode Design
- 本文執筆
- 作品固有のreview / revision / redirect
- 作品方針、確定設定、文体資料、現在状態の内容
- framework更新をその作品へ適用する変更

共通契約の問題は `novel-maker` で直し、各作品への採用は別の明示更新として扱う。

## 10. 再現可能とみなす条件

story repoのあるcommitについて、次を満たせばframework依存を追跡できる。

- `story.yaml` にframework repository、revision、contract versionがある。
- 対応するruntime contractがstory repoにGit管理されている。
- sync metadataから同期対象とhashを確認できる。
- story固有ruleがruntime snapshotと区別されている。
- sibling checkoutや会話履歴なしでも操作規約を発見できる。
