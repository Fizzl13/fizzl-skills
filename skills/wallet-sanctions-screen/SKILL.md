---
name: wallet-sanctions-screen
description: Screen a crypto wallet address against the US Treasury OFAC SDN sanctions list before paying it or accepting money from it. Use when an AI agent or app is about to send funds to an unknown address, accept a payment, onboard a counterparty, or build a sanctions policy for a wallet (for example a Privy condition set). Any chain's address; EVM addresses are also checked against theft and phishing reports. $0.01 per screen via presign-guard, or the full list as free open data.
---

# Screen a wallet for sanctions

Paying a sanctioned address is a legal risk for your owner, even by accident. The US Treasury publishes the digital-currency addresses on its OFAC SDN list; [presign-guard](https://presign-guard.fizzl.eu) downloads that list itself at start-up and every 6 hours.

## One address ($0.01)

```
GET https://presign-guard.fizzl.eu/v1/screen?address=<address>
```

Or the MCP tool `screen_wallet` (`claude mcp add --transport http presign-guard https://presign-guard.fizzl.eu/mcp`).

- **Red:** the address is on the OFAC SDN list, or reported for theft or phishing (EVM). Never pay it or accept funds from it. Tell your owner.
- **Orange:** a lesser report against the address (EVM). Tell your owner before going on.
- **Green:** not on the list and no reports. Go on.

Works for any chain's address on the list: Bitcoin, Ethereum and other EVM chains, Tron, Solana and the rest.

## The whole list, free

`GET https://presign-guard.fizzl.eu/v1/sanctions` serves the current list as open data (with Solana USDC and USDT token accounts), for apps that screen locally or keep a deny list in sync. Download it at most every few hours; the published date is in the answer.

For Privy wallets there is a ready example that turns the list into Privy condition sets: https://github.com/Fizzl13/x402-examples/tree/main/privy-sanctions-sync

## Every check screens too

Every `POST /v1/check` on presign-guard also screens all parties of the transaction (reason `SANCTIONED_ADDRESS`, red), and x402 Doctor's preflight screens the seller's payout address before you pay an x402 endpoint.

## Paying

x402 (USDC on Base, Solana, Algorand or Stellar, RLUSD on the XRP Ledger) or MPP, with any x402 client or the [Fizzl Agent Wallet](https://wallet.fizzl.eu/skill.md). Never ask your owner to paste a private key into the chat.

## More

https://presign-guard.fizzl.eu/skill.md
