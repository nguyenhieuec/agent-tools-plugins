---
name: remote-jobs
description: "Remote Jobs by Country finds remote jobs a person can actually be hired for from where they live. Use when the user asks things like \"Find remote software jobs I can get from Vietnam.\" or \"Which remote customer support jobs are open to the Philippines?\"."
license: MIT
---

# Remote Jobs by Country

Remote Jobs by Country finds remote jobs a person can actually be hired for from where they live. Ask "remote software jobs I can get from Vietnam", "remote support jobs open to the Philippines" or "crypto jobs that hire in Brazil".

The tools come from the `remote-jobs` MCP server at `https://remotejobs.openkrill.app/mcp`, bundled with the `sales-hiring` plugin.
Call them as MCP tools, never from a shell. In Claude Code they are named `mcp__plugin_sales-hiring_remote-jobs__<tool>`, for example `mcp__plugin_sales-hiring_remote-jobs__find_remote_jobs`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `remote-jobs` is connected.

## When to use

- Finds remote jobs a person can be hired for from a named country
- Gives each job a verdict for the country, open or unclear, with a reason quoting the post
- Never shows a job whose post rules the country out by place or time zone
- Narrows by role family, job type, niche (crypto, AI, startup, nonprofit, academia, government) and keywords
- Links every job to its application page and its source
- Refreshed every day from public company job boards and remote job feeds

## Examples

- "Find remote software jobs I can get from Vietnam."
- "Which remote customer support jobs are open to the Philippines?"
- "Show me crypto jobs that hire from Brazil."
- "Find remote software jobs I can get from Vietnam." calls `find_remote_jobs`.
- "Which remote customer support jobs are open to the Philippines? Only ones that clearly include my country." calls `find_remote_jobs`.
- "Show me crypto jobs that hire from Brazil." calls `find_remote_jobs`.
- "Part-time remote marketing jobs I can do from India, please, up to 5." calls `find_remote_jobs`.
- "I'm a React developer in Singapore. Any remote React jobs I can apply to?" calls `find_remote_jobs`.

## When not to use

- "Find me remote jobs.": The verdict depends on the country; the tool must not guess it.
- "Apply to the first remote job for me.": The plugin only reads public job posts and cannot apply.
- "What jobs is Stripe hiring for in its office in Dublin?": find_remote_jobs indexes remote jobs only and answers by country, not by company office.

## Tools

- `find_remote_jobs`: Find remote jobs open to a country. Find remote jobs a person can be hired for from a country.

## How to answer

Use find_remote_jobs when the user wants remote jobs they can be hired for from a country. Pass the country they will work from; ask for it when it is unknown, never guess it. Set family, type, niche and keywords only from the user's words. Show each job's title, company, verdict and reason, and its applyUrl. Say that open means the post includes the country or hires from anywhere, and unclear means the post names no limit, so the user should check with the employer before applying. When a job has via, credit that source by name with its link. The verdict reads the post's own words and is not a promise of being hired. Do not apply for the user.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
