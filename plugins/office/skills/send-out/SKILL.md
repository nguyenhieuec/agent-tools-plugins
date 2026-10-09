---
name: send-out
description: "Turn assistant output into ready-to-send action links and files, with no sign-in. Use when the user asks things like \"Draft an email to the project team about status updates\" or \"Make a calendar event for Sprint Review next Monday at 10am\"."
license: MIT
---

# Send Out

Turn assistant output into ready-to-send action links and files with zero sign-in and zero data custody liability.

The tools come from the `send-out` MCP server at `https://sendout.openkrill.app/mcp`, bundled with the `office` plugin.
Call them as MCP tools, never from a shell. In Claude Code they are named `mcp__plugin_office_send-out__<tool>`, for example `mcp__plugin_office_send-out__draft_email`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `send-out` is connected.

## When to use

- Creates mailto draft links and RFC 822 .eml files ready to open in email clients
- Creates Google Calendar links and RFC 5545 .ics calendar files with reminders
- Creates WhatsApp wa.me click-to-chat links with normalized phone numbers
- Creates Obsidian obsidian://new deep links and Markdown notes
- Creates X post share links checked against character limits
- Generates Markdown files with structured YAML frontmatter
- Stateless and keyless with zero data storage and zero server-side sending

## Examples

- "Draft an email to the project team about status updates"
- "Make a calendar event for Sprint Review next Monday at 10am"
- "Create a WhatsApp link with a customer support follow-up message"
- "Draft an email to alex@example.com with subject 'Project Milestone' saying 'Phase 1 is complete and ready for review.'" calls `draft_email`.
- "Create a calendar event for 'Product Launch' on 2026-11-15T14:00 with duration 60 minutes in Europe/London." calls `create_calendar_event`.
- "Create a WhatsApp link for phone '+1 (555) 234-5678' with message 'Your appointment is confirmed for Tuesday at 3pm.'" calls `create_whatsapp_link`.
- "Export a research note titled 'Edge Compute Patterns' to my 'TechNotes' Obsidian vault with tags ['architecture', 'edge']." calls `create_obsidian_note`.
- "Create an X post: 'Excited to ship our new open source CLI today! Check it out at https://example.com' with hashtag #devtools." calls `create_x_post`.

## When not to use

- "Send an automated cold direct message on Instagram to 50 prospective clients.": Meta Graph API strictly limits direct messaging and OpenAI policies prohibit unsolicited automated outreach.
- "Send a private LinkedIn message to this recruiter asking about open positions.": LinkedIn User Agreement Section 8.2 explicitly bans automated communication plugins and bot messaging.
- "Blast a marketing email newsletter to a mailing list of 5,000 addresses.": Send Out is a stateless personal dispatch bridge with zero server-side relay, designed to prevent spam and enforce human-in-the-loop review.

## Tools

- `draft_email`: Draft email. Creates a mailto draft link and an RFC 822 .eml file.
- `create_calendar_event`: Create calendar event. Creates a Google Calendar web intent link and an RFC 5545 .ics calendar file.
- `create_whatsapp_link`: Create WhatsApp link. Creates an official WhatsApp click-to-chat (wa.me) link.
- `create_obsidian_note`: Create Obsidian note. Creates an obsidian://new URI and formatted Markdown text.
- `create_x_post`: Create X post. Creates an official X (Twitter) web share intent link.
- `create_markdown_file`: Create Markdown document. Creates a standard Markdown document with structured YAML frontmatter.
- `prepare_transfer`: Prepare file transfer. Prepares an interactive file transfer card and secure download link for any text or document export.

## How to answer

Use Send Out when the user wants to take generated content and send or export it to an external destination. Use draft_email when the user wants an email draft or .eml file. Use create_calendar_event when the user wants to schedule an event or download an .ics calendar file. Use create_whatsapp_link when the user wants to send a WhatsApp message. Use create_obsidian_note when the user wants to save or export a note to Obsidian. Use create_x_post when the user wants to compose or share a post on X. Use create_markdown_file when the user wants a clean standalone Markdown document with metadata. Use prepare_transfer when the user or an agent wants to stage a file transfer, create a temporary download link, or view the interactive transfer card. Always present the returned action link as a clickable markdown link and show the file content or summary. Remind the user that Send Out creates client-side drafts only: it does not send messages directly from the server, and human review is required before dispatch. Send Out does not support Instagram direct messages, LinkedIn messaging, or automated email blasting.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
