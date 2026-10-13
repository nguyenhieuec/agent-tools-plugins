---
name: shop-duty
description: "Check the customs side of selling online across borders. Use when the user asks things like \"What EU duty applies to these 6 order lines shipped under IOSS?\" or \"Which products in my Shopify inventory CSV have no HS code or origin?\"."
license: MIT
---

# Shop Duty Desk

Check the customs side of selling online across borders.

The tools come from the `shop-duty` MCP server, bundled with the `commerce` plugin.
In Claude Code they are named `mcp__plugin_commerce_shop-duty__<tool>`, for example `mcp__plugin_commerce_shop-duty__eu_duty_estimate`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `shop-duty` is connected.

## When to use

- Estimates the EUR 3 per item EU customs duty for consignments up to EUR 150, citing the regulations
- Counts items for the duty both as lines are given and with identical lines merged
- Checks a Shopify Inventory or product CSV for missing HS codes, origins, identifiers and invalid barcodes
- Suggests candidate UK and US tariff codes for up to 20 product titles, labelled not binding
- Estimates duty, VAT and landed cost of an item from a UK or US tariff line

## Examples

- "What EU duty applies to these 6 order lines shipped under IOSS?"
- "Which products in my Shopify inventory CSV have no HS code or origin?"
- "Find candidate tariff codes for these 15 products"
- "I ship 6 order lines to Germany under IOSS: 3 different cotton t-shirts at 25 euros each and 3 different ceramic mugs at 15 euros each, all made in China, 120 euros in total. What EU duty applies?" calls `eu_duty_estimate`.
- "Does the EUR 3 duty apply to a parcel worth 200 euros sent by post from China?" calls `eu_duty_estimate`.
- "Here is my Shopify inventory export. Which products have no HS code or country of origin?" calls `customs_data_gap_check`.
- "Find candidate HS codes for these products: wool beanie, ceramic coffee mug, stainless steel water bottle." calls `bulk_hs_candidates`.
- "What will duty and VAT cost on a 80 pound item with 12 pounds shipping, tariff code 6109100010, going to the UK from China?" calls `landed_cost_estimate`.

## When not to use

- "Tell me what the legally binding HS code for my product is.": The plugin offers candidate codes only; a binding classification comes from customs.
- "Declare this shipment to customs for me and pay the duty.": The plugin calculates and checks; it submits nothing to customs.
- "Here are my orders with customer names and addresses. Work out the duty on each.": Order lines carry no customer data, and the tool refuses lines that include it.

## Tools

- `eu_duty_estimate`: Estimate EU duty on a low-value consignment. Best for: the EUR 3 per item EU customs duty on a consignment of up to EUR 150 imported under IOSS or by post, from 1 July 2026 until 1 July 2028 (Council Regulation (EU) 2026/382, Delegated Regulation (EU) 2026/1022).
- `customs_data_gap_check`: Check a Shopify CSV for customs data gaps. Best for: "which products have no HS code?" or "is my export ready for customs?".
- `bulk_hs_candidates`: Find candidate tariff codes for products. Best for: "what HS code could this product use?" for up to 20 product titles, from the UK Trade Tariff and, if asked, the US Harmonized Tariff Schedule.
- `landed_cost_estimate`: Estimate duty, VAT and landed cost. Best for: "what will import duty and VAT be on this?" for one item shipped to the UK or US from a known tariff line.

## How to answer

Use eu_duty_estimate for the EUR 3 per item EU duty on consignments up to EUR 150 (channel ioss or postal); pass order lines with description, origin, qty, unit_value_eur and optionally hs_code, never customer details. Report the duty for the lines as given and mention the lower grouped figure when it differs. Use customs_data_gap_check with the text of a Shopify Inventory CSV (it holds HS Code and COO) or product CSV; a null count means the file has no column for that check. Use bulk_hs_candidates for candidate tariff codes and landed_cost_estimate for duty, VAT and total. For a US landed cost, pass origin and transport (ocean, air or other). Report each chapter 99 line and each skipped heading, and the processing and harbor fees. If complete is false, say what missing lists; do not treat that total as complete. Always call a tariff code a candidate, not binding, and cite the sources and legal_basis returned. If a result says a rate is not a plain percentage or an amount is null, say it was not calculated rather than guessing.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
