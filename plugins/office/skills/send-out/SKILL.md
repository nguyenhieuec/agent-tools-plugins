---
name: send-out
description: "Turn assistant output into ready-to-send action links and files, with no sign-in. Use when the user asks things like \"Draft an email to the project team about status updates\" or \"Make a calendar event for Sprint Review next Monday at 10am New York time\"."
license: MIT
---

# Send Out

Turn assistant output into ready-to-send drafts with no sign-in. You open each draft in your own app, check it and press send yourself.

The tools come from the `send-out` MCP server, bundled with the `office` plugin.
In Claude Code they are named `mcp__plugin_office_send-out__<tool>`, for example `mcp__plugin_office_send-out__draft_email`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `send-out` is connected.

## When to use

- Creates mailto draft links and RFC 822 .eml files ready to open in email clients
- Creates Google Calendar links and RFC 5545 .ics calendar files with reminders
- Creates WhatsApp wa.me click-to-chat links with normalized phone numbers
- Creates Obsidian obsidian://new links that open Obsidian with the note filled in
- Creates X post share links checked against character limits
- Generates Markdown files with structured YAML frontmatter
- Stages one file behind a temporary download link that expires in 1 to 168 hours
- Never sends a message from the server: you review and send every draft

## Examples

- "Draft an email to the project team about status updates"
- "Make a calendar event for Sprint Review next Monday at 10am New York time"
- "Save these meeting notes as a note in my Obsidian vault"
- "Draft an email to alex@example.com with subject 'Project Milestone' saying 'Phase 1 is complete and ready for review.'." calls `draft_email`.
- "Create a calendar event for 'Product Launch' on 2026-11-15T14:00 with duration 60 minutes in Europe/London." calls `create_calendar_event`.
- "Create a WhatsApp link for phone '+1 (555) 234-5678' with message 'Your appointment is confirmed for Tuesday at 3pm.'." calls `create_whatsapp_link`.
- "Export a research note titled 'Edge Compute Patterns' to my 'TechNotes' Obsidian vault with tags ['architecture', 'edge']." calls `create_obsidian_note`.
- "Create an X post: 'Excited to ship our new open source CLI today! Check it out at https://example.com' with hashtag #devtools." calls `create_x_post`.

## When not to use

- "Send an automated cold direct message on Instagram to 50 prospective clients.": Meta Graph API strictly limits direct messaging and OpenAI policies prohibit unsolicited automated outreach.
- "Send a private LinkedIn message to this recruiter asking about open positions.": LinkedIn User Agreement Section 8.2 explicitly bans automated communication plugins and bot messaging.
- "Blast a marketing email newsletter to a mailing list of 5,000 addresses.": Send Out is a stateless personal dispatch bridge with zero server-side relay, designed to prevent spam and enforce human-in-the-loop review.

## Tools

- `draft_email`: Draft email. Drafts one email the user sends themselves.
- `create_calendar_event`: Create calendar event. Builds one calendar event the user adds themselves.
- `create_whatsapp_link`: Create WhatsApp link. Makes a WhatsApp click-to-chat (wa.me) link.
- `create_obsidian_note`: Create Obsidian note. Makes an Obsidian note link.
- `create_x_post`: Create X post. Makes an X (Twitter) share link.
- `create_markdown_file`: Create Markdown document. Makes one standalone Markdown (.md) file with YAML front matter.
- `prepare_transfer`: Prepare file transfer. Stores one text file behind a temporary download link and shows a transfer card.

## How to answer

Use Send Out when the user wants to send or export generated content. Each tool returns a link or file text the user opens and sends themselves. Use draft_email for an email draft or .eml file. Use create_calendar_event for a meeting, invite or .ics file. Use create_whatsapp_link for a WhatsApp message. Use create_obsidian_note to save a note into Obsidian. Use create_x_post to compose a post on X. Use create_markdown_file for a plain .md file when the user does not use Obsidian. Use prepare_transfer only when the user wants a hosted download link: send file_name and content to stage a file, or only transfer_id to inspect one. The other tools already return the file text, so do not call prepare_transfer just to show a file. Always show the returned link as a clickable markdown link and show the file content or a summary. Tell the user that nothing is sent by the server and that they must review the draft before they send it. Send Out does not support Instagram messages, LinkedIn messages, SMS, or bulk or automated email. For those, say so plainly and offer an email draft or a copyable text instead.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
