---
name: Subscribe to Agicap events with webhooks
description: Create an HMAC-signed webhook subscription, test it, and manage its lifecycle.
api: openapi/agicap-events-v1-openapi.json
operations: []
---

# Subscribe to Agicap events with webhooks

React to events raised across Agicap products by registering an HTTPS webhook endpoint.
(The events-v1 spec does not assign operationIds; operations are addressed by method + path.)

## Steps

1. **Authenticate** with scope `agicap:public-api` (webhook management additionally maps to the
   `webhooks:endpoints:manage` authorization-server scope).
2. **Create a webhook** — `POST /public/events/v1/webhooks` with your endpoint `url`, the
   `eventTypes` to subscribe to, and a `secret` (plain-text HMAC-SHA256 signing key, minimum 24 chars).
   Set `enabled: true` to skip the default disabled state.
3. **Send a test event** — `POST /public/events/v1/webhooks/{id}/send-example` with an `eventType`
   to receive a synthetic payload and confirm your receiver verifies the signature.
4. **Manage lifecycle** — `GET /public/events/v1/webhooks/{id}`, and enable/disable/delete via
   `/{id}/enable`, `/{id}/disable`, `DELETE /{id}`.

## Rules
- Verify every delivery's HMAC-SHA256 signature with your stored secret; rotate the secret via the dedicated rotate-secret endpoint.
- A webhook can be `disabled_for_errors` automatically if your endpoint keeps failing — monitor its `status`.
- This API is in **beta**.
