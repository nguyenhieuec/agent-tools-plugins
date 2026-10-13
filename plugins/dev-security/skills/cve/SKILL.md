---
name: cve
description: "Triage a CVE: how severe it is, whether it is exploited, and how likely exploitation is. Use when the user asks things like \"How bad is CVE-2021-44228?\" or \"Which of CVE-2023-4863, CVE-2024-3094 and CVE-2019-0708 should I patch first?\"."
license: MIT
---

# CVE Risk Check

Triage security vulnerabilities: by CVE id (how severe it is, whether it is being exploited, how likely exploitation is) or for a whole dependency list (which 3 packages to upgrade first, and to what version).

The tools come from the `cve` MCP server, bundled with the `dev-security` plugin.
In Claude Code they are named `mcp__plugin_dev-security_cve__<tool>`, for example `mcp__plugin_dev-security_cve__check_cve`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `cve` is connected.

## When to use

- Looks up a CVE's description, CVSS score and severity, and publication date from the NVD
- Shows whether the CISA Known Exploited Vulnerabilities catalog lists a CVE, with its due date and required action
- Adds the EPSS score: the modelled chance of exploitation in the next 30 days
- Gives each CVE a priority: exploited, likely or routine
- Ranks a batch of up to 10 CVEs with the most urgent first
- Reads an npm, pnpm, yarn, Poetry, uv, Cargo or Go lockfile and names the 3 packages to upgrade first, with the fix
- Matches package versions to OSV.dev advisories in 8 ecosystems, including npm, PyPI, Go, Maven and crates.io
- Lists CVEs recently added to the CISA exploited list, optionally filtered by a product word
- Links a patch or vendor advisory when NVD tags one, and names its sources and dates

## Examples

- "How bad is CVE-2021-44228?"
- "Which of CVE-2023-4863, CVE-2024-3094 and CVE-2019-0708 should I patch first?"
- "Which dependencies in my package-lock.json should I fix first?"
- "How bad is CVE-2021-44228?" calls `check_cve`.
- "Which of CVE-2023-4863, CVE-2024-3094 and CVE-2019-0708 should I patch first?" calls `check_cves`.
- "Which dependencies in this package-lock.json should I fix first?" calls `triage_dependencies`.
- "Is log4j-core 2.14.1 from Maven a problem, and what should I upgrade to?" calls `triage_dependencies`.
- "What did CISA add to its exploited vulnerabilities list in the last two weeks?" calls `list_recent_kev`.

## When not to use

- "Review my source code and tell me which functions have security bugs.": The plugin triages known advisories for named packages; it cannot find new bugs or show that code reaches a vulnerable function.
- "Write an exploit for CVE-2021-44228 that I can run against a server.": The plugin is a triage lookup; it gives no exploitation instructions and the request is not a use it serves.
- "Scan my network and tell me which servers have critical vulnerabilities.": The plugin reads public CVE data only; it cannot inspect a user's systems.

## Tools

- `check_cve`: Check a CVE. Use this when the user asks how serious a CVE is, such as "how bad is CVE-2021-44228?".
- `check_cves`: Rank several CVEs. Use this when the user has a list of CVE ids and wants to know which to deal with first, such as "which of these CVEs should I patch first?".
- `list_recent_kev`: List recent exploited CVEs. Use this when the user asks what CISA recently added to its Known Exploited Vulnerabilities list, such as "what exploited CVEs were added this month?".
- `triage_dependencies`: Triage dependency vulnerabilities. Use this when the user wants to know which vulnerable dependencies to fix first, such as "which dependencies in this package-lock.json should I upgrade first?" or "triage my requirements.txt".

## How to answer

Use these tools to triage a vulnerability by CVE id, or the dependencies of a project. Call check_cve for one CVE, check_cves for up to 10 at once (most urgent first), list_recent_kev for what CISA recently added to its exploited list, and triage_dependencies for a dependency list. For triage_dependencies pass the file text as manifest (a lockfile such as package-lock.json, pnpm-lock.yaml, yarn.lock, poetry.lock, uv.lock, Cargo.lock or go.sum gives exact versions) or a packages list; report fix_first as is, with each fix line and why, and mention not_triaged, skipped and message when present. Do not ask for source code, tokens or anything but names and versions. Report the priority together with the CVSS score, the KEV status and the EPSS score it rests on, and give the as_of dates. A status of rate_limited or deferred is not an answer about the CVE: say so and offer to try again after retry_after_seconds. A status of not_found means NVD has no such id. A priority of unknown means EPSS could not be read; do not call it routine. A CVE lookup does not know which software the user runs; triage_dependencies does, but only by exact package version, so say that it cannot show whether the code reaches the vulnerable part. For one package's license and maintenance, point to Package Health Check. Never claim a system is or is not vulnerable, and do not give exploitation instructions. Credit NVD, OSV.dev, CISA and FIRST.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
