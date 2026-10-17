---
name: crypto-trend-signal
description: Get crypto trend signals for a trading or research agent: the Ichimoku cloud on any timeframe, a six-indicator confluence, support and resistance levels with a trade plan, a scan of 148 coins and ranked setups, plus a news-risk flag. Use when a user asks for the trend, signal or key levels of a coin such as BTC, ETH or SOL, or which coins are strongest. Free daily trend; paid calls from $0.01 via x402 or MPP (Ichimoku Signal). Data for the owner to judge, not financial advice.
---

# Crypto trend signals

[Ichimoku Signal](https://ichimoku-signal.fizzl.eu) gives trend data for any supported pair, for example `BTC-USDT`, `ETH-USDT` or `SOL-USDT`. Signals are data for your owner to judge, not financial advice.

## Free

- MCP tool `ichimoku_trend` (20 an hour): the daily verdict (`signal`, `cloud_position`, `tenkan_kijun_cross`). Add the server once: `claude mcp add --transport http ichimoku-signal https://ichimoku-signal.fizzl.eu/mcp`.
- MCP tool `pair_lookup`: is a pair supported, on which exchange, and which intervals.
- `GET https://ichimoku-signal.fizzl.eu/setups/preview`: the top setup, 24 hours delayed.

## Paid (x402 or MPP)

| Call | What it returns |
|---|---|
| `GET /signal/{pair}?interval=4h` | The Ichimoku signal on any interval (1m … 1M), with price and all five lines |
| `GET /signals/{pair}?interval=4h` | Six indicators with votes, a combined signal and confidence |
| `GET /levels/{pair}` | Support/resistance, ATR, pivots, Fibonacci and a long/short plan |
| `GET /scan` | The signal for 148 coins, strongest bullish first |
| `GET /setups` | Ranked setups with entry, stop, targets and risk/reward (a screener: in our walk-forward backtest no better than random entries after fees) |

The current prices are in https://ichimoku-signal.fizzl.eu/openapi.json; the signal starts at $0.01.

`/signal` and `/signals` can carry a `news` field: `risk` is `high` when a headline from the last 72 hours reports a hack, delisting, token unlock, peg failure or legal action for the coin. The signal itself never changes; on `high`, consider waiting.

## Paying

x402 (USDC on Base, Solana, Algorand or Stellar, RLUSD on the XRP Ledger) or MPP, with any x402 client or the [Fizzl Agent Wallet](https://wallet.fizzl.eu/skill.md). Always cap the price at the listed one. Never ask your owner to paste a private key into the chat.

## More

https://ichimoku-signal.fizzl.eu/skill.md
