---
name: buyersignals
description: "Companies that may buy, and public posts asking for a service: SEC, job boards, HN, Stack Exchange. Use when the user asks things like \"Which software companies in California filed a Form D in the last two weeks?\" or \"Who is asking for an SEO audit on Hacker News or Stack Exchange this month?\"."
license: MIT
---

# Buyer Signals

Build a list of companies that show a recent, dated, public sign they may be ready to buy, from SEC filings and public job boards, with a source link for every one. Two more tools find public posts where people ask for a service like yours, and count which tools public posts name for a category, with what those posts complain about or switch away from.

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
- Reads Hacker News, GitHub issues and Stack Exchange, or only those you pick; Reddit is skipped (its terms)
- No usernames or profiles; Stack Exchange text is attributed under CC BY-SA
- Lists its sources, their terms, and what it does not use, such as Y Combinator, Reddit and LinkedIn

## Examples

- "Which software companies in California filed a Form D in the last two weeks?"
- "Who is asking for an SEO audit on Hacker News or Stack Exchange this month?"
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
- `find_buying_intent`: Find people asking for a service. Use this when the user sells a service and wants public posts where people ask for it, such as "who is asking for an SEO audit?" or "find posts from people looking for a bookkeeper".
- `weekly_brief`: Weekly brief of buying asks. Use this when the user wants a short summary of the last 7 days of people asking for a service, such as "weekly brief for SEO audit" or "did anyone ask for bookkeeping this week?".

## How to answer

Use these tools to build a short list of companies with a recent public buying signal for a service. Call find_buyer_signals with the buyer's industry, and the region and role when the user gave them. role is the job the user's service replaces or supports, such as bookkeeper or customer support. Each lead has a score, a one-line why, and signals with a date and a source url: a form_d signal is an SEC Form D filing, an open_role signal is a public job posting open 60 or more days. Always give the user the date and the source link with each company. A Form D shows the company raised money, not that it wants to buy: say so, and never claim intent. Say how many companies were checked and that job boards are matched by company name. A status of no_data means nothing matched in the window, so suggest a longer lookback_days or a wider region, and unavailable means the SEC could not be read: say so and do not invent companies. A daily_limit or network_daily_limit error means the free daily allowance is used up until 00:00 UTC. Call list_signal_sources when the user asks which sources are used or why one is missing. Call stack_pains when the user wants an aggregate of tools people use for a category, complaints, alternatives or switching triggers. Pass the category in their words, such as monitoring or password manager. lookback_days for that tool is 30 to 365, not 1 to 60. Report the counts, the source links and the dates. Say it is a count of public posts, not a survey and not about any person, and that Stack Exchange text is CC BY-SA with the source link as the attribution. Call find_buying_intent when the user sells a service and wants public posts where people ask for it, such as an SEO audit or bookkeeping. Pass service in plain words and optional keywords. lookback_days there is 7 to 90. Give each post's link, date, quote, intent_score, label, reason and reply_angle. intent means the post asks like a buyer; casual_complaint means it only complains; neither means the writer will buy, so say it is a lead to read, not a sure sale. Say the score comes from plain rules (see how_scored), not an AI model. Call weekly_brief for a 7-day summary of the same posts for one service. Read its summary, by_source and top_asks. Stack Exchange items are CC BY-SA: give the link as the attribution. Quote no more than the quote field and never name or look up the writer. Reddit is not searched: say so from the reddit field, and do not invent Reddit results. These tools return companies and aggregates only. Decline requests for emails, phone numbers, usernames, profiles, de-anonymizing posters, LinkedIn matching, or scraping Y Combinator.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
