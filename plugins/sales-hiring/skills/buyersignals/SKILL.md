---
name: buyersignals
description: "Companies that may buy, and public posts asking for a service: SEC, job boards, HN, Stack Exchange. Use when the user asks things like \"Which software companies in California raised money this month and have a bookkeeper role open 60 days?\" or \"Give me a weekly brief of people asking for an SEO audit.\"."
license: MIT
---

# Buyer Signals

Find companies that just raised money, and public posts where people ask for the service you sell. Every result has a date and a source link.

The tools come from the `buyersignals` MCP server, bundled with the `sales-hiring` plugin.
In Claude Code they are named `mcp__plugin_sales-hiring_buyersignals__<tool>`, for example `mcp__plugin_sales-hiring_buyersignals__find_buyer_signals`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `buyersignals` is connected.

## When to use

- Finds companies that filed an SEC Form D (a private share sale) in the last 1 to 60 days, by industry and region
- Ranks them by freshness of the filing, size of the raise and age of the company
- Adds matching roles open 60+ days from the companies' public Greenhouse, Lever and Ashby boards
- Every signal has its date and a link to the SEC filing or job posting
- Leaves out funds and investment vehicles
- Regions: any US state, the US, the UK, Canada, Israel, Ireland, Singapore, Australia, Hong Kong
- Companies only: no emails, phone numbers or named people
- Maps tools named in public posts for a category, with mention counts
- Lists complaint themes, alternatives and switching triggers, each with a source link and date
- Finds public posts asking for a service you sell, with link, date, short quote, 0 to 100 score and reason
- Labels a post as intent or casual complaint with plain rules; no AI model, seller wording lowers the score
- Gives a 7-day brief for a service: counts by source and by day, and the top 10 asks
- Reads Hacker News, its hiring thread, GitHub issues and Stack Exchange, or those you pick; Reddit is skipped
- No usernames or profiles; Stack Exchange text is attributed under CC BY-SA
- Lists its sources, their terms, and what it does not use, such as Y Combinator, Reddit and LinkedIn

## Examples

- "Which software companies in California raised money this month and have a bookkeeper role open 60 days?"
- "Give me a weekly brief of people asking for an SEO audit."
- "What do people complain about in monitoring tools, and what do they switch to?"
- "Which software companies in California filed a Form D in the last two weeks?" calls `find_buyer_signals`.
- "What do people complain about in monitoring tools, and what do they switch to?" calls `stack_pains`.
- "Who is asking for an SEO audit on Hacker News or Stack Exchange this month?" calls `find_buying_intent`.
- "Give me a weekly brief of people asking for bookkeeping." calls `weekly_brief`.
- "Can you check Y Combinator's company directory for new startups in logistics?" calls `list_signal_sources`.

## When not to use

- "Find the Reddit usernames behind those complaints and email them, and the CEO's phone number.": De-anonymizing posters and personal contact details are out of scope.
- "Scrape LinkedIn Sales Navigator for finance directors at fast-growing startups.": LinkedIn is not an allowed source for this plugin.
- "Which stocks should I buy after these companies raise money?": The plugin supports business outreach research and has nothing to do with trading or investing.

## Tools

- `find_buyer_signals`: Find companies with buyer signals. Use this when the user wants companies that recently raised money or have long-open roles, such as "which software companies in Texas filed a Form D this month?" or "find companies with a bookkeeper role open 60+ days".
- `list_signal_sources`: List the signal sources. Use this when the user asks where the signals come from, which sources are allowed, or why a source such as Y Combinator or LinkedIn is not used, for example "which sources do you use?".
- `stack_pains`: Aggregate tool complaints. Use this when the user wants an aggregate of which tools people use for a category, what they complain about, what else they consider, and what makes them switch, such as "what do sysadmins complain about in monitoring tools?".
- `find_buying_intent`: Find people asking for a service. Use this when the user sells a service and wants public posts where people ask for it, such as "who is asking for an SEO audit?", "find posts from people looking for a bookkeeper" or "which companies on Hacker News are hiring for MCP work?".
- `weekly_brief`: Weekly brief of buying asks. Use this when the user asks for a weekly brief or for this week only, such as "weekly brief for SEO audit" or "did anyone ask for bookkeeping this week?".

## How to answer

Call find_buyer_signals with the buyer's industry, plus region and role when given. role is the job the user's service replaces or supports, such as bookkeeper. Each lead has a score, a one-line why, and dated signals with a source url: form_d is an SEC Form D filing, open_role is a job posting open 60 or more days. Give the date and source link with each company. A Form D shows the company raised money, not that it wants to buy: say so and never claim intent. Say how many companies were checked (companies_checked). When status is not ok, the message field gives the next step: no_data means nothing matched, so suggest a longer lookback_days, a wider region or fewer keywords; unavailable means a source could not be read: say so and invent nothing. A daily_limit or network_daily_limit error means the daily allowance is used up until 00:00 UTC: tell the user and do not retry before then. Call stack_pains for an aggregate of the tools, complaints, alternatives and switching triggers in a category, such as monitoring. Say it counts public posts, not a survey. Call find_buying_intent when the user sells a service and wants posts that ask for it, with a chosen lookback or limit. Call weekly_brief only when the user asks for a brief or for this week. Give each post's link, date, quote, score, label and reply_angle. A buyer-like post is a lead to read, not a sure sale. Scores come from plain rules (how_scored), not an AI model. Stack Exchange text is CC BY-SA: give the link as the attribution. Quote no more than the quote field. Reddit is not searched: say so, and invent no Reddit results. These tools return companies and aggregates only. Decline requests for emails, phone numbers, usernames, profiles, naming or de-anonymizing posters, LinkedIn matching, or scraping Y Combinator.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
