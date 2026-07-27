---
name: Apply for an Imprint card or loan
description: Start a co-branded credit card or loan application by creating a customer session, handing the client_secret to the Imprint SDK, and reconciling the result server-side.
api: openapi/imprint-openapi-original.yml
operations: [createSession, getSession, getPaymentMethod, linkCustomerAccount]
---

# Apply for an Imprint card or loan

Authenticate every server call with your environment-specific API key as the HTTP
Basic username (empty password), or as `Authorization: Bearer <api_key>`. Sandbox
base URL is `https://dev.sbx.imprint.co`; production is `https://dev.imprint.co`.
For multi-product partnerships add `x-imprint-merchant-key: <key>`.

## Steps

1. **Create a customer session** — `createSession` (`POST /v2/customer_sessions`).
   Optionally attach customer/transaction history. The response returns a
   `client_secret`; keep it confidential and pass it to the SDK.
2. **Render the application** — initialize the Web (`apply.imprint.co/imprintsdk.js`),
   iOS, or Android SDK with the `client_secret` and environment. The cardholder
   completes the flow (typically under two minutes).
3. **Handle the SDK outcome** — the SDK returns `OFFER_ACCEPTED`, `REJECTED`,
   `IN_PROGRESS`, or `ERROR`. Treat this as client-side only.
4. **Reconcile server-side** — the authoritative result arrives via the
   `Application` webhook (`hookApplicationStatus`). Verify the
   `X-IMPRINT-HMAC-SIGNATURE` header (HMAC-SHA256 over `<timestamp>.<body>`)
   before trusting it. Fetch the issued instrument with `getPaymentMethod`
   (`GET /v2/payment_methods/{payment_method_id}`).
5. **Link accounts (optional)** — `linkCustomerAccount`
   (`POST /v2/customers/{customer_id}/link`) ties the Imprint customer to your
   partner customer id.

## Rules

- Sandbox OTP is always `888888`. Drive application outcomes with a case-sensitive
  last name: `approve` → APPROVED, `review`/`frozen` → ACTION_REQUIRED, anything
  else → REJECTED.
- Errors use `{ "error": { "type", "message", "param" } }` (not RFC 9457).
- No documented idempotency key — do not blindly retry `createSession`.
