---
name: mailcheck
description: "Check a domain's email DNS, whether Gmail and Outlook accept its mail, and its registration. Use when the user asks things like \"Is DMARC set up correctly for github.com?\" or \"Will Gmail and Outlook accept mail from example.com sent from 203.0.113.7?\"."
license: MIT
---

# Domain Mail Check

Check how a domain is set up for email, whether Gmail and Outlook are likely to accept its mail, and who it is registered with, from public DNS, free blocklists and registry data.

The tools come from the `mailcheck` MCP server, bundled with the `web-email` plugin.
Call them as MCP tools. Do not call the server with curl or any other shell command: the MCP tool is the supported path. In Claude Code they are named `mcp__plugin_web-email_mailcheck__<tool>`, for example `mcp__plugin_web-email_mailcheck__launch_score`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `mailcheck` is connected.

## When to use

- Reads a domain's SPF record, counts its DNS lookups and flags weak or broken policies
- Reads the DMARC policy, percentage and report address
- Checks whether DKIM keys are published for up to 5 selectors you name
- Grades email spoofing protection from A to F with a short list of fixes
- Reads A, AAAA, MX, TXT, CAA and NS records with TTLs and names the mail provider from the MX records
- Shows registrar, creation and expiry dates, status flags and name servers from the registry over RDAP
- Never returns registrant or contact details; names its sources and dates
- Gives one verdict on whether Gmail and Outlook will likely accept the domain's mail, the top 3 causes and one fix each
- Checks SPF for the sending IP, reverse DNS, DKIM key length and DMARC against the Gmail, Yahoo and Outlook rules
- Reads 6 free blocklists for the domain and the sending IP, with each list's terms link
- Scores a cold-email sending domain 0 to 100 from SPF, DKIM, DMARC, domain age and name, with fixes
- Labels each rule standard or opinion and links its source, so advice is not shown as a requirement

## Examples

- "Is DMARC set up correctly for github.com?"
- "Will Gmail and Outlook accept mail from example.com sent from 203.0.113.7?"
- "Is acme-mail.com ready for cold email outreach?"
- "Is acme-mail.com ready for cold email outreach?" calls `launch_score`.
- "Will Gmail and Outlook accept mail from example.com sent from 203.0.113.7?" calls `delivery_check`.
- "Is DMARC set up correctly for github.com?" calls `check_email_auth`.
- "Which mail provider does acme.io use?" calls `check_dns_records`.
- "When does openai.com expire and who is the registrar?" calls `lookup_domain_registration`.

## When not to use

- "Is the name zephyrlabs available as a .com?": Checking whether a startup name is available belongs to Startup Name Check; this plugin inspects a domain that already exists.
- "Send a test email to my Gmail and tell me if it lands in spam.": delivery_check never sends mail or reads mailboxes, so it cannot show inbox placement.
- "Who owns this domain? Give me their phone number.": The plugin never reads or returns registrant or contact data.

## Tools

- `launch_score`: Score a cold-email domain. Use this when the user asks whether a new or secondary domain is ready for cold email or outreach, or wants a score for a sending domain, such as "is acme-mail.com ready for cold email?".
- `delivery_check`: Check mail delivery. Use this when the user asks whether mail from a domain will reach Gmail or Outlook, or why it is rejected or goes to spam, such as "will Gmail accept mail from example.com sent from 203.0.113.7?".
- `check_email_auth`: Check email authentication. Use this when the user asks whether a domain's email is set up correctly, such as "is DMARC set up for github.com?" or "check SPF and DKIM for example.com".
- `check_dns_records`: Look up DNS records. Use this when the user asks for a domain's DNS records, its mail provider or which certificate authorities may issue for it, such as "which mail provider does acme.io use?" or "show the MX and CAA records for github.com".
- `lookup_domain_registration`: Look up domain registration. Use this when the user asks who a domain is registered with or when it expires, such as "when does openai.com expire and who is the registrar?".

## How to answer

Use these tools to inspect a domain's email DNS and registration from public data. Call delivery_check when the user asks whether mail will reach Gmail or Outlook, or why it is rejected or filed as spam: pass the domain and, when known, the sending IP. Report the verdict, then the top causes with their fixes, then give the source links; say plainly that a likely_accepted verdict means no published rule is broken, not that the inbox is certain, and mention not_checked items only when they matter. Call launch_score when the user asks whether a new or secondary domain is ready for cold email or outreach, or how to score a sending domain: pass the domain. Report the score and verdict, then the fix_first list, and say for each fix whether it is a standard or an opinion; never present an opinion rule as a requirement, and say the score does not promise the inbox. A domain registered under 14 days ago is a fail on an opinion rule: tell the user to wait and warm up, not that the domain is broken. For the sending IP use delivery_check. Call check_email_auth for SPF, DMARC, DKIM and a spoofing-protection grade, check_dns_records for raw A, AAAA, MX, TXT, CAA or NS records, and lookup_domain_registration for registrar, dates and name servers. Pass dkim_selectors only when the user or their mail provider names them; DKIM cannot be found without a selector, so a status of unknown is not a failure. Report the grade with the issues and fixes behind it, and give the source and as_of date. A status of unavailable or an entry in unavailable means a lookup failed: say so and do not guess. A status of not_found means the name does not exist in DNS. A registered false answer means the registry has no record, which usually means the domain is free to register; for checking name availability for a startup use Startup Name Check. Never present registrant or owner details: these tools do not return them, so decline requests for an owner's name, email or phone number and offer the registrar instead. These tools do not list subdomains or send mail; delivery_check tests published sender rules and blocklists, not a real message.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
