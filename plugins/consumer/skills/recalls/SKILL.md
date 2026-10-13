---
name: recalls
description: "Check US recalls for a car, a baby product, a food or a medicine in the official recall databases. Use when the user asks things like \"Does my 2018 Honda Accord have any recalls?\" or \"Is the Fisher-Price Rock 'n Play sleeper recalled?\"."
license: MIT
---

# Recall Check

Check whether a car, a baby product, a food or a medicine has been recalled in the United States, using the official recall databases.

The tools come from the `recalls` MCP server, bundled with the `consumer` plugin.
In Claude Code they are named `mcp__plugin_consumer_recalls__<tool>`, for example `mcp__plugin_consumer_recalls__check_vehicle_recalls`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `recalls` is connected.

## When to use

- Lists the NHTSA recalls for a vehicle by model year, make and model
- Decodes a 17-character VIN into year, make, model, trim and engine
- Searches CPSC recalls of consumer products such as cribs, strollers, sleepers and heaters
- Searches FDA food, supplement and drug recalls, optionally limited to recent days
- Lists recent FDA pet food recalls, such as dog or cat food from the last week
- Shows the hazard, the maker's remedy and a link or recall number to confirm at the source

## Examples

- "Does my 2018 Honda Accord have any recalls?"
- "Is the Fisher-Price Rock 'n Play sleeper recalled?"
- "Any dog food recalls this week?"
- "Does my 2018 Honda Accord have any recalls?" calls `check_vehicle_recalls`.
- "Decode this VIN: 1HGCM82633A004352." calls `decode_vin`.
- "Is the Joolz Aer2 car seat adapter recalled?" calls `search_product_recalls`.
- "Was there a recall on this infant formula? It is Nara Organics Whole Milk Organic Infant Formula." calls `search_food_drug_recalls`.
- "Any dog food recalls this week?" calls `search_pet_food_recalls`.

## When not to use

- "Look up the owner of license plate 7ABC123 and tell me whether their car has been recalled.": The plugin takes no owner names or plates and has no plate or owner data; asking for them would be a privacy risk.
- "Has my BMW been recalled in Germany?": The data sources are US agencies only; a US result would be misleading for a car sold elsewhere.
- "Is it safe to take this medication while pregnant?": It is a medical safety question, not a recall lookup; the plugin returns recall records and no clinical guidance.

## Tools

- `check_vehicle_recalls`: Check vehicle recalls. Use this when the user asks whether a car, truck or motorcycle has recalls, such as "does my 2018 Honda Accord have any recalls?".
- `decode_vin`: Decode a VIN. Use this when the user gives a 17-character vehicle identification number or asks to "decode this VIN".
- `search_product_recalls`: Search product recalls. Use this when the user asks whether a consumer product is recalled, such as a crib, stroller, toy, appliance, heater or tool, or "is this car seat recalled?" (for car seat accessories).
- `search_food_drug_recalls`: Search food and drug recalls. Use this when the user asks about a recall of food, infant formula, a dietary supplement or a medicine, such as "was there a recall on this baby formula?" or "any supplement recalls this week?".
- `search_pet_food_recalls`: Search pet food recalls. Use this when the user asks about a recall of dog food, cat food, pet treats or other pet food, such as "dog food recall this week".

## How to answer

Use these tools for recall questions about vehicles, consumer products, food, pet food and drugs sold in the United States. For a vehicle, call check_vehicle_recalls with its year, make and model; when the user gives a VIN, call decode_vin first and use the year, make and model it returns. For cribs, strollers, toys, appliances and other consumer goods, and for car seat accessories, call search_product_recalls with the brand and product; for a car seat or tire itself it cannot see NHTSA recalls, so say that and point to nhtsa.gov/recalls. For food, infant formula, supplements or medicine, call search_food_drug_recalls; for dog, cat or other pet food, call search_pet_food_recalls. Convert "this week" or "this month" into withinDays. Never ask the user for their name, address or license plate: the lookups need only a year, make and model, a VIN, or a product description. Always tell the user the data is US only and not a safety guarantee, and for a specific car point them to nhtsa.gov/recalls with their VIN. Do not tell the user an item is safe because nothing was found.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
