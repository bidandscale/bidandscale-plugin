---
description: Connect Google Ads, import accounts, run the first sync, set optimization targets
---

Walk the user through setting up Bid & Scale end to end.

1. Call `gads_get_connection_status`. If Google Ads is already connected with imported accounts, summarize the state and skip ahead.
2. If not connected: give the user the `settingsUrl` from that result, and tell them to open it in a browser where they're logged into the app and connect Google Ads. Wait for them to confirm, then re-check `gads_get_connection_status`.
3. Call `gads_discover_ad_accounts` and present the reachable accounts (name, customer id, currency, manager or not, already imported or not).
4. Ask which account(s) to import, then `gads_import_ad_account` for each — pass `loginCustomerId` verbatim for MCC children.
5. Poll `gads_get_sync_status` (projectId from the import result) until the backfill finishes; tell the user data lands within minutes.
6. Ask for their optimization objective — target daily spend, ROAS floor (%), acceptable CPA band — and set it with `gads_set_optimization_targets`. Explain these drive the optimization plan and the auto-apply guardrails.
7. Finish with a one-paragraph summary of what's connected and a suggestion to run /bidandscale:audit once the sync completes.
