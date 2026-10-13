---
name: hire
description: "Split a job into work AI can do now, work AI drafts for a person to approve, and work for a person. Use when the user asks things like \"Which tasks of a bookkeeper can AI do, and which need a person?\" or \"Split a software engineer role into AI, draft, and human work.\"."
license: MIT
---

# Hire or Automate

Split a job into work AI can do now, work AI should draft for a person to approve, and work that needs a person, before you hire.

The tools come from the `hire` MCP server, bundled with the `sales-hiring` plugin.
In Claude Code they are named `mcp__plugin_sales-hiring_hire__<tool>`, for example `mcp__plugin_sales-hiring_hire__split_role`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `hire` is connected.

## When to use

- Matches a job title to O*NET occupations by official title, then alternate title, then word overlap
- Tags each O*NET task as AI now, AI drafts for approval, or needs a person, with the rule
- Returns a job-ad skeleton for the human tasks and a two-week trial template
- Asks a language model only to break a tie, inside a daily cap, and does not store the job text

## Examples

- "Which tasks of a bookkeeper can AI do, and which need a person?"
- "Split a software engineer role into AI, draft, and human work."
- "Write a job ad for the human part of a restaurant cook, plus a two-week trial."
- "Which tasks of a bookkeeper can AI do, and which need a person?" calls `split_role`.
- "Split a software engineer role into what AI can draft and what a person still has to approve." calls `split_role`.
- "Look up O*NET 29-1141.00 and tell me which nursing tasks need a person." calls `split_role`.
- "Write a job ad for the human part of a restaurant cook, and a two-week trial." calls `split_role`.
- "We are hiring a senior software engineer. Which of the standard tasks can we stop hiring for?" calls `split_role`.

## When not to use

- "Should I hire Jane Doe or fire the current bookkeeper?": The plugin has no candidate data and must not be used as a hiring or firing decision.
- "Pull this candidate's resume from LinkedIn and tell me if they can do the job.": Job text is accepted only as a title or a description the user pastes, with no personal data, and nothing is fetched about a person.
- "What stocks should our financial analyst buy this week?": The plugin splits occupational tasks. It is not a market or trading tool.

## Tools

- `split_role`: Split a role into AI and human work. Use this when the user wants to split a whole job into work AI can do now, work AI drafts for approval, and work that needs a person.

## How to answer

Use split_role when the user wants to know which parts of a job AI can do, which it should only draft, and which need a person, or wants a job ad or a two-week trial for the human part. Pass the job title, or an O*NET-SOC code such as 15-1252.00. Pass a job description only when the title is vague, and never include a person's name, email, phone number or resume. Do not call it to judge a named candidate, to hire or fire someone, or to give trading, legal or pay advice. Read match.method and match.confidence. If status is no_match, say so and ask for a clearer title or a code; do not invent tasks. If status is personal_data, ask for the role without contact details. Quote the pile and the rule with each task you mention. Include the source attribution in the answer. Say it is a planning aid, not a decision about any person. If the human pile is empty, say not to hire for the listed tasks.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
