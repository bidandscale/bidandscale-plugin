---
name: bidandscale-optimize-an-account
description: Work a Google Ads account's ranked optimization plan in Bid & Scale toward the user's spend and efficiency targets, turning each agreed step into a proposal. Use when the user wants to improve results, scale spend, cut waste, or act on an audit.
---

# Optimize an account

Resolve the account with `gads_list_ad_accounts` and pass its returned
`projectId` on each account-scoped call.

1. **Check the data.** `gads_get_sync_status`; do not act on stale or failed
   syncs. Relay any `dataFreshness.warning`.
2. **Read the plan.** `gads_get_optimization_plan`. Quote pacing against the
   daily spend goal and CPA against the band before recommending anything.
3. **Fix signal before money.** Tracking problems and wasted search terms come
   before budget or bid moves: negatives
   (`gads_propose_add_negative_keywords`), pausing a losing keyword
   (`gads_propose_keyword_status`).
4. **Then move money.** Budgets (`gads_propose_budget_change`), targets on the
   current bid strategy (`gads_propose_campaign_target`), keyword bids
   (`gads_propose_keyword_bid`). Switching bid strategy type
   (`gads_propose_campaign_bid_strategy`) resets learning; say so.
5. **Agree first.** Describe each change and why, and propose only the ones
   the user agrees to pursue.
6. **Report the outcome.** Each result carries `mode` and `policyReasons`:
   say which changes applied at once and which queued for review, and why.
   `gads_explain_policy` explains the bounds.

Creating campaigns, ad groups or ads goes live on approval. Stage with
`startPaused: true` (or `activateOnApprove: false` for responsive search ads)
whenever the build spans more than one approval.
