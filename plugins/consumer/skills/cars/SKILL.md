---
name: cars
description: "Check a US car: decode a VIN, see recalls, crash ratings, owner complaints and fuel economy. Use when the user asks things like \"Give me a report on a 2016 Honda Civic\" or \"Any recalls on a 2018 Toyota Camry?\"."
license: MIT
---

# Car Check

Check a US car's public record before you buy or while you own it: decode a VIN, see its recalls, crash test ratings and owner complaints, and compare fuel economy.

The tools come from the `cars` MCP server, bundled with the `consumer` plugin.
In Claude Code they are named `mcp__plugin_consumer_cars__<tool>`, for example `mcp__plugin_consumer_cars__decode_vin`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `cars` is connected.

## When to use

- Decodes a 17-character VIN into year, make, model, trim, body style, engine and fuel, with NHTSA's own error codes
- Lists NHTSA recalls for a year, make and model with the component, consequence and remedy
- Shows NHTSA 5-star crash test ratings: overall, front, side and rollover
- Counts owner complaints filed with NHTSA and names the components reported most often
- Shows EPA fuel economy: city, highway and combined mpg, mpge, electric range and annual fuel cost
- Builds a one-call report from a VIN or from year, make and model
- Covers US vehicles only and never gives valuation, history or safety advice

## Examples

- "Give me a report on a 2016 Honda Civic"
- "Any recalls on a 2018 Toyota Camry?"
- "What car is VIN 1HGCM82633A004353?"
- "What car is VIN 1HGCM82633A004353?" calls `decode_vin`.
- "Give me a report on a 2016 Honda Civic." calls `get_vehicle_report`.
- "Any recalls on a 2018 Toyota Camry?" calls `list_recalls`.
- "What mpg does a 2022 Toyota RAV4 hybrid get?" calls `get_fuel_economy`.
- "How many miles of range does a 2020 Tesla Model 3 have?" calls `get_fuel_economy`.

## When not to use

- "How much is my 2015 Honda Accord worth?": The plugin reads recalls, ratings, complaints and fuel economy; it never estimates a vehicle's value.
- "Has this used car ever been in an accident? Here is the VIN.": NHTSA and EPA data hold no accident or ownership history, so the plugin cannot answer and must not guess.
- "My 2012 Ford Focus is pulling to the left when I brake. Is it safe to keep driving?": The plugin gives no safety or mechanical advice and a recall list cannot diagnose a specific car.

## Tools

- `decode_vin`: Decode a VIN. Use this when the user gives a 17-character vehicle identification number (VIN) and wants to know what car it is, such as "what car is 1HGCM82633A004353?".
- `get_vehicle_report`: Get a vehicle report. Use this when the user wants an overview of a US car, such as "give me a report on a 2016 Honda Civic".
- `list_recalls`: List recalls. Use this when the user asks about recalls for a car model, such as "any recalls on a 2018 Toyota Camry?".
- `get_fuel_economy`: Get fuel economy. Use this when the user asks about mpg, electric range or fuel cost for a car, such as "what mpg does a 2022 Toyota RAV4 hybrid get?".

## How to answer

Use these tools for questions about US cars by VIN or by year, make and model. Call decode_vin to turn a VIN into the vehicle's details and surface any error_codes it returns, list_recalls for NHTSA recalls, get_fuel_economy for EPA mpg figures, and get_vehicle_report for recalls, crash ratings, complaints and fuel economy together. Recalls and complaints are listed by year, make and model, not by VIN, so say so and point to nhtsa.gov/recalls to check one specific car. Complaints are reports from owners, not confirmed defects. A count of zero or a missing rating is not proof of safety. Name every vehicle variant the data returns instead of picking one. This is a data lookup, not a valuation, history report or inspection: do not give safety, mechanical, valuation or buying advice. US vehicles only. Credit NHTSA and the EPA.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
