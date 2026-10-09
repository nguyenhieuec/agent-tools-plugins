---
name: emailpld
description: "Email Engagement Report turns a raw email event export into a per-recipient engagement report. Use when the user asks things like \"Turn this Customer.io webhook export into a per-recipient engagement report.\" or \"Dedup these email events and list opens that are machine opens, not human opens.\"."
license: MIT
---

# Email Engagement Report

Email Engagement Report turns a raw email event export into a per-recipient engagement report. Paste a Customer.io reporting webhook file as NDJSON or JSON. SendGrid and Mailgun event objects are accepted when their field names are present.

The tools come from the `emailpld` MCP server, bundled with the `web-email` plugin.
Call them as MCP tools. Do not call the server with curl or any other shell command: the MCP tool is the supported path. In Claude Code they are named `mcp__plugin_web-email_emailpld__<tool>`, for example `mcp__plugin_web-email_emailpld__engagement_report`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `emailpld` is connected.

## When to use

- Removes repeated events by event id
- Joins events to recipients by id, delivery id or email
- Keeps machine opens and link-scanner clicks apart from human opens
- Counts unique open and unique click once per recipient per send
- Clears delivered when a bounce arrives, inside an editable time window
- Returns a summary, a recipient table, a CSV and unmatched events

## Examples

- "Turn this Customer.io webhook export into a per-recipient engagement report."
- "Dedup these email events and list opens that are machine opens, not human opens."
- "Join this event file to my recipient CSV and return the CSV plus unmatched events."
- "Turn this Customer.io NDJSON export into a per-recipient report and CSV." calls `engagement_report`.
- "Join these events to my recipient CSV and drop events with no match." calls `engagement_report`.
- "Make the recipient report for these events. Treat Apple privacy opens as impressions only, and use a 7 day window." calls `engagement_report`.
- "Make the recipient report for these events. A delivered event then a bounce should not count as delivered." calls `engagement_report`.
- "This SendGrid event export needs a per-recipient report." calls `engagement_report`.

## When not to use

- "Send a test email to the list and tell me who opened it.": The plugin has no mail platform login and does not send messages.
- "Log into Customer.io and download last month's events.": The plugin does not call Customer.io, SendGrid or Mailgun.
- "What is the capital of France?": General knowledge is outside an event report.

## Tools

- `engagement_report`: Build an email engagement report. Use this when the user pastes a Customer.io reporting webhook export, or a SendGrid or Mailgun event file, and wants a per-recipient engagement report or CSV.

## How to answer

Use engagement_report when the user pastes an email platform event export and wants a per-recipient report, a CSV, or a list of events with no recipient. Send the raw NDJSON or JSON as events. Send recipients_csv and campaigns_csv when the user provides them. Change rules only when the user names a different window, machine-open rule, or bounce rule. Show the summary first, then say how many exceptions there are, and give the CSV when the user asked for a file. Do not claim the tool called Customer.io, SendGrid or Mailgun. It only reads the pasted export. Do not store the export.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
