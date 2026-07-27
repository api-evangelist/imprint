---
name: Reconcile Imprint transactions
description: Track a purchase from transaction intent through settled transaction, reconciling against real-time transaction webhooks.
api: openapi/imprint-openapi-original.yml
operations: [createTransactionIntent, getTransactionIntent, searchTransactionIntents, updateTransactionIntent, getTransaction, getTransactions]
---

# Reconcile Imprint transactions

Authenticate with your API key (HTTP Basic username or Bearer). Amounts are
integers in the currency minor unit (`1000` = $10.00 USD); dates are RFC 3339.

## Steps

1. **Create a transaction intent** — `createTransactionIntent`
   (`POST /v2/transaction_intents`) to represent a pending transaction and track
   its lifecycle before it finalizes.
2. **Update if needed** — `updateTransactionIntent`
   (`POST /v2/transaction_intents/{intent_id}`).
3. **React to events** — the `Transaction` webhook (`hookTransactionStatus`)
   fires as the transaction is created and progresses. Verify the
   `X-IMPRINT-HMAC-SIGNATURE` before processing.
4. **Fetch the settled transaction** — `getTransaction`
   (`GET /v2/transactions/{transaction_id}`); the intent links to it via
   `intent_id`.
5. **Reconcile per customer** — `getTransactions`
   (`GET /v2/customers/{customer_id}/transactions`) and
   `searchTransactionIntents` (`GET /v2/transaction_intents`). Page with `limit`
   + `starting_after`; read `data`, `has_more`, `total`.

## Rules

- In sandbox, drive events with `simulateTransactionEvent`
  (`POST /v2/simulate_transaction_event`) — sandbox only (403 in production).
- 404s carry a typed error (`TRANSACTION_FOUND_ERROR`,
  `TRANSACTION_INTENT_FOUND_ERROR`).
