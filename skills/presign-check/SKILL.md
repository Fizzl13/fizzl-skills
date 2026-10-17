---
name: presign-check
description: Check a blockchain transaction, token approval, signature request or token before an AI agent signs or buys it. Use when an agent or wallet is about to sign an EVM transaction, an ERC-20 approval or permit, an eth_signTypedData or personal_sign request, a Solana, XRP Ledger or Algorand transaction, or buy a token, and whenever a user asks "is this safe to sign?". Answers green, orange or red with reason codes (known drainers, sanctioned addresses, unlimited approvals, look-alike tokens, simulation results) via presign-guard. Free quick check; $0.01 full verdict.
---

# Check before you sign

A signature can't be taken back. Before signing anything you didn't build yourself, ask [presign-guard](https://presign-guard.fizzl.eu): green, orange or red, with the reasons. You are never charged for an error.

- **Red:** don't sign. Tell your owner why.
- **Orange:** tell your owner the reasons before going on.
- **Green:** go on.

## Quick and free

MCP tools `presign_quick_check` and `token_quick_verdict` (10 calls an hour together) give the verdict only. Add the server once: `claude mcp add --transport http presign-guard https://presign-guard.fizzl.eu/mcp`.

## The full check ($0.01)

```
POST https://presign-guard.fizzl.eu/v1/check
```

Request types:

- Approval: `{"type": "approval", "chainId": 8453, "token": "0x…", "spender": "0x…", "amount": "1000000"}`
- Transaction: `{"type": "transaction", "chainId": 8453, "to": "0x…", "data": "0x…", "value": "0", "from": "0x…"}` (with `from` it is simulated)
- Typed-data signature: `{"type": "signature", "typedData": { … the eth_signTypedData_v4 payload … }}`
- Message: `{"type": "message", "message": "…", "origin": "https://site.example"}` (`eth_sign` is always red; a sign-in message for another site than `origin` is red)
- Solana: `{"type": "solana", "transaction": "<base64 wire transaction>"}` (decoded and simulated)
- Also `"type": "xrpl"` and `"type": "algorand"`.

EVM chains: 1, 10, 56, 137, 8453, 42161. Every party is screened against the OFAC SDN list. Add `input.intent` with what the transaction should do, and the answer says whether it matches.

`POST /v1/check/explain` ($0.03) adds a plain-language explanation (`lang`: `en` or `nl`).

## Before buying a token ($0.01)

`GET https://presign-guard.fizzl.eu/v1/token?chain=base&address=<token>`: grade, reasons and market data. Also `chain=solana` (mint address) and `chain=algorand` (asset id).

## Open approvals of a wallet ($0.02)

`GET https://presign-guard.fizzl.eu/v1/approvals?chain=base&address=<wallet>` lists every open approval and which to revoke (also `chain=solana`).

## Paying

Paid calls use x402 (USDC on Base, Solana, Algorand or Stellar, RLUSD on the XRP Ledger) or MPP. Use any x402 client with its own wallet, or the [Fizzl Agent Wallet](https://wallet.fizzl.eu/skill.md), which already runs this check on every signature it makes. Prepaid packs: `GET /v1/credits`. Never ask your owner to paste a private key into the chat.

## More

Every tool, price and schema: https://presign-guard.fizzl.eu/skill.md and https://presign-guard.fizzl.eu/openapi.json
