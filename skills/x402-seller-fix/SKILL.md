---
name: x402-seller-fix
description: Diagnose and fix a broken x402 or MPP paid endpoint on the seller side. Use when someone building an x402 API sees agents fail to pay, clients reject the 402, the CDP Bazaar or another index does not list the route, or payments do not settle; or when reviewing a new x402 integration before launch. Runs x402 Doctor's checks (402 challenge, resource URL, price against the listing, network and token, payout address, Bazaar discovery, MPP realm) and returns the fix as code. Free web check; $0.01 diagnosis, $0.05 fixes.
---

# Fix a broken x402 endpoint

Most x402 endpoints that agents can't pay fail on a few details. In x402 Doctor's daily scan of about 50,000 paid endpoints, the most common problems are:

| Problem | What it means | Fix |
|---|---|---|
| Resource mismatch | The 402 names another URL than the one called (relative path, other host, http instead of https) | Put the full public https URL of the route in `resource` |
| No 402 | The listed URL answers 404, 401, a login page, or 402 only on POST while the listing says GET | Answer 402 on the listed method, or fix the listing |
| Testnet only | Only a testnet is offered | Add a mainnet option |
| MPP realm mismatch | An MPP challenge names another host as its realm | Use the service's own host |
| Price above advertised | The 402 asks more than the listing | Keep listing and 402 in sync |
| Stellar asset as text | `"USDC"` instead of the USDC token contract | Use the contract address the official client expects |

## Check your endpoint

1. Free, in the browser: paste the URL at https://x402-doctor.fizzl.eu for the full report.
2. Free, from the terminal or CI: `npx github:Fizzl13/x402-doctor <url>` (exits 1 on failures), or the GitHub Action `Fizzl13/x402-doctor-action@v1`.
3. Free MCP tool `x402_quick_check` (10 an hour): `claude mcp add --transport http x402-doctor https://x402-doctor.fizzl.eu/mcp`.
4. Paid, for agents: `GET https://x402-doctor.fizzl.eu/api/v1/diagnose?url=<endpoint>&method=GET` ($0.01, every check with a hint) and `GET /api/v1/fix?url=<endpoint>&method=GET` ($0.05, the code changes for your stack).

After a fix, run the check again. Indexes such as the CDP Bazaar read the 402 again on their own schedule.

## Prove that a payment settled

`POST https://x402-doctor.fizzl.eu/api/settlement` with `{"header": "<the PAYMENT-RESPONSE header>"}` shows the on-chain transaction, network, payer and amount (free). Web page: https://x402-doctor.fizzl.eu/settlement

## More

- The most common seller mistakes, explained: https://x402-doctor.fizzl.eu/sellers
- Every tool and price: https://x402-doctor.fizzl.eu/skill.md
