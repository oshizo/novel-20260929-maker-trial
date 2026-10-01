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
