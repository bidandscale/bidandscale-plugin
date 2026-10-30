---
description: Use Bid & Scale from Claude Code — setup (connect, discover, import, sync), account analysis, optimization, proposal-based writes, and the approval queue. Use when the user asks about a Google Ads account managed in Bid & Scale or wants to set one up.
---

# Bid & Scale

This plugin connects Claude Code to your Bid & Scale instance via a remote
MCP server. Tools appear with the prefix `gads_*` and return a JSON envelope
`{ ok: true, … }` on success or `{ ok: false, error: "…" }` on failure — read
the error, it usually says exactly what to do next.

## Authorization: two credentials

**OAuth** (see Install below) is how a person connects, and the only thing a
claude.ai connector can use — there is no token to generate or copy. An OAuth
session is always workspace-scoped with full write access: every tool in this
reference is callable, across every account in the workspace.

**A `bsk_` API key** — the same key the REST API at `/api/v1` takes, minted
in Workspace → Settings → API — is the better credential for a headless agent.
Send it as `Authorization: Bearer bsk_…`. It carries what an OAuth session
cannot: a ranked scope (`read` < `write` < `admin`), the orthogonal
`canApprove` grant, an optional pinned or subsetted account, and an expiry.
Revoking it stops MCP and REST together. A key is an *agent* principal, so
nothing auto-applies — every proposal queues for review.

- Account-scoped tools take an optional `projectId` argument — call
  `gads_list_ad_accounts` first and pass the account's `projectId` on every
  account-scoped call. A key pinned to one account already has it.
- Setup tools (connect, discover, import) and the approval tools
  (`gads_approve_action`, `gads_reject_action`, `gads_revert_action`) are
  workspace-level and need no `projectId`.

## Setup — from zero to synced account

1. `gads_get_connection_status` — is Google Ads OAuth connected?
2. If not: the HUMAN opens its `settingsUrl` in a browser where they're
   logged into the app and connects Google Ads (the consent screen can't be done here).
3. `gads_discover_ad_accounts` — everything the connection can reach.
4. `gads_import_ad_account` — creates the project + enqueues a 90-day backfill.
5. `gads_get_sync_status` / `gads_trigger_sync` — watch or refresh the warehouse.
6. `gads_set_optimization_targets` — daily spend goal, ROAS floor, CPA band.
   Set these early: the optimization plan and the safe-apply policy key off them.

## Analysis (warehoused reads, fast)

`gads_get_account_overview`, `gads_list_campaigns`, `gads_list_ad_groups`,
`gads_list_keywords` (incl. Quality Score), `gads_list_ads`,
`gads_get_search_terms`, `gads_get_conversion_actions`, `gads_get_change_history`,
`gads_get_funnel`, and `gads_get_optimization_plan` — the ranked action feed
(scale / improve / protect / reallocate) that grounds every optimization
conversation. `gads_run_gaql_query` runs read-only GAQL against the live API
for anything not warehoused (device/geo/hour splits, today's numbers).
Expansion: `gads_discover_search_term_silos`, `gads_ideate_keyword_silos` (slow,
LLM + Keyword Planner).

## Writes — proposals under the safe-apply policy

`gads_propose_*` tools never mutate directly: each runs a `validate_only`
dry-run, then the safe-apply policy classifies it. Bounded, low-risk actions
(negatives, small exact keyword adds, keyword pauses, ≤30% budget moves on
healthy campaigns) **auto-apply**; everything else **queues** for review. The
result's `mode` and `policyReasons` say which happened and why.
`gads_explain_policy` explains the thresholds for the account.

## Approvals — the human decides, you execute

`gads_list_pending_actions` shows the queue. `gads_approve_action`,
`gads_reject_action`, and `gads_revert_action` are **operator decisions**:
only call them when the human has explicitly decided on that specific action
— present each pending action (summary, risk tier, policy reasons) and let
them rule. Approval applies a money-moving change with no further review.
`gads_list_action_history` is the audit trail; applied actions with a snapshot
can be undone with `gads_revert_action`.

## Recovering from errors

- `401 Unauthorized`: the credential expired or was revoked. On OAuth, run
  `/mcp` (Claude Code) or reconnect the connector (claude.ai) to re-authorize;
  on a `bsk_` key, mint a new one in Workspace → Settings → API.
- "No Google Ads account is in scope": pass `projectId`
  (from `gads_list_ad_accounts`).
- Auth errors from Google Ads itself: `gads_get_connection_status`, then
  reconnect at its `settingsUrl`.
