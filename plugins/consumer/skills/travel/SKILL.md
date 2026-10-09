---
name: travel
description: "UK and US government travel advice for a country, plus live delays and weather at an airport. Use when the user asks things like \"What is the current travel advice for Thailand?\" or \"Compare the UK and US travel advice for Mexico, Colombia and Peru\"."
license: MIT
---

# Travel Check

Check what the UK and US governments currently advise about a country, and see live delays and weather at an airport.

The tools come from the `travel` MCP server at `https://travel.openkrill.app/mcp`, bundled with the `consumer` plugin.
Call them as MCP tools, never from a shell. In Claude Code they are named `mcp__plugin_consumer_travel__<tool>`, for example `mcp__plugin_consumer_travel__get_travel_advice`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `travel` is connected.

## When to use

- Shows the UK FCDO travel advice alert and last review date for a country
- Shows the US State Department advisory level, summary and date for a country
- Quotes the UK entry requirements, safety or health section on request
- Compares the UK and US advice for up to five countries at once
- Reports FAA delays and closures at a US airport
- Shows the latest weather observation and forecast for an airport by IATA or ICAO code
- Links every answer to the official source and says which citizens the advice is written for

## Examples

- "What is the current travel advice for Thailand?"
- "Compare the UK and US travel advice for Mexico, Colombia and Peru"
- "Is JFK having delays right now?"
- "What is the current travel advice for Thailand?" calls `get_travel_advice`.
- "What are the entry requirements for Japan for a British traveller?" calls `get_travel_advice`.
- "Compare the UK and US travel advice for Mexico, Colombia and Peru." calls `compare_advisories`.
- "Is JFK having delays right now?" calls `get_airport_status`.
- "What is the weather at London Heathrow, EGLL?" calls `get_airport_status`.

## When not to use

- "Do I need a visa to enter Brazil with an Indian passport?": The plugin has no nationality-specific visa data; UK and US entry text applies only to their own citizens.
- "Should I cancel my trip because I feel unwell and have a fever?": It is a medical and trip-decision question; the plugin returns government advisory text and airport conditions only.
- "Will my flight BA117 be on time tomorrow, and which seat should I pick?": The plugin covers airports, not flights; airport weather is not a flight prediction and is not for flight planning.

## Tools

- `get_travel_advice`: Get travel advice. Use this when the user asks about travel advice, safety warnings, or entry requirements for a country, such as "is it safe to travel to Thailand?" or "what are the entry requirements for Japan?".
- `get_airport_status`: Get airport status. Use this when the user asks about delays, closures or weather at an airport, such as "is JFK delayed?" or "what is the weather at Heathrow?".
- `compare_advisories`: Compare travel advisories. Use this when the user wants the advice for several countries side by side, such as "compare Mexico, Colombia and Peru" or "which of these countries has the highest advisory level?".

## How to answer

Use these tools for questions about official government travel advice for a country and about current airport conditions. For one country, call get_travel_advice with the country name or ISO code; set section to entry-requirements, safety or health when the user asks about one of those. For several countries, call compare_advisories with up to five. For an airport, call get_airport_status with its IATA or ICAO code. Always give the user the dates and the source links, and say that UK advice is written for British citizens and US advice for US citizens, so entry rules for other passports can differ. Do not claim a country is safe or unsafe in your own words: quote the advisory level or alert as the government states it. Do not give visa, medical or legal advice, and do not present airport weather as a flight-planning source. FAA delay information covers US airports only.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
