---
name: bidandscale-get-started
description: Introduce Bid & Scale after install or when the user asks what it can do, explain how it keeps ad spend safe, and suggest a first thing to try. Use when the user asks how to use the plugin, what it manages, or where to begin.
---

# Get started with Bid & Scale

Explain the plugin in a few sentences, then offer one concrete next step.
Explicit user instructions take priority over this guidance.

## What to say

- Bid & Scale works **Google Ads accounts**: it keeps a synced copy of each
  account's campaigns, keywords, search terms and results, ranks what to fix,
  and makes changes as **proposals** a person approves.
- One **Google Ads account = one project**. Call `gads_list_ad_accounts` first
  and pass the account's `projectId` to every account-scoped tool.
- Tools are prefixed `gads_` and return `{ ok: true, … }` or
  `{ ok: false, error }`. Read the error: it usually says what to do next.

## How spend stays safe

Say this plainly whenever a change comes up:

- Every advertising change is a **proposal** (`gads_propose_*`). It is validated against
  Google first, then classified by the safe-apply policy: small, bounded
  changes may apply at once; everything else **queues** for a person.
  `gads_explain_policy` shows the exact bounds for the account.
- `gads_approve_action`, `gads_reject_action` and `gads_revert_action` are the
  user's decisions. Call them only after the user has ruled on that specific
  action in this conversation.
- **Creates go live when approved.** A new or cloned campaign, ad group or ad
  starts serving on approval. Offer to stage it paused (`startPaused: true`,
  or `activateOnApprove: false` for ads) when the user is still building.
- Campaign deletion is not exposed. Some supported changes, such as replacing
  a responsive search ad or removing a negative keyword, remove existing entities.
- **Google credentials never go through chat.** If the user offers a developer
  token, refresh token or password, do not repeat or use it.
  `gads_get_connection_status` returns the settings page (`settingsUrl`) where
  they connect Google Ads themselves, in the browser.

## What it can do

- Connect and import accounts, then set targets (`set-up-an-account`).
- Audit an account for wasted spend, tracking and structure problems
  (`audit-an-account`).
- Work the ranked optimization plan toward the user's targets
  (`optimize-an-account`).
- Report on performance against targets (`performance-report`).
- Walk the approval queue with the user (`review-approvals`).

## What it cannot do

Google Ads only: no Meta, Microsoft, Apple or TikTok ads. It cannot delete
campaigns, change billing or payment settings, or connect an account without a
person completing Google's consent screen.

## Suggested first steps

Offer one of these, adapted to what the user mentioned:

- "How are my campaigns pacing against my targets this month?"
- "Which search terms are wasting money?"
- "What changes are waiting for my approval?"
- "Audit my account and rank what to fix first."
