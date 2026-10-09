---
name: tariffs
description: "Find an HS code, HTS code or customs code, and read the customs duty or import tax. Use when the user asks things like \"What are candidate tariff codes for a ceramic flower pot in the UK?\" or \"What is the US import duty on lithium-ion batteries?\"."
license: MIT
---

# Tariff & HS Code Duty Finder

Find an HS code, HTS code or customs code, and read the customs duty or import tax. Ask "what is the tariff code for a ceramic flower pot in the UK?", "what is the US duty on HTS 8507.60.00?" or "what duty applies to code 6914100000 from Vietnam in the UK?".

The tools come from the `tariffs` MCP server, bundled with the `commerce` plugin.
In Claude Code they are named `mcp__plugin_commerce_tariffs__<tool>`, for example `mcp__plugin_commerce_tariffs__find_tariff_codes`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `tariffs` is connected.

## When to use

- Finds candidate HS codes, HTS codes and customs codes for a described product in the UK or US schedule
- Shows the headings above each candidate, so lines called Other can be read in context
- Reads the customs duty and import tax rate for a UK or US tariff code
- Shows UK VAT, the preferential rate for an origin country and other measures on a code
- Lists the lines under a heading when a code is not one goods are declared under
- Labels every code as a candidate to check with customs, not a binding classification

## Examples

- "What are candidate tariff codes for a ceramic flower pot in the UK?"
- "What is the US import duty on lithium-ion batteries?"
- "What duty applies to UK commodity code 6914100000 from Vietnam?"
- "What are candidate tariff codes for a lithium-ion battery imported into the US?" calls `find_tariff_codes`.
- "What is the tariff code for a flower pot in the UK?" calls `find_tariff_codes`.
- "What is the UK import duty and VAT on commodity code 6914100000 from Vietnam?" calls `get_duty_rate`.
- "What is the US duty rate for HTS 8517.13.00.00 smartphones?" calls `get_duty_rate`.
- "What lines are under heading 6914 in the UK tariff?" calls `get_duty_rate`.

## When not to use

- "Give me a binding classification for my product so customs cannot challenge it.": The plugin lists candidate codes from the schedule; only customs can issue a binding ruling.
- "How much will it cost to ship a pallet of ceramics from Vietnam to Manchester?": The plugin reads duty rates in the schedule; it does not quote freight, fees or total landed cost.
- "What is the import duty for goods going into Germany?": Only the UK and US schedules are read, and the EU tariff is not covered.

## Tools

- `find_tariff_codes`: Find an HS code, HTS or customs code. Use this when the user asks for the tariff code, commodity code, customs code, HS code, HTS code, Harmonized System code or customs classification for a product, for example "what is the tariff code for a ceramic flower pot in the UK?", "HTS code for a lithium-ion battery" or "which heading covers cotton t-shirts for import into the US?".
- `get_duty_rate`: Get customs duty, tariff and import tax. Use this when the user asks what import duty, customs duty, tariff, import tax or VAT applies to a tariff code or HS code, for example "what is the US duty on HTS 8507.60.00?", "what duty applies to UK commodity code 6914100000 from Vietnam?" or "what is under heading 6914 in the UK tariff?".

## How to answer

Use find_tariff_codes to turn a product description into candidate tariff codes and get_duty_rate to read the duty on a code. Always send country: GB for goods imported into the United Kingdom, US for goods imported into the United States; ask which if the user did not say. The codes are candidates, not a classification: say so every time, tell the user the right code depends on the product's make-up and use and that customs decides, and suggest confirming with customs or a licensed customs broker, or asking for a binding ruling, before declaring goods. Describe the product plainly (what it is, the material, the use). If nothing matches, retry with fewer or plainer words, or the product noun alone. When several candidates fit, show the best few with their headings and let the user choose; do not pick one for them as the answer. Send originCountry (a two-letter code) to get_duty_rate when the user names where the goods come from, and read it as the schedule's rate for that origin, not as proof the goods qualify. If get_duty_rate returns lines under the code instead of a rate, the code is not one goods are declared under: show the lines and ask which fits. Pass on every note returned. For the US, say that chapter 99 additional duties are not included. Do not present the result as legal, customs or trade advice, and do not estimate total landed cost.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
