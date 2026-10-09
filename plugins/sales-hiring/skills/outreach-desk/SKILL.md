---
name: outreach-desk
description: "Score inbound recruiter and sales pitches on relevance, specificity, cadence and spam signals. Use when the user asks things like \"Initialize my outreach desk for a Staff Distributed Systems Engineer role at $210k+ remote.\" or \"Score this inbound recruiter pitch against my criteria and check for spam signals.\"."
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

- "Initialize my outreach desk for a Staff Distributed Systems Engineer role at $210k+ remote."
- "Score this inbound recruiter pitch against my criteria and check for spam signals."
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

- `set_criteria`: Set Candidate Criteria. Use this to configure or update your target role, core skills, minimum salary floor, remote policy, and dealbreakers on your private outreach desk.
- `score_message`: Score Inbound Pitch. Use this to evaluate an inbound recruiter pitch or outreach message across relevance, specificity, cadence, and spam signals.
- `get_scoreboard`: Get Sender Scoreboard. Use this to view your private sender domain scoreboard, yield rates, and recent pitch evaluations.
- `draft_response`: Draft Response. Use this to generate a tailored reply draft, inquiry, or polite decline with an optional standardized feedback card.
- `purge_data`: Purge Desk Data. Use this to wipe records for a specific sender domain or permanently delete your entire desk and all associated history.
- `intake_message`: Inbound Message Intake. Use this to open the interactive inbound pitch intake form in ChatGPT, or evaluate and stage an incoming recruiter pitch.

## How to answer

Outreach Desk is a candidate-side gatekeeper and quality scoreboard for triaging inbound job outreach. When a user wants to set up or update their search criteria, call set_criteria. If they have an existing desk_key, pass it; otherwise omit it to generate a new bearer key. When a user pastes an inbound message or pitch from a recruiter, call score_message with the desk_key, message text, and optional sender domain. When a user wants to see which companies or agencies send the highest quality opportunities, call get_scoreboard. When a user wants to reply to a recruiter, call draft_response with the evaluation_id. If declining, ask if they want to include the objective feedback card. When a user wants to remove records, call purge_data for a specific sender or the entire desk. Never store or pass raw message text into permanent logs. The tool strips recruiter phone numbers and email addresses automatically.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
