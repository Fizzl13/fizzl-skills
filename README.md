# Fizzl agent skills

Every [Agent Skill](https://agentskills.io) for the Fizzl services: `SKILL.md` files that tell an AI agent (Claude Code, Claude, Codex, Cursor and others that read skills) when and how to use a live service for agents that pay.

| Skill | Use it to | Where it lives |
|---|---|---|
| [`x402-preflight`](https://github.com/Fizzl13/x402-doctor/tree/master/skills/x402-preflight) | Check an x402 or MPP endpoint before paying it: go, caution or no_go | [x402 Doctor](https://x402-doctor.fizzl.eu) repo |
| [`x402-seller-check`](https://github.com/Fizzl13/x402-doctor/tree/master/skills/x402-seller-check) | Check your own paid endpoint until it passes, with the fix as code | [x402 Doctor](https://x402-doctor.fizzl.eu) repo |
| [`presign-check`](https://github.com/Fizzl13/presign-guard/tree/main/skills/presign-check) | Check a transaction, approval, signature or token before signing or buying, and screen an address against the OFAC SDN list before sending | [presign-guard](https://presign-guard.fizzl.eu) repo |
| [`agent-wallet-limits`](skills/agent-wallet-limits/SKILL.md) | Give an agent a wallet with spending limits and phone approval | this repo, for the [Fizzl Agent Wallet](https://wallet.fizzl.eu) |
| [`crypto-trend-signal`](skills/crypto-trend-signal/SKILL.md) | Ichimoku, confluence, levels, a 148-coin scan and setups for a coin | this repo, for [Ichimoku Signal](https://ichimoku-signal.fizzl.eu) |
| [`buy-live-data`](skills/buy-live-data/SKILL.md) | Get live data you can't produce yourself (weather, prices, search, news): the right paid API bought in one call | this repo, for [Fizzl Buy](https://buy.fizzl.eu) |

The services are paid per call over [x402](https://x402.org) or MPP (USDC on Base, Solana, Algorand or Stellar, RLUSD on the XRP Ledger), with no account or API key. Each has free calls to try first.

## Install

**Claude Code:** all six come from one plugin marketplace, together with the services' MCP servers:

```
/plugin marketplace add Fizzl13/x402-doctor
/plugin install x402-doctor@fizzl
/plugin install presign-guard@fizzl
/plugin install fizzl-agent-wallet@fizzl
/plugin install ichimoku-signal@fizzl
/plugin install fizzl-buy@fizzl
```

**Any agent that reads skills:**

```sh
npx skills add Fizzl13/x402-doctor
npx skills add Fizzl13/presign-guard
npx skills add Fizzl13/fizzl-skills
```

Or copy a skill folder into your agent's skills directory, for example `~/.claude/skills/`.

**MCP:** the services are also remote MCP servers, no install needed:

```
claude mcp add --transport http x402-doctor https://x402-doctor.fizzl.eu/mcp
claude mcp add --transport http presign-guard https://presign-guard.fizzl.eu/mcp
claude mcp add --transport http ichimoku-signal https://ichimoku-signal.fizzl.eu/mcp
claude mcp add --transport http fizzl-buy https://buy.fizzl.eu/mcp
```

## Ground rules in every skill

- Never ask the owner to paste a private key or seed phrase into the chat.
- Always cap the price at the listed one, and use the free calls first when they are enough.
- On red or no_go, don't pay or sign; on orange or caution, tell the owner why first.

## More

- [fizzl.eu](https://fizzl.eu) · [llms.txt](https://fizzl.eu/llms.txt)
- What x402 Doctor's daily scan of 50,000 paid endpoints shows: [x402 health report, October 2026](https://fizzl.eu/research/x402-health-october-2026/)
- Source of the services: [x402-doctor](https://github.com/Fizzl13/x402-doctor), [presign-guard](https://github.com/Fizzl13/presign-guard), [x402-examples](https://github.com/Fizzl13/x402-examples)

MIT licensed. Made by [Frits Zwager](https://cv.fizzl.eu) (Fizzl, Amsterdam).
