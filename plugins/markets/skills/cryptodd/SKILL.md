---
name: cryptodd
description: "Screen a new token for red flags and keep signals, and find a wallet's self-declared public name. Use when the user asks things like \"Screen this Solana token for red flags: paste the mint address\" or \"Does the wallet that launched this Base token have a public name?\"."
license: MIT
---

# Crypto Due Diligence

Screen a new token for red flags and keep signals, and find a wallet's self-declared public name. One call runs the checks people do by hand before they buy a fresh coin, each with its source and a link, and lists what no free source can answer.

The tools come from the `cryptodd` MCP server, bundled with the `markets` plugin.
In Claude Code they are named `mcp__plugin_markets_cryptodd__<tool>`, for example `mcp__plugin_markets_cryptodd__screen_token`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `cryptodd` is connected.

## When to use

- Screens a new token for red flags and keep signals in one call, each with source and link
- Flags a paid DEX Screener promotion with the date it was paid
- Flags a GitHub that is days old or nearly empty, and credits an established, active one
- Flags a pump.fun or launchpad launch from the mint, the pair and GoPlus
- Shows whether DefiLlama tracks usage revenue for the token's protocol
- Lists contract hazards GoPlus marks: mint, freeze, honeypot, blacklist, high tax
- Checks whether the deployer wallet has a self-declared ENS or Farcaster name
- Spots a token that pumped, fell back and is still shipping code
- Says which signals no free source can answer, such as X account age
- Wallet identity from self-declared ENS and Farcaster names only, never private people
- Read-only. No wallet, no trading and no buy or sell advice

## Examples

- "Screen this Solana token for red flags: paste the mint address"
- "Does the wallet that launched this Base token have a public name?"
- "Is this token's GitHub real or made the day before launch?"
- "Screen this Solana token for red flags: DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263." calls `screen_token`.
- "Did the team pay for DEX Screener promotion on Base token 0x532f27101965dd16442e59d40670faf5ebb142e4?" calls `screen_token`.
- "Is the GitHub for Uniswap's token 0x1f9840a85d5af5bf1d1762f925bdaddc4201f984 on Ethereum established or new?" calls `screen_token`.
- "Does the wallet 0xd8da6bf26964af9d7eed9e03e53415d37aa96045 have a public ENS or Farcaster name?" calls `wallet_identity`.
- "Screen Ethereum token 0x6982508145454ce325ddbe47a25d4ec3d2311933 and check its deployer 0xd8da6bf26964af9d7eed9e03e53415d37aa96045 for a public name." calls `screen_token`.

## When not to use

- "Buy 2 SOL of this pump.fun token for me and set a sell at 3x.": The plugin only reads public sources. It has no wallet and gives no trading advice.
- "Find the real name and home address of the person behind wallet 0x1234567890abcdef1234567890abcdef12345678.": Wallet identity returns only names the owner published. Linking a wallet to a private person is out of scope.
- "Tell me this token is 100 percent safe so I can put my savings in it.": The screen lists signals from free sources and never rates a token as safe.

## Tools

- `screen_token`: Screen a token for red flags. Use this when the user gives a token address and asks "is this a rug?", "screen this coin", "who is the dev?" or what to check before buying.
- `wallet_identity`: Find a wallet's public name. Use this when the user asks whether a wallet address has a public name, such as a token deployer or a known trader's wallet.

## How to answer

Use screen_token when the user gives a token address and asks whether it looks risky, who is behind it, or what to check before buying. Use wallet_identity when the user asks whether a wallet has a public name. Show every signal with its state and source link, then the reading, then the unknown signals with what the user can check by hand. unknown is not a pass and not a fail: never count it either way and never fill it in. Never call a token safe or a scam, never recommend buying, selling or holding, and always show the notice. Credit GoPlus with the powered_by line when contract hazards are shown. For wallet_identity, report only the names returned and the note. Never guess who owns a wallet, and refuse to search for a private person's wallets.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
