---
name: tax
description: "Get the US federal tax numbers right, with the IRS document each one comes from. Use when the user asks things like \"What is the 2026 standard deduction for a single filer?\" or \"I sell e-books from Germany with 14,000 EUR sales to France. Must I register for VAT?\"."
license: MIT
---

# Tax Numbers

Get the US federal tax numbers right, with the IRS document each one comes from.

The tools come from the `tax` MCP server, bundled with the `office` plugin.
In Claude Code they are named `mcp__plugin_office_tax__<tool>`, for example `mcp__plugin_office_tax__get_tax_parameters`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `tax` is connected.

## When to use

- Looks up federal tax brackets, standard deduction and capital gains thresholds with IRS citations
- Gives the Social Security wage base and self-employment tax rules
- Gives 401(k) and IRA contribution limits
- Finds the IRS standard mileage rate for any date
- Lists filing and estimated-payment deadlines for a tax year
- Estimates federal tax with stated assumptions and exclusions
- Says if a seller must register for VAT or sales tax in the EU, UK or a US state, with limit, rate and filing
- Adds an official source link, the check date and an accountant note to every duty answer

## Examples

- "What is the 2026 standard deduction for a single filer?"
- "I sell e-books from Germany with 14,000 EUR sales to France. Must I register for VAT?"
- "Roughly how much federal tax on $90,000 of wages and $30,000 of freelance income in 2026?"
- "What is the standard deduction for a single filer in 2026?" calls `get_tax_parameters`.
- "I run a German shop that sells e-books. I made 14,000 EUR from buyers in France this year. Do I have to register for VAT, and at what rate?" calls `tax_duty`.
- "What was the IRS business mileage rate on 15 March 2026, and what is it for medical driving?" calls `get_mileage_rate`.
- "It is 1 October 2026. When is my next estimated tax payment due for the 2026 tax year?" calls `tax_deadlines`.
- "Roughly how much US federal tax would I owe in 2026 as a single filer with $100,000 of freelance income and no other income?" calls `estimate_federal_tax`.

## When not to use

- "Please prepare and e-file my 2025 tax return for me.": The plugin has published figures and a simple estimate; it never fills in or files anything.
- "How much California state income tax will I owe on $120,000 of wages?": State tax is outside the federal tables the plugin holds.
- "Register my company for VAT in Germany for me, and tell me if my toy qualifies for the reduced rate.": The plugin looks up published rules; it registers no one and holds no reduced rate tables.

## Tools

- `get_tax_parameters`: Look up US federal tax figures. Use this when the user asks for a published US federal tax figure, for example "what are the 2026 tax brackets for married filing jointly", "standard deduction for head of household in 2025", "long-term capital gains thresholds", "Social Security wage base", "401(k) contribution limit".
- `get_mileage_rate`: Look up the IRS mileage rate. Use this when the user asks for the IRS standard mileage rate on a day, for example "what was the business mileage rate on 15 March 2026", "how much per mile can I deduct for charity driving", "medical mileage rate in 2025".
- `tax_deadlines`: List US federal tax deadlines. Use this when the user asks when a US federal individual tax date falls, for example "when is the 2025 return due", "when is the extension deadline", "when is the next estimated tax payment due".
- `estimate_federal_tax`: Estimate US federal income tax. Use this when the user wants a rough US federal income tax figure, for example "how much federal tax on $90,000 of wages", "estimate my tax with $40,000 of freelance income", "what is my marginal rate".
- `tax_duty`: Check VAT or sales tax registration duty. Use this when a seller asks whether they must register for VAT or sales tax in a place, for example "I sell e-books from Germany and made 14,000 EUR to buyers in France, do I need to register?" or "my US company sells SaaS with 150,000 USD of sales to Texas, do I owe sales tax?".

## How to answer

Use get_tax_parameters for published federal figures (brackets, standard deduction, capital gains thresholds, self-employment tax, retirement limits, mileage) and quote the figure with its source document. Use get_mileage_rate for the standard mileage rate on a date, tax_deadlines for filing and estimated-payment dates, and estimate_federal_tax for a rough federal figure. Never use remembered figures: tax numbers change yearly, so always call a tool. Ask for the tax year and filing status if the user did not give them; never guess them. If a result has status unsupported_year or not_available, tell the user which years are available and do not substitute another year. Status amended means a figure changed after first release; mention it when it matters. With every estimate, say it is an estimate and not tax advice, and say what the not_included list leaves out. Use tax_duty when a seller asks whether to register for VAT or sales tax in an EU country, the United Kingdom or a US state: pass seller_country, buyer_place (write a US state as US-TX; a bare CA means Canada), product_kind and yearly_sales in the buyer place's currency (leave it out when the user gave no figure, never send 0). Quote must_register, the threshold, the rate and the filing rhythm with the source link and its checked date, and always repeat the not_tax_advice line. If status is not_covered, show the covered places; if must_register is unclear, say what is unclear and suggest an accountant. If stale_data is true, say the table is overdue for a check. Never fill a gap from memory. The other tools cover US federal income tax only: they do not cover state income tax, file or prepare a return, or advise on what the user should do.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
