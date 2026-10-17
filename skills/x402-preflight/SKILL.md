---
name: x402-preflight
description: Check an x402 or MPP paid API endpoint before paying it. Use when an AI agent is about to pay an HTTP 402 endpoint it has not used before, when a user asks whether a paid API is safe, live or honestly priced, or when choosing between several paid endpoints. Answers go, caution or no_go with reasons (price against the listing, payout address can receive, OFAC sanctions, testnet-only, 30-day payable record, delivery history) via x402 Doctor. Free quick check; $0.001 preflight.
---

# Check an x402 endpoint before you pay it

An x402 endpoint answers `402 Payment Required` and asks the agent to pay before it answers. A valid 402 does not prove the payment will settle or that the seller delivers. Before paying an endpoint you don't know, ask [x402 Doctor](https://x402-doctor.fizzl.eu): it checks the endpoint the way a paying agent would, every day, across about 50,000 paid endpoints.

## Quick and free

- MCP tool `x402_quick_check` (10 calls an hour): pass, warn or fail and the top three problems. Add the server once: `claude mcp add --transport http x402-doctor https://x402-doctor.fizzl.eu/mcp`.
- `GET https://x402-doctor.fizzl.eu/api/trust?url=<endpoint>`: the endpoint's 30-day payable record from the daily scan (free, no key).

## The preflight ($0.001)

```
GET https://x402-doctor.fizzl.eu/api/v1/preflight?url=<url-encoded endpoint>&max_usd=0.05
```

Or the MCP tool `x402_preflight`. The answer has:

- `verdict`: `go`, `caution` or `no_go`, and `safe_to_pay`;
- `reasons`: what was found (price above the listing, payout address that can't receive, sanctioned recipient, testnet only, failed deliveries, and so on);
- the cheapest option that will settle.

What to do with it:

- `go`: pay the endpoint, with your price cap at its listed price.
- `caution`: tell your owner the reasons before paying.
- `no_go`: don't pay. Say why.

Related calls: `/api/v1/preflight/batch` ($0.005, up to 10 endpoints at once) and `/api/v1/preflight/deep` ($0.01, adds the full diagnosis, history and seller checks, for expensive calls).

## Paying for the preflight

The preflight is itself an x402 endpoint: USDC on Base, Solana, Algorand or Stellar, RLUSD on the XRP Ledger, or MPP. Use any x402 client with its own wallet, or the [Fizzl Agent Wallet](https://wallet.fizzl.eu/skill.md) for spending limits and owner approval. Never ask your owner to paste a private key into the chat.

## More

- Every tool, price and schema: https://x402-doctor.fizzl.eu/skill.md and https://x402-doctor.fizzl.eu/openapi.json
- What the daily scan shows about the x402 market: https://fizzl.eu/research/x402-health-october-2026/
