---
name: Authenticate to the Agicap Public API
description: Obtain an OAuth2 client-credentials access token and discover the entities it can act on.
api: openapi/agicap-auth-v1-openapi.json
operations: [Organizations_ListOrganizations, Organizations_ListEntities]
---

# Authenticate to the Agicap Public API

Every Agicap Public API call is authorized with an OAuth2 **client-credentials** bearer token.

## Steps

1. **Generate API credentials** in the Agicap app: Organization settings > Advanced settings > API settings > Generate new credentials. Store the `client_id` and `client_secret`.
2. **Request an access token** from the authorization server:
   - `POST https://myaccount.agicap.com/connect/token`
   - body: `grant_type=client_credentials`, `client_id`, `client_secret`, `scope=agicap:public-api`
   - Payments and supplier operations may additionally require fine-grained scopes such as `public-api:import_payment_files` or `public-api:manage-suppliers` (see `scopes/agicap-scopes.yml`).
3. **Send the token** on every request as `Authorization: Bearer {access_token}` against `https://api.agicap.com`.
4. **Discover your entities** — most business calls are entity-scoped. List the organizations and entities the token can access with `Organizations_ListOrganizations` then `Organizations_ListEntities`, and reuse those ids in downstream path parameters.

## Rules
- Token format is opaque; treat it as a secret and refresh when it expires.
- Errors are returned as RFC 9457 `application/problem+json` (see `errors/agicap-problem-types.yml`).
- A `429` means you have hit the rate limit — back off and retry.
