---
name: bidandscale-review-approvals
description: Walk the user through Bid & Scale's queue of pending Google Ads changes and apply their verdict on each one. Use when the user asks what is waiting for approval, wants to approve or reject changes, or wants to undo an applied change.
---

# Review approvals

Resolve the account with `gads_list_ad_accounts` and pass its returned
`projectId` on each account-scoped call.

1. `gads_list_pending_actions`. If the queue is empty, say so and stop.
2. Present each action: what it changes (its summary), the approval decision, the
   policy reasons it queued, and a one-line view of whether it looks sound
   against the account's targets.
3. Ask for a verdict on each. If the user says something like "approve the
   safe ones", list exactly which ones you take that to mean and confirm
   before acting.
4. Apply only what the user ruled on: `gads_approve_action` or
   `gads_reject_action`, one call per action. Approval queues a real change
   to the live account with no further human review. Check the returned result and action history;
   queued or failed does not mean applied.
5. Summarize what is now applying, what was rejected, and what is still
   pending.

To undo an applied change, find it in `gads_list_action_history` and call
`gads_revert_action` only on the user's explicit decision. Approving needs an
active subscription; if it is refused for billing, say so and point the user to
the app. Rejecting requires the approval grant but does not require a paid subscription.
