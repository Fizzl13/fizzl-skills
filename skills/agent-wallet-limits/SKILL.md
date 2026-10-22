---
name: agent-wallet-limits
description: Give an AI agent a crypto wallet with spending limits so it can pay for x402 and MPP APIs and send USDC within rules its owner sets, with approval on the owner's phone above the limit. Use when a user wants their agent (Claude, Cursor or another MCP client) to pay for APIs on its own, asks how to cap what an agent can spend, or needs a receipt for every purchase. The owner keeps the keys; every signature is checked by presign-guard first. Free plan; Pro $5 a month.
---

# A wallet with limits for your agent

The [Fizzl Agent Wallet](https://wallet.fizzl.eu) lets an agent pay for things within rules its owner sets:

- Up to the owner's limit the agent pays on its own; above it the owner approves on their phone (Telegram) or on the dashboard. No answer means no payment.
- Every signature is checked by presign-guard first. Known drainers and sanctioned addresses are never signed.
- The owner sees every purchase as a receipt: what was bought, why, and what came back.
- The server never holds keys or money. The agent signs with its own key; the server only says yes or no.

## Ground rules for the agent

1. Never ask your owner to paste a private key, seed phrase or recovery phrase into the chat. Keys go in the MCP configuration, which the owner edits.
2. Use a separate agent wallet with only what you may spend, never your owner's main wallet. The limits are checked by the MCP server on your owner's computer, not on the blockchain, so an agent that can read files could read the key: never read the key file or the MCP config yourself.
3. Always set `max_price_usd`, and pay only what the task needs.
4. If a payment is refused, tell your owner why. Don't look for a way around the rules.

## Setup (walk your owner through it)

1. The owner opens https://wallet.fizzl.eu, signs in with e-mail (no crypto wallet needed), a passkey or their wallet (MetaMask, Phantom, Xaman, Pera or Defly; no password), and adds an agent under **Your agents**. The dashboard shows a key starting with `awk_` once.
2. The owner makes a new wallet just for the agent (the dashboard can make one in the browser) and puts a few dollars of USDC on Base in it: from an exchange (withdraw with Base as the network) or from their own wallet.
3. The owner installs the MCP server, for example in Claude Code. The key goes into a file at a prompt (not shown, not in the shell history or the config), and the version is pinned:

```sh
mkdir -p ~/.fizzl && (umask 077; printf 'Agent wallet key: '; stty -echo; read -r k; stty echo; echo; printf '%s\n' "$k" > ~/.fizzl/agent-key)
claude mcp add wallet -e AGENT_KEY_FILE=$HOME/.fizzl/agent-key -e WALLET_SERVER_URL=https://wallet.fizzl.eu -e WALLET_SERVER_KEY=awk_… -- npx -y presign-guard-wallet-mcp@0.20.0
```

The dashboard shows the same setup for Claude Desktop, Cursor and other MCP clients, with the current version.

4. Optional: **Connect Telegram** on the dashboard for approvals on the phone. **Test my setup** checks everything without paying.

## Using it

| Tool | When |
|---|---|
| `wallet_status` | First, and before spending: balances, limits, what is left today |
| `find_services` | Search the x402 catalog for a paid API, with a price cap |
| `pay_x402` | Call and pay an x402 API (or an MPP API), with `max_price_usd` and a short `reason` |
| `send_usdc` | Send USDC to an address, with a `reason` |
| `pause_spending` | Something looks wrong: stop all signing until the owner resumes |

The catalog is also searchable without the wallet: `GET https://wallet.fizzl.eu/api/public/services/search?q=bitcoin%20signal&max=0.1` (add `&reliable=1` for sellers that x402 Doctor has seen payable on 90%+ of its daily checks).

## More

- Full instructions: https://wallet.fizzl.eu/skill.md
- The MCP server: https://www.npmjs.com/package/presign-guard-wallet-mcp
- Source: https://github.com/Fizzl13/x402-examples
