---
name: industry-pulse
description: "What happened in AI coding agents like Claude Code, Codex, Cursor and Copilot, and what people say. Use when the user asks things like \"What is new in AI coding agents today?\" or \"What happened with Claude Code this week?\"."
license: MIT
---

# Industry Pulse

Ask what happened in AI coding agents (Claude Code, Codex, Cursor, Copilot, Gemini CLI, Cline, Aider and others) and what people say about a product.

The tools come from the `industry-pulse` MCP server, bundled with the `dev-security` plugin.
In Claude Code they are named `mcp__plugin_dev-security_industry-pulse__<tool>`, for example `mcp__plugin_dev-security_industry-pulse__latest_events`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `industry-pulse` is connected.

## When to use

- Lists the latest AI coding agent news as deduplicated events with a score and a heat count
- Shows which sources reported each event, with links and dates
- Writes a daily digest with a short overview of the day
- Searches the last 30 days of events for a product, company or topic
- Counts the top complaints and praises for a product from Hacker News, GitHub issues and App Store reviews
- Reports when the sources were last read, so you can judge how fresh an answer is

## Examples

- "What is new in AI coding agents today?"
- "What happened with Claude Code this week?"
- "What do people complain about with Cursor?"
- "What is new in AI coding agents today?" calls `latest_events`.
- "What happened with Claude Code this week?" calls `search_events`.
- "Give me yesterday's digest for AI coding agents." calls `daily_digest`.
- "What do people complain about with Cursor?" calls `product_feedback`.
- "Is GitHub Copilot well liked? What do users praise?" calls `product_feedback`.

## When not to use

- "Write a function that parses an RSS feed in TypeScript.": A coding task asks for no events or product feedback to be read.
- "Should I buy Nvidia stock after today's AI news?": The plugin reports developer-tool news and feedback, not market advice.
- "What are people saying about me on X and Reddit?": The plugin does not read X or Reddit and does not look up people.

## Tools

- `latest_events`: Get the latest AI coding agent events. List recent events in AI coding agents, ranked by score and heat.
- `daily_digest`: Get a day's AI coding agent digest. Get one UTC day's events with a short overview paragraph.
- `search_events`: Search AI coding agent events. Search stored events by words.
- `product_feedback`: Get a product's top complaints and praises. Count what people complain about and praise for a product.

## How to answer

Use latest_events for what is new (hours defaults to 24), search_events when the user names a product, company or topic, and daily_digest for a day's overview (date is a UTC day, default the last finished day). Each event has a title, a short summary, a score from 0 to 10, a heat count (independent sources within 48 hours) and source links: link the sources when you cite an event and say when the data was read (asOf). Events scored by rules instead of the model say so in scoring. If note says the first collection is still running, tell the user to try again in a few minutes and do not invent news. Use product_feedback when the user asks what people complain about or praise for a product; the counts are from a sample of recent public posts, not a survey, so say that and read the sources list: a source with status unavailable or not_covered was not read. Never present these results as investment, hiring or legal advice.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
