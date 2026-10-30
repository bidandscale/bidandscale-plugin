---
description: Read-only performance narrative for the last period
---

Write a performance report for the account (argument, or `gads_list_ad_accounts` to resolve which one). Read-only — do not propose changes.

Pull `gads_get_account_overview` (default 30 days, or the period the user asked for), `gads_list_campaigns`, top movers from `gads_list_keywords` and `gads_get_search_terms`, and `gads_get_funnel` if connected. Compare spend/conversions/CPA/ROAS against the targets from `gads_get_optimization_plan`. Structure: headline numbers → what changed and why (cross-check `gads_get_change_history`) → risks → the 3 highest-impact next moves.

$ARGUMENTS
