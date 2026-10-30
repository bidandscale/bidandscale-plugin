---
description: Review the pending action queue and approve/reject per the user's verdicts
---

Review the approval queue with the user.

1. `gads_list_pending_actions` (status "pending"). If empty, say so and stop.
2. Present each action: what it does (summary), risk tier, policy reasons, and your own one-line take on whether it looks sound given the account's targets.
3. Ask the user for a verdict on each (or "approve all the safe-looking ones" style instructions — but confirm the exact list before acting).
4. Apply their verdicts strictly with `gads_approve_action` / `gads_reject_action` — never approve anything they didn't explicitly rule on.
5. Summarize: what's now applying, what was rejected, what's still pending.
