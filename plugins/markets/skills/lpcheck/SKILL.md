---
name: lpcheck
description: "Read a liquidity pool's volume, TVL and fee APR over 1 and 7 days, and spot a short APR spike. Use when the user asks things like \"Compare the fee tiers of the WETH and USDC pools on Ethereum: 1-day and 7-day volume and fee APR for each.\" or \"Which pools on Base have recent volume and a fee of at least 0.3 percent?\"."
license: MIT
---

# LP Pool Check

Read a liquidity pool's trading volume, TVL (total value locked) and fee APR over 1-day and 7-day windows. See whether a high APR comes from steady trading or a short spike, with the reason.

The tools come from the `lpcheck` MCP server, bundled with the `markets` plugin.
In Claude Code they are named `mcp__plugin_markets_lpcheck__<tool>`, for example `mcp__plugin_markets_lpcheck__check_pool`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `lpcheck` is connected.

## When to use

- Volume, TVL, and fee APR over 1-day and 7-day windows for one pool or a short scan
- Flags a high APR as real trading, a short spike, or unconfirmed, with the reason
- Compares volume, TVL, fee rate, and fee APR across chains and DEXes in one scan
- Estimates whether fees cover impermanent loss, gas, and exit cost for a range and holding period
- Calculates a plain impermanent loss break-even price line against expected fee income
- Evaluates range width against recent price trends to warn on narrow ranges in trending markets
- Checks out-of-range risk and warns when price is outside or near boundary ticks
- Assesses pool depth and fee tier suitability against asset volatility
- Cautions on tokenized stock market hours, oracle lag, and emerging chain liquidity
- Shows the fee APR formula and the concentrated-liquidity IL formula in the answer
- Token flags for holder concentration, mint, and transfer restrictions when GoPlus covers the chain
- Labels a position as fee-earning, accumulating at lows, or disguised token holding
- Read-only. No wallet, no signing, and no buy or sell recommendation

## Examples

- "Compare the fee tiers of the WETH and USDC pools on Ethereum: 1-day and 7-day volume and fee APR for each."
- "Which pools on Base have recent volume and a fee of at least 0.3 percent?"
- "If fees are 40 percent APR and price moves from 2000 to 2400 over 14 days, do fees cover impermanent loss?"
- "What are the 1-day and 7-day volume and fee APR for WETH and USDC on Ethereum?" calls `check_pool`.
- "Is the fee APR on the WETH-USDC pool on Base from real trading or a short spike?" calls `check_pool`.
- "Which pools on Base have at least 100000 of 1-day volume and a fee of at least 0.3 percent?" calls `find_active_pools`.
- "If I set a range from 2000 to 3000, price is 2500, fee APR is 40 percent, and I hold 14 days, do fees cover impermanent loss before gas?" calls `estimate_lp`.
- "Check token risk and the position type for this pool id on Ethereum: b99bcdf5-1350-4269-981e-0e9b5cccb007." calls `check_pool`.

## When not to use

- "Buy the LP token for the highest APR pool and set a stop.": The plugin is a read of public figures. It does not recommend buying, selling, or providing liquidity, and it cannot place an order.
- "Sign this wallet transaction to add liquidity to the WETH-USDC pool.": The plugin is read-only. It has no wallet and no signing path.
- "What is the current tick band of pool contract 0x88e6a0c2ddd26feeb64f039a2c41296fcb3f5640, and should I enter it?": The open index has no pool-contract field, and the plugin does not give an enter-or-avoid recommendation.

## Tools

- `check_pool`: Check one liquidity pool. Use this when the user asks about one real liquidity pool, such as "volume and fee APR for WETH and USDC on Ethereum" or "is this pool APR a spike?".
- `find_active_pools`: Find pools with volume and fees. Use this when the user wants pools across chains or DEXes with recent volume and a fee rate, such as "high fee pools on Base with real volume".
- `estimate_lp`: Estimate fees versus impermanent loss. Use this when the user gives numbers for a what-if and wants to know whether swap fees cover impermanent loss, gas, and exit cost.

## How to answer

Use these tools to read public liquidity-pool figures. Call check_pool for one real pool: a DefiLlama pool id, or chain plus token_a and token_b (symbols or token addresses). Call find_active_pools to scan chains for pools with recent volume and a fee rate. Call estimate_lp for a pure what-if with no pool: a price range or later price, a holding period, and a fee APR. Always show the APR flag and its reason, the risk checklist warnings, the as_of time, and the not-financial-advice notice. Every answer has a message that says what to pass next when the call did not find one pool. Follow it. status needs_tokens: a pool contract address is not in the open index, so ask for the two token addresses or a pool id, and do not invent a match. status ambiguous: several pools match, listed in other_tiers. Show them, or call check_pool again with one pool_id from other_tiers. status not_found: the pair, id, or chain is not in the index. Say so and ask for another chain or the token addresses. status unavailable: a source did not answer. Say so, do not fill in numbers, and try again later. apr_flag unconfirmed means the 7-day comparison is missing: do not call it real trading. find_active_pools ranks by 1-day fee APR, so a small pool with a one-day spike can come first. Check tvl_usd and apr_flag before you lead with a pool. position_type and risk warnings are labels and readings, not advice to enter or leave a position. Not financial advice: do not recommend buying, selling, or providing liquidity, and do not claim fees will be earned. Credit DefiLlama and, when token flags are present, GoPlus.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
