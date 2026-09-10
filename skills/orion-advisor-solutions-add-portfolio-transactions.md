---
name: Add portfolio transactions
description: Create verbose portfolio transactions in Orion and read them back for
  verification.
api: openapi/orion-advisor-solutions-orion-connect-openapi.json
operations: [Token_GetTokenAsync, Transactions_CreateVerboseTransactionAsync, Transactions_CreateVerboseTransactionsAsync, Transactions_GetVerboseListById, Transactions_GetListById]
generated: '2026-09-10'
method: generated
---

# Add portfolio transactions

Grounded in the Adding Transactions guide
(https://developers.orionadvisor.com/guides/adding-transactions/) and the Orion Connect
contract. Authenticate first (see the authenticate-and-list-portfolio skill).

1. **Create one transaction** — `POST /v1/Portfolio/Transactions/Verbose`
   (`Transactions_CreateVerboseTransactionAsync`) with the verbose transaction body.
2. **Create many** — `POST /v1/Portfolio/Transactions/Verbose/New`
   (`Transactions_CreateVerboseTransactionsAsync`) accepts a list.
3. **Verify** — read back with `POST /v1/Portfolio/Transactions/Verbose/List`
   (`Transactions_GetVerboseListById`) or `/v1/Portfolio/Transactions/List`
   (`Transactions_GetListById`) using the returned ids.

Rules: there is no idempotency key — if a create times out, read back (step 3) before
retrying, or you will double-post. A `400` response is a validation failure described in
prose. No documented reversal window exists for posted transactions; corrections go through
Orion's reconciliation workflows, so confirm inputs before writing.
