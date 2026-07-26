---
name: Fetch and execute a Dialect Blink
description: Retrieve an interactive Blink and turn a chosen action into a ready-to-sign Solana transaction, including multi-step chained actions.
api: openapi/dialect-blinks-openapi.json
operations: [getFullBlink, executeBlinkAction, executeChainedAction, generateBlinkPreview, getBlinkDataTable]
---

# Fetch and execute a Dialect Blink

A Blink (Blockchain Link) exposes onchain actions that resolve to a ready-to-sign transaction, so a user can transact without leaving your app.

## Auth
- Base URL: `https://api.dial.to`
- Header: `x-blink-client-key: <your client key>`.

## Steps
1. `GET /v1/blink` (`getFullBlink`) to retrieve the complete Blink with its metadata and available `LinkedAction`s.
   - Use `GET /v1/blink-preview` (`generateBlinkPreview`) for a marketing-optimized preview, or `GET /v1/blink-data-table` (`getBlinkDataTable`) for key stats (APY, balance, etc.).
2. Choose an action, then `POST /v1/blink` (`executeBlinkAction`) with the action inputs. The response is a `TransactionResponse` containing a base64 transaction to sign with the user's Solana wallet.
3. If the action returns a `NextActionLink`, continue the flow with `POST /v1/blink/chain` (`executeChainedAction`) until the workflow completes (`CompletedAction`).

## Rules
- Sign and submit the returned transaction client-side with the user's wallet; never sign server-side on the user's behalf.
- Upstream/protocol failures surface as `502` with an `ActionError { message }` (errors/dialect-problem-types.yml) — surface the message, do not silently retry.
- Blink lists are fetched via `getBlinkList` and may return `401`/`404`.
