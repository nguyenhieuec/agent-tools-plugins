---
name: trending
description: "The posts rising fastest right now on Hacker News, GitHub, Product Hunt and more, for any topic. Use when the user asks things like \"What is trending in AI in the last 6 hours?\" or \"Which new GitHub repos and Product Hunt launches are rising today?\"."
license: MIT
---

# Trending Now

The posts rising fastest right now on Hacker News, GitHub, Product Hunt and more, for any topic. One call reads Hacker News, DEV Community, GitHub, Product Hunt, Lobsters and YouTube, for a topic and a time window. Writers, marketers and founders use it to pick what to write or post about today instead of keeping 20 tabs open.

The tools come from the `trending` MCP server, bundled with the `content-media` plugin.
In Claude Code they are named `mcp__plugin_content-media_trending__<tool>`, for example `mcp__plugin_content-media_trending__trending`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `trending` is connected.

## When to use

- Lists the fastest-rising posts across Hacker News, DEV Community, GitHub, Product Hunt, Lobsters and YouTube
- Filters by a niche or by words, and by a window from 1 hour to 7 days
- Gives each post its count, gain per hour and a 0 to 100 heat score within its source
- Lists rising keywords with the percent change of their share of titles, 3 days against the 3 before
- Daily digest per niche: top posts overall and the top 5 per source
- Says which sources were read, when, and which failed
- Official APIs and public feeds only; Reddit and X are not read
- Titles and links only: no post text, author names or accounts

## Examples

- "What is trending in AI in the last 6 hours?"
- "Which new GitHub repos and Product Hunt launches are rising today?"
- "Give me today's content ideas digest for marketing."
- "What is trending in AI in the last 6 hours?" calls `trending`.
- "Which posts about postgres are rising this week?" calls `trending`.
- "Which new GitHub repos are rising today?" calls `trending`.
- "Give me today's content ideas digest for marketing." calls `trending_digest`.
- "What is on the trend digest across everything right now?" calls `trending_digest`.

## When not to use

- "What is trending on Reddit and X about our brand?": Reddit and X are not read under their terms.
- "Write a tweet thread about TypeScript generics.": Writing asks for no trend data.
- "Who wrote the top Hacker News post today and what is their email?": The store keeps no author names or contact details.

## Tools

- `trending`: Rising posts for a topic. Use this when the user wants what is trending or rising right now, such as "what is trending in AI today?" or "rising posts about postgres this week".
- `trending_digest`: Daily trend digest for a niche. Use this when the user wants a daily digest or content ideas for a niche, such as "give me today's AI digest" or "content ideas for marketing".

## How to answer

Call trending when the user wants what is popular or rising now. Pass topic as a niche slug or the user's own words, and window (1h, 6h, 24h, 3d, 7d; default 24h). Give each post's title, link, source, and its count with unit and velocity_per_hour. Say heat compares a post only with others from the same source. velocity_basis since_last_read is measured between two reads; since_published is the average since posting; list_order means the source gives no count (Product Hunt). Quote rising_keywords with change_percent only when keywords_baseline is ready; while it is building, give the post counts and say the comparison needs six days of data. Call trending_digest for a niche's daily digest or content ideas, and give its page link. If status is warming, say the first read is running and to ask again in a minute. If no_posts, suggest a longer window or a wider topic. Name any source whose status is failed. Reddit and X are not read: say so from not_read and never invent posts from them. Reddit is not read: its Data API terms need a separate agreement for commercial use. Answers hold titles and links only; do not invent post text, authors or counts.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
