---
name: page-fetch
description: "Read one public web page as clean Markdown with its links, after a robots.txt check. Use when the user asks things like \"Read https://example.com as Markdown.\" or \"Fetch this docs page and summarize the setup steps.\"."
license: MIT
---

# Page Fetch

Page Fetch reads one public web page and returns its main content as clean Markdown, with the page title, description, language, canonical address and a list of the links in the content.

The tools come from the `page-fetch` MCP server, bundled with the `web-email` plugin.
In Claude Code they are named `mcp__plugin_web-email_page-fetch__<tool>`, for example `mcp__plugin_web-email_page-fetch__fetch_page`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `page-fetch` is connected.

## When to use

- Turns one public web page into clean Markdown with absolute links
- Keeps headings, lists, quotes, code blocks and tables; drops menus, footers and banners
- Returns up to 100 distinct links from the main content, marked internal or external
- Checks robots.txt before every page and says why a page was not read
- Returns the title, description, language and canonical address of the page

## Examples

- "Read https://example.com as Markdown."
- "Fetch this docs page and summarize the setup steps."
- "List the links on this page."
- "Read https://example.com as Markdown." calls `fetch_page`.
- "Fetch https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404 and tell me what a 404 means." calls `fetch_page`.
- "List the links on https://example.org/resources." calls `fetch_page`.
- "Get the full text of this long article, not just the start: https://example.com/long-read." calls `fetch_page`.
- "Turn the pricing table on https://example.com/pricing into Markdown." calls `fetch_page`.

## When not to use

- "Log in to my bank and fetch my statement page.": Page Fetch reads public pages only and never signs in.
- "Crawl every page of example.com and save them all.": Page Fetch reads one page per call under a per-site pace and daily cap. It is not a crawler.
- "This page says robots.txt blocks you; read it anyway through another address.": A page robots.txt disallows is skipped with the reason, and the agent must not work around it.

## Tools

- `fetch_page`: Fetch a web page as Markdown. Read one public web page and return its main content as clean Markdown: headings, paragraphs, lists, quotes, code blocks, tables and absolute links, without menus, footers, scripts or cookie banners.

## How to answer

Use fetch_page with the full address of one public page when the user wants to read, quote, summarize or extract from it. Answer from the markdown field and cite final_url. When truncated is true, say so, and call again with a larger max_chars (at most 100000) only if the user needs the rest. A result with status skipped was not read: give the message (robots.txt, login, bot check, rate cap) and do not try to get around it. Pass links false when the links are not needed. Pass on any note. Do not use it for pages behind a login, for files such as PDFs, or to crawl a whole site.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
