---
name: bidandscale-set-up-an-account
description: Take a Google Ads account from nothing to synced and targeted in Bid & Scale — connect Google Ads, import the account, wait for the first sync, and set spend and efficiency targets. Use when the user wants to add an account or nothing is connected yet.
---

# Set up an account

1. **Check the state.** `gads_get_connection_status`. If Google Ads is
   connected and accounts are imported, summarize and skip ahead.
2. **Connect (the user does this).** If it is not connected, give the user the
   `settingsUrl` from that result and ask them to open it in a browser where
   they are signed in to Bid & Scale and connect Google Ads. Google's consent
   screen cannot be completed in chat. Never ask for or accept a token or
   password. When they say it is done, re-check `gads_get_connection_status`.
3. **Pick accounts.** `gads_discover_ad_accounts` lists every account the
   connection reaches: name, customer id, currency, manager or not, already
   imported or not. Ask which to import.
4. **Import.** `gads_import_ad_account` for each choice. For an account under a
   manager (MCC), pass the `loginCustomerId` exactly as discovery returned it.
5. **Wait for data.** Poll `gads_get_sync_status` with the new `projectId`
   until the first sync completes. Tell the user data lands within minutes.
6. **Set targets.** Ask for a daily spend goal, a ROAS floor and an acceptable
   CPA band, then `gads_set_optimization_targets`. Explain that these drive the
   optimization plan and the bounds of what may apply without review.

Done is an imported account, a completed sync, and targets set. Suggest an
audit (`audit-an-account`) next.
