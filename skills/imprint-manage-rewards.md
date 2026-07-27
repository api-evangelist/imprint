---
name: Manage Imprint rewards
description: Read a customer's rewards balance, list and inspect rewards, and update pending rewards, reacting to reward webhooks.
api: openapi/imprint-openapi-original.yml
operations: [getRewardsBalance, getRewards, searchRewards, getReward, patchReward]
---

# Manage Imprint rewards

Authenticate with your API key (HTTP Basic username or Bearer). Amounts are in the
currency minor unit.

## Steps

1. **Read the balance** — `getRewardsBalance`
   (`GET /v2/customers/{customer_id}/rewards_balance`).
2. **List rewards** — `getRewards`
   (`GET /v2/customers/{customer_id}/rewards`, newest-updated first) or
   `searchRewards` (`GET /v2/rewards`). Page with `limit` + `starting_after`.
3. **Inspect one** — `getReward` (`GET /v2/rewards/{reward_id}`).
4. **Update a pending reward** — `patchReward`
   (`PATCH /v2/rewards/{reward_id}`) to change status or amount of pending
   rewards only.
5. **React to reward events** — the `Reward` webhook (`hookReward`) fires when a
   reward is created, updated, or made available. Verify
   `X-IMPRINT-HMAC-SIGNATURE` before processing.

## Rules

- In sandbox, trigger reward webhooks with `simulateReward`
  (`POST /v2/simulate_reward`) and `simulateStatementReward`
  (`POST /v2/simulate_statement_reward`) — sandbox only (403 in production); a
  409 means a simulation is already in progress.
- Only pending rewards can be updated via `patchReward`.
