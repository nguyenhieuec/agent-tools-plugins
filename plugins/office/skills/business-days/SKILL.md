---
name: business-days
description: "Count workdays, office days and PTO days, or list bank holidays and school breaks. Use when the user asks things like \"How many working days are there between 14 July and 18 August 2026 in Germany?\" or \"What date is 37 business days after 11 August 2026 in England?\"."
license: MIT
---

# Business Days & Workday Calc

Count workdays, office days and PTO days between two dates, add business days for an SLA due date, or list bank holidays and school breaks. Ask "how many working days are there between 14 July and 18 August in Germany?", "what date is 37 business days after 11 August in England?", "is Monday a bank holiday in Bavaria?" or "what are the school holidays in Bavaria?".

The tools come from the `business-days` MCP server, bundled with the `office` plugin.
Call them as MCP tools. Do not call the server with curl or any other shell command: the MCP tool is the supported path. In Claude Code they are named `mcp__plugin_office_business-days__<tool>`, for example `mcp__plugin_office_business-days__count_business_days`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `business-days` is connected.

## When to use

- Counts workdays, office days and PTO days between two dates for a country or region
- Adds or subtracts business days from a date, skipping weekends and public holidays
- Lists bank holidays and statutory public holidays of a country and year, by region
- Lists school holidays and school breaks where they are published
- States how days were counted, including the weekend and which holidays were applied

## Examples

- "How many working days are there between 14 July and 18 August 2026 in Germany?"
- "What date is 37 business days after 11 August 2026 in England?"
- "List the public holidays in Bavaria in 2027"
- "How many working days are there between 14 July 2026 and 18 August 2026 in Germany?" calls `count_business_days`.
- "What date is 37 business days after 11 August 2026 in England?" calls `add_business_days`.
- "List the public holidays in Bavaria in 2027." calls `list_holidays`.
- "How many working days are there from 5 December to 31 December 2026 in Ontario, Canada?" calls `count_business_days`.
- "What are the school holidays in Bavaria in 2027?" calls `list_holidays`.

## When not to use

- "How many calendar days are there between 1 March and 30 June 2026?": A plain calendar-day difference does not depend on holidays; the tools exist for counts that skip weekends and holidays.
- "Can my employer in Germany make me work on a public holiday?": The plugin lists public holidays and counts days; it does not give employment or legal advice.
- "What time is it in Tokyo when it is 9 am on Monday in Berlin?": Time zone conversion is not something the plugin does; it works with dates and holidays only.

## Tools

- `count_business_days`: Count working days, workdays and PTO. Use this when the user asks how many working days, business days, workdays, office days or PTO days there are between two dates, or for a statutory holiday count, for example "how many working days between 14 July and 18 August in Germany?", "how many working days are left in December in Ontario?" or "does 16 days of leave cover 5 to 31 December?".
- `add_business_days`: Calculate a business-day due date and SLA. Use this when the user asks what date is a number of working days, business days or workdays after or before a date, including an SLA due date or turnaround, for example "what date is 37 business days after 11 August in England?", "when is payment due 30 business days from invoice date 3 March in Germany?" or "5 working days before 20 December in the US".
- `list_holidays`: List bank holidays and school breaks. Use this when the user asks which public holidays, bank holidays, statutory holidays or school breaks a country, state or province has in a year, for example "is Monday a bank holiday in Bavaria?", "public holidays in Vietnam in 2027" or "school holidays in Bavaria".

## How to answer

Use count_business_days for how many working days fall between two dates, add_business_days for the date a number of business days after or before a date, and list_holidays for a country's public or school holidays in a year. Always send the country as a two-letter code; ask for the country if the user did not give one. Send a region when the user names a state, province or nation (Bavaria is DE-BY, Scotland is GB-SCT, England is GB-ENG, California is US-CA); holidays only some regions keep are not counted without it, and regionalHolidaysNotCounted lists them, so mention them when it is not empty. Send weekendDays when the weekend is not Saturday and Sunday. State the day-count convention in the answer: for a range both ends are counted unless includeStartDate or includeEndDate is false, and for add_business_days the start date is day 0 and is not counted. Name the holidays that changed the result. If a note is returned, pass it on. Holiday data can differ from an employer's or a contract's calendar, so do not present the result as a legal deadline.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
