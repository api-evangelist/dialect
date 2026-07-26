---
name: Query Dialect markets and wallet positions
description: Read real-time DeFi lending/yield markets and a wallet's positions and PnL across Solana protocols via the Markets & Positions API.
api: openapi/dialect-markets-openapi.json
operations:
  - markets-positions.marketsV0Api.listMarkets
  - markets-positions.marketsV0Api.listMarketsGroupedByType
  - markets-positions.positionsV0Api.positionsByOwners
  - markets-positions.positionsV0Api.positionsPnl
---

# Query Dialect markets and wallet positions

Fetch lending rates, TVL, market history, and a wallet's positions/PnL across Solana's top DeFi protocols (Jupiter, Kamino, Lulo, MarginFi).

## Auth
- Base URL: `https://markets.dial.to/api`
- Header: `x-dialect-client-key: <key>` — the public demo key `pk_demo` works for read access (sandbox/dialect-sandbox.yml).

## Steps
1. `GET /v0/markets` (`listMarkets`) — list markets; filter with `provider`, `type`, `asset`, `marketIds`, and paginate with `cursor` + `limit`.
   - `GET /v0/marketsByType` (`listMarketsGroupedByType`) groups them by market type (lending / yield / loop / perpetual).
   - `GET /v0/markets/history` for historical series (`startTime`, `endTime`, `resolution`).
2. `GET /v0/positions/owners` (`positionsByOwners`) with `walletAddresses` to fetch a wallet's positions joined to their markets.
3. `GET /v0/positions/pnl` (`positionsPnl`) for realized/unrealized profit-and-loss on those positions.

## Rules
- All endpoints are `GET` and return `200`; paginate with `cursor`/`limit` (conventions/dialect-conventions.yml).
- Markets API is beta (`v0`) — treat schemas as evolving.
