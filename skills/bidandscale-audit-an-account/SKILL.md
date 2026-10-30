---
name: bidandscale-audit-an-account
description: Run a prioritized health check of a Google Ads account in Bid & Scale — data freshness, conversion tracking, wasted spend, keyword and ad quality, structure — and end with a ranked list of fixes. Use when the user asks what is wrong with an account, for an audit, or where money is being wasted.
---

# Audit an account

Read-only until the user picks fixes. Resolve the account with
`gads_list_ad_accounts` even when the user supplies its name; use the returned projectId.

1. **Freshness first.** `gads_get_sync_status`. If the latest run failed or the
   data is more than a day old, say so; offer `gads_trigger_sync` and wait
   before drawing conclusions. Quote any `dataFreshness.warning` the reads
   return.
2. **Ground it.** `gads_get_optimization_plan` (targets, pacing, ranked
   actions) and `gads_get_account_overview` for the period. The latest
   recurring audit is `gads_get_audit_report`.
3. **Tracking.** `gads_get_conversion_actions` and `gads_get_tracking_health`:
   is a primary conversion firing, and is value being recorded? Broken tracking
   outranks everything else, because every other number depends on it.
4. **Wasted spend.** `gads_get_search_terms` for terms with spend and no
   conversions, and keywords whose CPA is far outside the band
   (`gads_list_keywords`).
5. **Quality.** Quality Score components from `gads_list_keywords`; ad
   strength from `gads_list_ads`; underperforming assets from
   `gads_get_asset_performance`.
6. **Structure.** Campaigns pacing over or under budget, and mixed intent in
   one ad group (`gads_list_campaigns`, `gads_list_ad_groups`).

End with the five highest-impact problems, ranked, each with its cost in money
and the `gads_propose_*` call that would fix it. Propose nothing until the user
chooses; then follow `optimize-an-account`.
