# Fizzl agent skills

Agent skills for AI agents that pay. Each skill is a `SKILL.md` that tells an agent when and how to use a live service, in the [Agent Skills](https://agentskills.io) format that Claude Code, Claude, Cursor and other agents read.

| Skill | Use it to | Service |
|---|---|---|
| [`x402-preflight`](skills/x402-preflight/SKILL.md) | Check an x402 or MPP endpoint before paying it: go, caution or no_go | [x402 Doctor](https://x402-doctor.fizzl.eu) |
| [`x402-seller-fix`](skills/x402-seller-fix/SKILL.md) | Find out why agents can't pay your x402 endpoint, and get the fix as code | [x402 Doctor](https://x402-doctor.fizzl.eu) |
| [`presign-check`](skills/presign-check/SKILL.md) | Check a transaction, approval, signature or token before signing or buying: green, orange or red | [presign-guard](https://presign-guard.fizzl.eu) |
| [`wallet-sanctions-screen`](skills/wallet-sanctions-screen/SKILL.md) | Screen a wallet address against the OFAC SDN list before paying it or accepting money from it | [presign-guard](https://presign-guard.fizzl.eu) |
| [`agent-wallet-limits`](skills/agent-wallet-limits/SKILL.md) | Give an agent a wallet with spending limits and phone approval | [Fizzl Agent Wallet](https://wallet.fizzl.eu) |
| [`crypto-trend-signal`](skills/crypto-trend-signal/SKILL.md) | Ichimoku, confluence, levels, a 148-coin scan and setups for a coin | [Ichimoku Signal](https://ichimoku-signal.fizzl.eu) |

The services are paid per call over [x402](https://x402.org) or MPP (USDC on Base, Solana, Algorand or Stellar, RLUSD on the XRP Ledger), with no account or API key. Each has free calls to try first.

## Install

**Claude Code**, as a plugin:

```
/plugin install fizzl-agent-payments --marketplace Fizzl13/fizzl-skills
/plugin install fizzl-crypto-signals --marketplace Fizzl13/fizzl-skills
```

On older Claude Code versions: `/plugin marketplace add Fizzl13/fizzl-skills`, then `/plugin install fizzl-agent-payments@fizzl`.

**Any agent that reads skills:** copy a folder from `skills/` into your agent's skills directory, for example `~/.claude/skills/` or `.claude/skills/` in a project.

**MCP:** the services are also remote MCP servers, no install needed:

```
claude mcp add --transport http x402-doctor https://x402-doctor.fizzl.eu/mcp
claude mcp add --transport http presign-guard https://presign-guard.fizzl.eu/mcp
claude mcp add --transport http ichimoku-signal https://ichimoku-signal.fizzl.eu/mcp
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
