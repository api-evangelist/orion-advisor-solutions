---
name: Authenticate and read Orion portfolio data
description: Exchange Orion service credentials for a Session token, then list the
  clients, accounts, and registrations the user has access to.
api: openapi/orion-advisor-solutions-orion-connect-openapi.json
operations: [Token_GetTokenAsync, Clients_GetAsync, Clients_GetEntityByKeyAsync, Accounts_GetAccountsAsync, Accounts_GetEntityByKeyAsync, Registrations_GetAsync]
generated: '2026-09-10'
method: generated
---

# Authenticate and read Orion portfolio data

Base URL: `https://api.orionadvisor.com/api/v1` (test: `https://testapi.orionadvisor.com/api/v1`).
Credentials are provisioned by Orion on request (SME-Integrations@orion.com) — there is no self-serve signup.

1. **Get a token** — `GET /v1/Security/Token` (`Token_GetTokenAsync`) with header
   `Authorization: Basic {base64(uid:pwd)}` using your Orion service-level credentials.
   The response carries the auth token.
2. **Call with the Session scheme** — every subsequent request sends
   `Authorization: Session {token}`. Tokens expire; on a `401` re-run step 1.
3. **List clients** — `GET /v1/Portfolio/Clients` (`Clients_GetAsync`) returns clients the
   authenticated user can access. Fetch one with `GET /v1/Portfolio/Clients/{key}`
   (`Clients_GetEntityByKeyAsync`).
4. **List accounts** — `GET /v1/Portfolio/Accounts` (`Accounts_GetAccountsAsync`); a single
   account is `GET /v1/Portfolio/Accounts/{key}` (`Accounts_GetEntityByKeyAsync`). Accounts
   link to registrations via `registrationId`; list registrations with
   `GET /v1/Portfolio/Registrations` (`Registrations_GetAsync`).

Rules: no idempotency mechanism exists — treat every write as at-most-once and check state
before retrying. Errors are prose-described (no problem+json); a `400` means the request was
invalid, a `404` means the entity does not exist or is not visible to your access rights.
