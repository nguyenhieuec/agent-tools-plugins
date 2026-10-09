---
name: food
description: "Look up a packaged food by barcode, search and compare products, and check listed allergens. Use when the user asks things like \"What is in the product with barcode 3017620422003?\" or \"Find oat milks sold in France\"."
license: MIT
---

# Food Facts

Look up a packaged food by its barcode, search for products, compare them side by side and check a product's listed allergens.

The tools come from the `food` MCP server, bundled with the `consumer` plugin.
In Claude Code they are named `mcp__plugin_consumer_food__<tool>`, for example `mcp__plugin_consumer_food__lookup_barcode`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `food` is connected.

## When to use

- Looks up a product by EAN or UPC barcode after checking the barcode's check digit
- Shows ingredients, listed allergens and traces, Nutri-Score and NOVA group
- Shows nutrition per 100 g: energy, fat, saturated fat, carbohydrates, sugars, fibre, protein, salt, sodium
- Searches products by name or brand, optionally limited to a country
- Compares up to five products by sugars, salt or energy per 100 g
- Checks a product's listed allergens against the ones you want to avoid
- Says how complete the entry is and reminds you to check the physical label

## Examples

- "What is in the product with barcode 3017620422003?"
- "Find oat milks sold in France"
- "Does barcode 5449000000996 contain milk or nuts?"
- "What is in the product with barcode 3017620422003?" calls `lookup_barcode`.
- "Find oat milks sold in France." calls `search_products`.
- "Which has less sugar per 100 g: 3017620422003 or 5449000000996?" calls `compare_products`.
- "Does barcode 3017620422003 contain milk or peanuts?" calls `check_allergens`.
- "Is 1234567890123 a real barcode? What product is it?" calls `lookup_barcode`.

## When not to use

- "Is it safe for my child with a severe peanut allergy to eat this chocolate bar?": The plugin reads community-contributed data and gives no safety or medical advice.
- "Plan me a 1,800 calorie vegetarian diet for the week to lose weight.": The plugin looks up packaged product data; it does not plan diets or give nutrition or medical guidance.
- "Has this brand of peanut butter been recalled?": Recalls are not in Open Food Facts; the Recalls plugin covers them.

## Tools

- `lookup_barcode`: Look up a barcode. Use this when the user gives a product barcode (EAN or UPC) and wants to know what is in it, such as "what is in 3017620422003?".
- `search_products`: Search products. Use this when the user wants to find food products by name or brand, such as "find oat milks" or "Nutella jars in Germany".
- `compare_products`: Compare products. Use this when the user wants to compare packaged foods, such as "which of these barcodes has the least sugar?".
- `check_allergens`: Check allergens. Use this when the user asks whether a product contains certain allergens, such as "does 5449000000996 contain milk or nuts?".

## How to answer

Use these tools for questions about packaged food products identified by barcode or name. Call lookup_barcode for one product, search_products to find barcodes by name or brand, compare_products to rank up to five barcodes by sugars, salt or kcal per 100 g, and check_allergens to see whether a product's listed allergens include ones the user wants to avoid. The data is crowd-sourced: always mention the data_quality_note and tell the user to check the physical label, and never describe an allergen check as a guarantee that a product is safe. If check_allergens returns unknown true, say the entry has no allergen or ingredient data. Credit Open Food Facts. Do not give dietary or medical advice. Open Food Facts has no recall data: say so when asked about a recall.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
