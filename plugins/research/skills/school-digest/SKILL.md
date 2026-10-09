---
name: school-digest
description: "School Digest makes one printable daily page per child from snapshots you supply. Use when the user asks things like \"Make today's school page from this Canvas snapshot.\" or \"Compare these two school snapshots and list what changed.\"."
license: MIT
---

# School Digest

School Digest makes one printable daily page per child from snapshots you supply.

The tools come from the `school-digest` MCP server, bundled with the `research` plugin.
Call them as MCP tools. Do not call the server with curl or any other shell command: the MCP tool is the supported path. In Claude Code they are named `mcp__plugin_research_school-digest__<tool>`, for example `mcp__plugin_research_school-digest__make_school_digest`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `school-digest` is connected.

## When to use

- Builds one printable daily page per child from a Canvas snapshot you supply
- Adds Infinite Campus grades and school notice emails when you include them
- Lists due today, the next 7 days, missing and late work, and recent scores
- Compares with yesterday's snapshot and lists what changed
- Applies fixed attention rules and prints an optional 3-line summary you write
- Stores no login, no token and no student data

## Examples

- "Make today's school page from this Canvas snapshot."
- "Compare these two school snapshots and list what changed."
- "Add these 3 summary lines to the school page for River."
- "Make today's school page from this Canvas snapshot for 2026-10-08." calls `make_school_digest`.
- "Compare this Canvas snapshot with yesterday's and list what changed." calls `make_school_digest`.
- "Add this Infinite Campus grade snapshot and these school notice emails to the page." calls `make_school_digest`.
- "Here are 3 summary lines for River Sample. Put them on the school page." calls `make_school_digest`.
- "What is due in the next 7 days, and what is missing, in this Canvas snapshot?" calls `make_school_digest`.

## When not to use

- "Log in to Canvas with this password and pull the grades.": The plugin has no login and must not accept a password or a token.
- "Store this student's grades so I can see them tomorrow.": The plugin stores no student data.
- "What is the capital of France?": General knowledge is outside a school page tool.

## Tools

- `make_school_digest`: Make a daily school page. Use this when the user wants a printable daily school page from a Canvas snapshot they already have, for example "make today's page from this Canvas JSON".

## How to answer

Use make_school_digest when the user supplies a Canvas snapshot, an Infinite Campus grade snapshot, or school notice emails and wants a daily page. Send date, run_at, timezone and canvas. Send infinite_campus, parentsquare_emails and previous when the user has them. Do not ask for or send a token or a password. Show page_text. The printable page is html. If the user wants a 3-line summary, write 3 short lines from the child's attention and changes, using only those facts, and send them as summaries to print them. Do not say the tool called a model. Do not say any data was stored.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
