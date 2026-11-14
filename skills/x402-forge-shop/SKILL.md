---
name: x402-forge-shop
description: Find, try and buy small ready-made x402 tools from x402 Forge (forge.fizzl.eu), for example a Dutch IBAN check, VAT calculator, postcode or phone number check, or working-days counter. Each tool is sold once, to one owner, for $1 in USDC on Base; the buyer gets a small x402 server with the tool, runs it with their own wallet as payee, and Forge lists that copy. Use when your user wants to own or resell a paid API, asks for a tool like these, or says "buy a tool from x402 Forge". Searching, describing and trying are free; only the buy URL is paid, by your own x402 wallet.
---

# Shop at x402 Forge

[x402 Forge](https://forge.fizzl.eu) is a small factory of AI agents that builds, tests and ships small deterministic tools that other agents pay for per call with x402. Every tool on the shelf is **for sale once**: the first buyer owns it. They get a small x402 server with the tool, start it with their own wallet as payee, and Forge stops selling the tool and lists the owner's copy (x402 Trust Index and the Bazaar). Every paid call to that copy goes to the owner.

## 1. Find a tool (free)

Add the MCP server once, then use its tools:

```
claude mcp add --transport http x402-forge https://forge.fizzl.eu/mcp
```

- `search_products` with `{ "query": "iban" }`: matching tools with a plain `summary`, `for_sale`, `buy_price` and a `page`.
- `describe_product` with `{ "slug": "iban-nl" }`: input and output schema, examples and the test record.

Without MCP: `GET https://forge.fizzl.eu/catalog.json` (every tool, with `sale: "for-sale" | "sold"`) or `https://forge.fizzl.eu/llms.txt`.

## 2. Try it (free)

- `try_product` with `{ "slug": "iban-nl" }`, or `GET https://forge.fizzl.eu/try/iban-nl`: runs the tool's own example.

Show your user what it returns before buying.

## 3. Buy it

1. Call `buy_product` with `{ "slug": "iban-nl" }`. It is free and does not pay: it returns `buy_url`, `price`, `network` and what the buyer receives, or `for_sale: false` with the owner's copy if the tool is sold.
2. Confirm with your user: the tool, the price ($1.00, once) and that it is exclusive.
3. Pay `buy_url` (`GET https://forge.fizzl.eu/forge/packages/{slug}?format=zip`) with an x402 client. It answers 402 with a PAYMENT-REQUIRED header (x402 v2, scheme `exact`, USDC on Base, `eip155:8453`); your client signs and retries with PAYMENT-SIGNATURE. Gas is paid by the facilitator, and you are charged only when the answer is 200.
4. Keep the receipt: the `X-Forge-Receipt-URL` header is proof of purchase, checked against Base when opened.

If someone else is buying the same tool at that moment, the answer is 409 and nothing is charged; 410 means it was just sold.

## 4. Run the copy

Unzip, then:

```sh
npm install
PAY_TO=0xYourWallet PUBLIC_URL=https://your-host npm start
```

On any Node host (Railway, Render, Fly, a VPS). The copy tells Forge where it runs; Forge checks that it is the real tool, at most 1 cent a call and paid to the owner's wallet, then lists it. Set `FORGE_LISTING=off` to stay unlisted.

## Paying

Use any x402 client: an x402 fetch or payment tool in your agent, `@x402/fetch` in code, or the [Fizzl Agent Wallet](https://wallet.fizzl.eu/skill.md), which pays within your owner's limits and asks on their phone above them. Cap the price at the `price` that `buy_product` returned ($1.00). Never ask your owner to paste a private key or seed phrase into the chat. Before paying, you can check the route with x402 Doctor's `x402-preflight` skill.

## More

https://forge.fizzl.eu/llms.txt · https://forge.fizzl.eu/openapi.json · https://forge.fizzl.eu/.well-known/x402
