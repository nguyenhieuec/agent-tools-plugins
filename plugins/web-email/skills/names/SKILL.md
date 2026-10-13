---
name: names
description: "Check whether the domain for a startup, product or business name is already registered. Use when the user asks things like \"Is acme.com available?\" or \"Check if the startup name Lumenly is taken\"."
license: MIT
---

# Startup Name Check

Check whether the domain for a startup, product or business name is already registered before you fall in love with it.

The tools come from the `names` MCP server, bundled with the `web-email` plugin.
In Claude Code they are named `mcp__plugin_web-email_names__<tool>`, for example `mcp__plugin_web-email_names__check_domain_availability`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `names` is connected.

## When to use

- Checks whether a name's domain is registered in .com, .net, .org, .ai, .app and .dev
- Checks a full domain such as acme.com on its own
- Compares up to five candidate names in one answer
- Shows registration and expiry dates for domains that are taken, when the registry publishes them
- Shows the registration and renewal cost when Cloudflare Registrar returns one
- Never returns owner or contact details

## Examples

- "Is acme.com available?"
- "Check if the startup name Lumenly is taken"
- "Find an available .com for my bakery name, Rise & Crumb"
- "Check if the startup name zzqxnowherewidgets123 is taken." calls `check_domain_availability`.
- "Find an available .com for my bakery name: Rise & Crumb, Crumb Theory or zzqxnowherebakery123." calls `check_domain_availability`.
- "Are openai.ai and wikipedia.org registered, and when do they expire?" calls `check_domain_availability`.
- "Is the .dev domain for the name Stripe taken?" calls `check_domain_availability`.

## When not to use

- "Register acme.com for me and point it at my website.": The plugin is read-only: it cannot buy, reserve or configure domains.
- "Who owns the domain acme.com? I want to email them.": Registrant contact data is deliberately never returned.
- "Is the name Acme trademarked in the US?": The plugin only checks domain registration, not trademark registers.

## Tools

- `check_domain_availability`: Check name and domain availability. Use this when the user asks whether a domain or startup name is available or taken, for example "is acme.com available", "check if this startup name is taken" or "find an available .com for my bakery name".

## How to answer

Use check_domain_availability when the user asks whether a domain, startup name or business name is available or taken. Pass names as the user wrote them, or full domains such as acme.com to check only that domain; set tlds when the user wants specific endings ("a .com" means tlds ["com"]). When asked to find an available domain, brainstorm up to five candidate names and check them together. Always say the result is indicative and not a reservation, and that trademarks were not checked. Give the source and asOf date. When registrationCost is present, state that cost and that it is not a charge. A status of unsupported means the ending has no public registration data; unavailable means the lookup did not answer, so say the result is unknown. Never guess who owns a domain, and never offer to register, buy or reserve one.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
