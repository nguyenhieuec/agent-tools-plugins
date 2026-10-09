---
name: openkrill-api
description: "YouTube transcripts, open jobs from careers pages, app store reviews and X posts. Use when the user asks things like \"Summarize https://www.youtube.com/watch?v=dQw4w9WgXcQ\" or \"What jobs does greenhouse:airbnb have open in Europe?\"."
license: MIT
---

# Agent Tools API

YouTube transcripts, open jobs from careers pages, app store reviews and X posts. Use when the user shares a YouTube link to summarize or quote, a careers page or Greenhouse, Lever or Ashby board, a Google Play or App Store app, or an x.com post link.

The tools come from the `openkrill-api` MCP server, bundled with the `content-media` plugin.
In Claude Code they are named `mcp__plugin_content-media_openkrill-api__<tool>`, for example `mcp__plugin_content-media_openkrill-api__get_transcript`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `openkrill-api` is connected.

## Examples

- "Summarize https://www.youtube.com/watch?v=dQw4w9WgXcQ"
- "What jobs does greenhouse:airbnb have open in Europe?"
- "What do recent reviews say about the Duolingo app?"

## Tools

- `get_transcript`: Full transcript of a YouTube video as text, segments, SRT or VTT, optionally translated.
- `list_jobs`: Open jobs from a Greenhouse, Lever, Ashby, Workable or SmartRecruiters board, by careers URL or "ats:slug".
- `get_app_reviews`: App metadata and recent reviews for a Google Play or App Store app.
- `read_x_post`: Read X (Twitter) posts by URL or id, without a login.
- `index_tools`: Search every Agent Tools MCP server by task, to find the right one.
- `submit_feedback`: Send the maintainers a bug, a missing tool or stale data.

## How to answer

Quote titles, channels, companies and authors as the tool returns them, and link the source URL.
A video without captions, a private app or a deleted post comes back as an error status: say so instead of guessing.
If no tool here fits the task, call `index_tools` with the task in plain words to find another Agent Tools server.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
