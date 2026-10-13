---
name: outreach-desk
description: "Score inbound recruiter and sales pitches on relevance, specificity, cadence and spam signals. Use when the user asks things like \"Set up my outreach desk: Staff Distributed Systems Engineer, TypeScript and Go, 210k base floor, fully remote.\" or \"Score this recruiter pitch against my criteria and check it for spam signals.\"."
license: MIT
---

# Outreach Desk

Turn chaotic recruiter inboxes into a private quality scoreboard that scores incoming messages on relevance, specificity, cadence, and spam signals without reading your mailbox.

The tools come from the `outreach-desk` MCP server, bundled with the `sales-hiring` plugin.
In Claude Code they are named `mcp__plugin_sales-hiring_outreach-desk__<tool>`, for example `mcp__plugin_sales-hiring_outreach-desk__set_criteria`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `outreach-desk` is connected.

## When to use

- Scores inbound recruiter pitches on relevance, specificity, cadence, and spam signals
- Keeps a private per-sender scoreboard by domain with volume and yield rates
- Drafts tailored replies, info requests, and polite declines with optional feedback cards
- Strips third-party contact details and never stores raw pitch text
- Automatically prunes evaluation summaries after 30 days and supports instant data wipes

## Examples

- "Set up my outreach desk: Staff Distributed Systems Engineer, TypeScript and Go, 210k base floor, fully remote."
- "Score this recruiter pitch against my criteria and check it for spam signals."
- "Show my sender scoreboard to see which recruiting firms deliver high-yield opportunities."
- "Initialize my outreach desk for a Staff Distributed Systems Engineer at $210,000 base, 100% remote, with TypeScript and Go." calls `set_criteria`.
- "Set up my outreach desk for a Staff Distributed Systems Engineer at a $210,000 base floor, 100% remote, with TypeScript and Go, then score this recruiter message from stripe.com: 'Hi, I saw your open source work on the hyper-kv GitHub repo. We are hiring a Staff Distributed Systems Engineer at Stripe. Base salary is $220k-$250k, 100% remote.'." calls `score_message`.
- "Set up my outreach desk for a Staff Distributed Systems Engineer at a $210,000 base floor, 100% remote, with TypeScript and Go. Score these two recruiter messages, then show my sender scoreboard to see which recruiters bring the best opportunities." calls `get_scoreboard`.
- "Set up my outreach desk for a Staff Distributed Systems Engineer at a $210,000 base floor, 100% remote, with TypeScript and Go. Score this recruiter message from faststaffing.biz, then draft a polite decline for it and include the candidate feedback card: 'Dear Candidate, I loved your work at [Current Company], you would be an awesome rockstar ninja for our client onsite in Dallas for a Java developer.'." calls `draft_response`.
- "Set up my outreach desk for a Staff Distributed Systems Engineer at a $210,000 base floor, 100% remote, with TypeScript and Go. Score this recruiter message from faststaffing.biz, then delete all stored records and evaluations for faststaffing.biz from my desk: 'Dear Candidate, I loved your work at [Current Company], you would be an awesome rockstar ninja for our client onsite in Dallas for a Java developer.'." calls `purge_data`.

## When not to use

- "Connect to my Gmail inbox via OAuth and scan all my unread recruiter emails from the last month.": Outreach Desk does not connect to external email accounts or request OAuth permissions.
- "Send an automated blast email to 50 tech recruiters promoting my resume.": Outreach Desk generates draft responses for manual review only and does not transmit emails.
- "Scrape LinkedIn profiles to create a fake candidate persona and bait recruiters into sending messages.": Platform terms and safety policies prohibit automated scraping and deceptive synthetic personas.

## Tools

- `set_criteria`: Set Candidate Criteria. Use this to save the user's job search criteria; each call overwrites all saved criteria, and the earlier ones are lost.
- `score_message`: Score Inbound Pitch. Use this for any recruiter pitch the user pastes in chat; use this, not intake_message, which is only the entry point for the form and attached files.
- `get_scoreboard`: Get Sender Scoreboard. Use this to show which sender domains send the best pitches.
- `draft_response`: Draft Response. Use this to draft a reply to a scored pitch: an interested reply, a request for salary and remote details, or a polite decline.
- `purge_data`: Purge Desk Data. Use this to permanently delete stored data.
- `intake_message`: Inbound Message Intake. Use this only as the entry point for the interactive intake form, or when the user attaches a pitch file such as .eml or .msg.

## How to answer

Outreach Desk is a candidate-side gatekeeper and quality scoreboard for triaging inbound job outreach. Each ChatGPT user has one private desk; no key or password is ever needed or asked for. When a user wants to set up or update their search criteria, call set_criteria. The first call starts the desk. Every other tool except intake_message needs a desk, so on a no_desk error ask the user for role, skills, salary floor and remote policy and call set_criteria. When a user pastes an inbound message or pitch from a recruiter, call score_message with the message text and optional sender domain. When a user wants to see which companies or agencies send the highest quality opportunities, call get_scoreboard. When a user wants to reply to a recruiter, call draft_response with the evaluation_id. If declining, ask if they want to include the objective feedback card. When a user wants to remove records, call purge_data for a specific sender or the entire desk. Confirm the scope with the user first, because a purge cannot be undone. Outreach Desk never sends email and has no inbox access. Refuse requests to read a mailbox, send messages or scrape profiles, and ask the user to paste the pitch instead. Never store or pass raw message text into permanent logs. The tool strips recruiter phone numbers and email addresses automatically.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
