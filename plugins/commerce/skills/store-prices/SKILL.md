---
name: store-prices
description: "Ask what an online store sells, what it charges and what changed since the last time anyone looked. Use when the user asks things like \"Track competitor prices on allbirds.com\" or \"What did gymshark.com change this week?\"."
license: MIT
---

# Store Price Tracker

Ask what an online store sells, what it charges and what changed since the last time anyone looked.

The tools come from the `store-prices` MCP server, bundled with the `commerce` plugin.
In Claude Code they are named `mcp__plugin_commerce_store-prices__<tool>`, for example `mcp__plugin_commerce_store-prices__check_store_prices`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `store-prices` is connected.

## When to use

- Lists the products, prices, variants and availability of a Shopify store
- Shows new products, removed products, price changes and stock changes since the previous daily snapshot
- Keeps a daily snapshot of every store it is asked about, so price history builds up
- Reads the price and availability history of one product
- Compares up to five stores in one answer
- Compares store catalogs for price gaps, currency differences, and landed margins

## Examples

- "Track competitor prices on allbirds.com"
- "What did gymshark.com change this week?"
- "How has the price of the Allbirds Tree Runner moved?"
- "Track competitor prices on allbirds.com." calls `check_store_prices`.
- "What did gymshark.com change since yesterday?" calls `check_store_prices`.
- "Compare product prices on mysneakerstore.com with allbirds.com and find products with at least 15% price gap." calls `compare_store_prices`.
- "How has the price of the Allbirds Tree Runner changed over time?" calls `get_product_price_history`.
- "Which products at colourpop.com are sold out right now?" calls `check_store_prices`.

## When not to use

- "Write a product description for my new candle.": A writing task names no store whose products or prices need to be read.
- "Log in to my Shopify admin and lower all my prices by 10 percent.": The plugin only reads public product data and cannot sign in to or edit a store.
- "What is the price of the iPhone 17 on amazon.com?": Amazon is not a Shopify store, so there is no public product feed to read.

## Tools

- `check_store_prices`: Check a store's products and price changes. Read an online store's products, prices and availability, and what changed since its previous daily snapshot.
- `get_product_price_history`: Get a product's price history. Read how one product's price and availability moved over the days a store has been tracked.
- `compare_store_prices`: Compare prices and find gaps across stores. Compare product prices between two online stores to find price gaps, spreads, and margin differences.

## How to answer

Use check_store_prices when the user asks what an online store sells, what it charges, or what changed on it; pass the store's web address as the user wrote it, up to five stores. The first call for a store records its first snapshot, so changes are empty then: say that changes show up from the next daily snapshot. Use get_product_price_history when the user asks how one product's price or stock moved; pass the product's link, handle or title. Use compare_store_prices when the user asks to compare product prices between two stores, find price gaps or margins, or see which store sells an item cheaper; pass source_store and target_store web addresses, with optional query, match_by, min_gap_pct, target_currency, fx_rate, shipping_allowance, duty_allowance, or include_landed_duty. When stores quote in different currencies, prices are normalized into the target currency using official ECB reference rates read via Frankfurter. Link each product's url. Only Shopify stores with a public product feed can be read; for any other status (invalid, unavailable) relay the message and do not guess prices.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
