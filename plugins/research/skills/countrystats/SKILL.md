---
name: countrystats
description: "Official economic and demographic figures for a country, with IMF projections and World Bank data. Use when the user asks things like \"What is Vietnam's population according to the World Bank?\" or \"Compare GDP per capita for Germany, France and Italy.\"."
license: MIT
---

# Country Stats Facts

Look up official economic and demographic figures for a country, and see the IMF's projections, from the World Bank and the IMF.

The tools come from the `countrystats` MCP server, bundled with the `research` plugin.
In Claude Code they are named `mcp__plugin_research_countrystats__<tool>`, for example `mcp__plugin_research_countrystats__get_country_indicator`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `countrystats` is connected.

## When to use

- Latest World Bank value, or the value for a given year, for a country on 14 economic and demographic series
- Compares up to 6 countries on one series and ranks them in a single call
- IMF projections for real GDP growth, inflation and government debt for the current year and the next two
- Every value carries its year, unit, source and the date the source was last updated
- Accepts country names, common short names and ISO 2 or 3 letter codes
- Annual statistics only: no live prices, exchange rates or advice

## Examples

- "What is Vietnam's population according to the World Bank?"
- "Compare GDP per capita for Germany, France and Italy."
- "What does the IMF forecast for India's growth next year?"
- "What is Vietnam's population according to the World Bank?" calls `get_country_indicator`.
- "Compare GDP per capita for Germany, France and Italy." calls `compare_countries`.
- "What does the IMF forecast for India's growth next year?" calls `get_imf_forecast`.
- "What was the unemployment rate in Spain in the latest year available?" calls `get_country_indicator`.
- "Rank Nordic countries by life expectancy." calls `compare_countries`.

## When not to use

- "Should I buy euros now?": Currency timing is live market data and advice; this plugin returns annual statistics.
- "What is the weather in Hanoi?": Weather is not a World Bank or IMF statistic.
- "Will the stock market crash next quarter?": The plugin reports published statistics and IMF projections, not market predictions or advice.

## Tools

- `get_country_indicator`: Get a country statistic. Use this when the user asks for one official statistic for one country, such as "what is Vietnam's population?" or "what was unemployment in Spain in 2022?".
- `compare_countries`: Compare countries. Use this when the user asks to compare or rank countries on one statistic, such as "compare GDP per capita for Germany, France and Italy" or "rank the Nordic countries by life expectancy".
- `get_imf_forecast`: Get an IMF forecast. Use this when the user asks what the IMF projects for a country, such as "what does the IMF forecast for India's growth next year?".

## How to answer

Use these tools to answer questions about a country's official statistics from the World Bank and the IMF. Call get_country_indicator for one country and one series, compare_countries to compare or rank two to six countries on one series, and get_imf_forecast for the IMF's projection of growth, inflation or debt. Always state the year of each value and the source with its as_of date: the latest World Bank value is usually one to two years old and can be from a different year for each country, so say so when same_year is false. Group names such as Nordic or G7 must be turned into a list of countries before calling compare_countries, for up to six countries. A status of no_data means the source has no figure for that country and year, and unavailable means the source could not be read: say so and do not estimate a number. IMF values for the current and later years are projections that change with each edition, not facts. These tools do not provide live market data, exchange rates, stock or currency advice, weather or news; decline those requests.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
