# Framework migrations

- kind: bootstrap
  from_revision: "none"
  to_revision: "9a9416c35eb14b8e77c85ccca5b95ad4adc05fb8"
  resolved_commit: "9a9416c35eb14b8e77c85ccca5b95ad4adc05fb8"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "none"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "9a9416c35eb14b8e77c85ccca5b95ad4adc05fb8"
  to_revision: "d4b6760ea97ea123fd3b71386011281d3b194a2e"
  resolved_commit: "d4b6760ea97ea123fd3b71386011281d3b194a2e"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "issue #5でOverall Planner、Arc Planner、Story Craft ChallengerのCodex用設定を新契約へ更新"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "d4b6760ea97ea123fd3b71386011281d3b194a2e"
  to_revision: "8090fbfee3da7dc7c5ae6daaf0b08196172debe4"
  resolved_commit: "8090fbfee3da7dc7c5ae6daaf0b08196172debe4"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "issue #7でPlanning kickoff templateをrun trace契約へ更新"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "8090fbfee3da7dc7c5ae6daaf0b08196172debe4"
  to_revision: "d057210d9f64a930260c51edd3efea7bc8d18d53"
  resolved_commit: "d057210d9f64a930260c51edd3efea7bc8d18d53"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "issue #9で冒頭導入セットの採否判定契約を反映"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "d057210d9f64a930260c51edd3efea7bc8d18d53"
  to_revision: "461c2178a5119e73db2455cddc417591da2efbfb"
  resolved_commit: "461c2178a5119e73db2455cddc417591da2efbfb"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "issue #10でCodexのStory Craft Regression設定とPlanning Issue templateをmaker #129相当へ更新"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "461c2178a5119e73db2455cddc417591da2efbfb"
  to_revision: "2c84fa517e29c707e59ca018834ab461e49e684a"
  resolved_commit: "2c84fa517e29c707e59ca018834ab461e49e684a"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "issue #10でCodex custom agentの10役割をmaker #131のGPT-6 Sol / Luna割当に更新"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "2c84fa517e29c707e59ca018834ab461e49e684a"
  to_revision: "dc7d0786437ce8280fcd9a7381dd65d1ea356726"
  resolved_commit: "dc7d0786437ce8280fcd9a7381dd65d1ea356726"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "none"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "dc7d0786437ce8280fcd9a7381dd65d1ea356726"
  to_revision: "a5abd3a47d5be2a571f2d46b1ca1e6493292aa92"
  resolved_commit: "a5abd3a47d5be2a571f2d46b1ca1e6493292aa92"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "Issue #12でarc-planner.tomlをmaker #137のtemplateから明示同期。GPT-6 Sol / Luna割当を維持"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "a5abd3a47d5be2a571f2d46b1ca1e6493292aa92"
  to_revision: "f7811f1a83204fe45f102536f59f1f54f98b2673"
  resolved_commit: "f7811f1a83204fe45f102536f59f1f54f98b2673"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "none"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "f7811f1a83204fe45f102536f59f1f54f98b2673"
  to_revision: "06f0d8ee07e1e2f2a01ee2e86530f764585cc048"
  resolved_commit: "06f0d8ee07e1e2f2a01ee2e86530f764585cc048"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "Issue #16: 固定maker merge commitの10役instructions、Planning入口、bootstrap由来AGENTS説明を明示同期。作品独自追記なし。Sol系TOMLのgpt-6-solは互換用既定値とし、spawn時primaryはgpt-6.1-sol。"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "06f0d8ee07e1e2f2a01ee2e86530f764585cc048"
  to_revision: "f4491ce08c3c3be8f092ed20f48587e89e80ec87"
  resolved_commit: "f4491ce08c3c3be8f092ed20f48587e89e80ec87"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "Issue #16継続: maker PR #151 merge commitから10役instructionsとPlanning入口を明示同期。bootstrap由来AGENTSを照合。作品独自追記なし。再判定はstory-craft.md §5の同じ解消条件を引き継ぐ。primaryと許可fallbackはcodex-model-policy.md。"
  unresolved_issue: "none"

- kind: framework-sync
  from_revision: "f4491ce08c3c3be8f092ed20f48587e89e80ec87"
  to_revision: "2e6aa815635cb9ea8c553ec0e4ad7dbab612038a"
  resolved_commit: "2e6aa815635cb9ea8c553ec0e4ad7dbab612038a"
  contract_version: 1
  applied_migration: "none"
  manual_decision: "none"
  unresolved_issue: "none"
