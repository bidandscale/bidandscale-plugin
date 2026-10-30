---
description: Prioritized account health sweep — find the highest-impact problems
---

Run a full account audit. If the argument names an account, match it via `gads_list_ad_accounts`; otherwise ask which account.

Run the audit sweep. Ground the sweep in `gads_get_optimization_plan` and `gads_get_account_overview` first, then work through conversion tracking, wasted spend (search terms), quality score, ad strength, and structure. End with a ranked list of the highest-impact problems, each with the concrete tool call that fixes it — propose fixes only after the user agrees.

$ARGUMENTS
