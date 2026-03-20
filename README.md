# Stratscope

A static, single-file DeFi strategy P&L dashboard for Ethereum wallet `0x488Bdd8975620f080c396739295858B511890059`.

## What it shows

- **Total capital now** — live values from on-chain contract reads (`balanceOf`, `convertToAssets`)
- **Current strategies** — open positions with live vault values and accrued yield
- **Past strategies** — closed positions with realised P&L
- **Transactions** — all 16 txs from Feb 18 → Mar 20 2026 with exact amounts and gas costs
- **Token flows** — net received/sent per token

## Data sources

All figures are sourced directly from Ethereum mainnet via [Blockscout](https://eth.blockscout.com):

| Data point | Method |
|---|---|
| USDC wallet balance | `ERC20.balanceOf(wallet)` |
| Morpho vault value | `VaultV2.convertToAssets(shares)` on `0xBEeFF047...` |
| Fluid vault value | `FluidLiteUSD.convertToAssets(shares)` on `0x273DA948...` |
| Swap receipts | Exact `token_transfer` log value (USDC 6 decimals) |
| Gas costs | `fee_wei / 1e18 × ETH_price_at_block` |
| Morpho claim amounts | Delta between cumulative Merkle proof allocations |

## Key decimal notes

- USDC has **6 decimals** — raw value `15660863` = `$15.660863`
- Vault share tokens have **18 decimals**
- Gas fees are tiny in the post-Dencun era — total across 16 txs = **$0.3482**
- The Mar 7 MORPHO sell and Mar 20 DAM sell used `fillOrderArgs` (limit orders) — gas was paid by the order filler, not the wallet

## Strategies

### Current
| Strategy | Deployed | Current value | Net P&L |
|---|---|---|---|
| Steakhouse / Morpho | $20,000 | $20,051.890375 | +$108.14 |
| Fluid Lite USD | $10,040.741489 | $10,040.906407 | +$0.14 |

### Past (closed)
| Strategy | Deployed | Yield | Net P&L |
|---|---|---|---|
| Aave V3 USDC | $20,000 | +$36.630761 | +$36.62 |
| Aave Horizon RWA | $10,000 | +$15.431962 | +$15.41 |

## Running locally

This is a single static HTML file. Just open `index.html` in any browser — no build step, no dependencies, no server needed.

```bash
open index.html
# or
python3 -m http.server 8080
```

## Logos

Protocol logos are sourced from Blockscout token contract metadata (`icon_url` field on each token contract), proxied via [wsrv.nl](https://wsrv.nl) to bypass CoinGecko's CORS/referrer restrictions when served from a local file.

## Tech stack

- Vanilla HTML / CSS / JS — zero dependencies
- Fonts: Syne + DM Mono via Google Fonts
- Data: hardcoded from verified Blockscout API calls (see comments in source)
