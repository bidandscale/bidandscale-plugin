---
description: Work the optimization plan — scale toward target while holding ROAS/CPA
---

Run an optimization pass on the account (argument, or `gads_list_ad_accounts` to resolve which one).

Call `gads_get_optimization_plan` first and work the ranked feed top-down: quote pacing vs the daily-spend goal and CPA vs the band, fix signal quality before moving money, and use the matching `gads_propose_*` tool for each action the user approves pursuing. State clearly which proposals auto-applied and which queued for review (check `mode` + `policyReasons`).

$ARGUMENTS
