---
name: dev-facts
description: "Software end-of-life dates, open-source licenses, browser support for web features and RFC facts. Use when the user asks things like \"Is Node.js 20 still supported, and when does it reach end of life?\" or \"Is the MIT license OSI approved?\"."
license: MIT
---

# Dev Facts Check

Check the facts a developer asks for again and again: when a software version reaches end of life, what an open-source license is, whether a web feature works in a browser and what an RFC says about itself.

The tools come from the `dev-facts` MCP server, bundled with the `dev-security` plugin.
In Claude Code they are named `mcp__plugin_dev-security_dev-facts__<tool>`, for example `mcp__plugin_dev-security_dev-facts__get_eol_status`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `dev-facts` is connected.

## When to use

- Shows whether a version of a product has reached end of life, with its end-of-life and LTS dates
- Lists the release cycles of a product that are still supported, newest first, with the latest patch
- Looks up an SPDX license by identifier or name: OSI approved, FSF free, deprecated or not
- Shows the first release of each major browser that supports a web feature, and its Baseline status
- Looks up an RFC by number or title words: status, abstract, and the RFCs that obsolete or update it
- Names the source and the date of its data in every answer

## Examples

- "Is Node.js 20 still supported, and when does it reach end of life?"
- "Is the MIT license OSI approved?"
- "Can I use CSS container queries in Safari?"
- "Is Node.js 20 still supported and when does it reach end of life?" calls `get_eol_status`.
- "What is the latest Python release and which versions get security fixes?" calls `get_eol_status`.
- "Is the MIT license OSI approved and what is its SPDX id?" calls `check_license`.
- "Can I use CSS container queries in Safari? Is it Baseline?" calls `check_browser_support`.
- "Which RFC obsoletes RFC 2616?" calls `lookup_rfc`.

## When not to use

- "Is express@4.17.1 vulnerable?": Package vulnerabilities and health belong to Package Health Check; this plugin covers versions, end of life, licenses and browser support.
- "Explain how React hooks work.": The plugin looks up dated reference facts; it does not teach programming.
- "Write a license for my project.": The plugin identifies existing SPDX licenses; it does not write or advise on licenses.

## Tools

- `get_eol_status`: Get end-of-life status. Use this when the user asks whether a software version is still supported or when it reaches end of life, such as "is Node.js 20 still supported?" or "which Python versions get security fixes?".
- `check_license`: Check a license. Use this when the user asks about an open-source license, such as "is the MIT license OSI approved?" or "what is the SPDX id of Apache License 2.0?".
- `check_browser_support`: Check browser support. Use this when the user asks whether a web platform feature works in browsers, such as "can I use CSS container queries in Safari?" or "is subgrid Baseline?".
- `lookup_rfc`: Look up an RFC. Use this when the user asks about an RFC, such as "which RFC obsoletes RFC 2616?" or "find the RFC about WebSocket".

## How to answer

Use these tools for developer reference facts: get_eol_status for support and end-of-life dates of a software product or version, check_license for an SPDX license's identifier and OSI or FSF status, check_browser_support for Baseline status and browser versions of a web platform feature, and lookup_rfc for an RFC by number or by words in its title. Pass one product, license, feature or RFC per call. When a result has status not_found, ambiguous or version_not_found, read error.hint and the candidates, then call again with one of them instead of guessing. Always give the source and asOf date with the answer. endoflife.date is community maintained: say so when the answer drives a decision. A license answer is not legal advice and says nothing about what a license permits. Do not use these tools for package vulnerabilities or package health: say this plugin does not cover them.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
