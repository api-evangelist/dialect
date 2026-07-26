---
name: Send a Dialect alert
description: Send a real-time multi-channel notification to an app's wallet subscribers via the Dialect Alerts V2 API.
api: openapi/dialect-alerts-openapi.json
operations: [postV2ByAppIdSend, postV2ByAppIdSend-batch, getV2ByAppIdSubscribers, getV2ByAppIdTopics]
---

# Send a Dialect alert

Deliver a notification to one, several, or all subscribers of your app across PUSH, IN_APP, EMAIL, and TELEGRAM channels.

## Auth
- Base URL: `https://alerts-api.dial.to`
- Header: `x-dialect-api-key: <your app API key>` (issued in the Dialect dashboard).

## Steps
1. (Optional) List valid targets with `getV2ByAppIdSubscribers` and `getV2ByAppIdTopics` to confirm `appId`, subscribers, and `topicId`.
2. `POST /v2/{appId}/send` (`postV2ByAppIdSend`) with a JSON body:
   - `channels`: array of `PUSH` / `IN_APP` / `EMAIL` / `TELEGRAM`.
   - `message`: `{ title (1-60 chars), body (1-500 chars) }`.
   - `recipient`: single subscriber, list of subscribers, or all subscribers.
   - optional `topicId` (UUID), `image` (public URL), `actions` (≤3 label+url), `data`, `push`.
3. For fan-out to many recipients, use `postV2ByAppIdSend-batch`.

## Rules
- Success is `202 Accepted` (queued), not `200`.
- Handle `400` (validation), `401` (bad/missing API key), `403` (not permitted for this app).
- No idempotency key exists — do not blind-retry a non-error timeout; check delivery instead (conventions/dialect-conventions.yml).
- Errors follow the standard status-JSON envelope (errors/dialect-problem-types.yml).
