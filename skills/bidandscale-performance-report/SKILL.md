---
name: bidandscale-performance-report
description: Write a read-only performance report for a Google Ads account in Bid & Scale — headline numbers against targets, what changed and why, risks, and the next moves. Use when the user asks how an account is doing, for a weekly or monthly summary, or for a report to share.
---

# Performance report

Read-only: propose nothing in a report.

1. Resolve the account (`gads_list_ad_accounts`) and check
   `gads_get_sync_status`. Quote any freshness warning at the top.
2. `gads_get_account_overview` for the period the user asked for (30 days if
   they did not say), and `gads_get_optimization_plan` for the targets.
3. `gads_list_campaigns` for the split by campaign; top movers from
   `gads_list_keywords` and `gads_get_search_terms`.
4. Cross-check causes: `gads_get_change_history` for edits in Google Ads, and
   `gads_list_action_history` for changes made through Bid & Scale.

Structure it as: headline numbers against targets (spend, conversions, CPA,
ROAS) → what changed and why → risks → the three highest-impact next moves.
Name the period and currency. Never invent a number the tools did not return.
