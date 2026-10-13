---
name: certpacket
description: "CertPacket helps a small importer get the US safety certificate for a consumer product ready before the goods ship. Use when the user asks things like \"Which safety rules and certificate does a painted wooden toy for 3 year olds need?\" or \"Find CPSC-accepted labs in Vietnam for a baby bib.\"."
license: MIT
---

# CertPacket

CertPacket helps a small importer get the US safety certificate for a consumer product ready before the goods ship.

The tools come from the `certpacket` MCP server, bundled with the `commerce` plugin.
In Claude Code they are named `mcp__plugin_commerce_certpacket__<tool>`, for example `mcp__plugin_commerce_certpacket__find_cert_rules`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `certpacket` is connected.

## When to use

- Names the CPSC rules and the certificate type (CPC or GCC) for a product
- Shortlists CPSC-accepted labs for the exact tests, Vietnam and China first
- Scores a lab report for missing rules, failed results, lab acceptance and date gaps
- Drafts the CPSC eFiling certificate fields from a report
- Finds and watches recalls of similar products in the US, Canada and the EU

## Examples

- "Which safety rules and certificate does a painted wooden toy for 3 year olds need?"
- "Find CPSC-accepted labs in Vietnam for a baby bib."
- "Check this lab report before I file the certificate."
- "Which safety rules and certificate does a painted wooden toy for 3 year olds need?" calls `find_cert_rules`.
- "Find CPSC-accepted labs in Vietnam that can test lead in paint and lead content." calls `find_accepted_labs`.
- "Check this lab report for a baby bib before I file." calls `check_lab_report`.
- "Have similar plush toys been recalled in the US, Canada or the EU?" calls `match_recalls`.
- "Watch baby walkers and plush toys for new recalls." calls `watch_recalls`.

## When not to use

- "Is my certificate correct? Just confirm it so I can file.": The tools give decision support only and never claim a filing is correct.
- "What EU CE marking rules apply to my toy?": The rules seed holds US CPSC rules only.
- "Book a test at the cheapest lab and pay for it.": The tools read public lists; they have no booking or payment.

## Tools

- `find_cert_rules`: Find the safety rules and certificate. Find which US product-safety rules a consumer product must meet and which certificate the importer files: CPC (Children's Product Certificate, for products made for children 12 and under) or GCC (General Certificate of Conformity).
- `find_accepted_labs`: Find CPSC-accepted labs. Shortlist testing labs that CPSC currently accepts for every test a product needs, in one country (default Vietnam and China).
- `check_lab_report`: Check a lab report. Check a children's product lab test report before the importer certifies.
- `match_recalls`: Find recalls of similar products. Find recalls of products like this one in the US (CPSC), Canada (Health Canada) and the EU (Safety Gate), usually from the last 2 years.
- `watch_recalls`: Watch for new recalls. Start a watch list of 1 to 10 product types and hear about new recalls of similar products in the US, Canada and the EU.
- `stop_watching_recalls`: Stop a recall watch. Delete a recall watch list and everything it has seen.

## How to answer

Start with find_cert_rules for the product; if the user sent a photo, describe the product, its age and features in words. Pass its testedRuleIds to find_accepted_labs, with f963Sections and metalParts when they apply. For check_lab_report, pass the uploaded PDF as file, or the pasted text as text, plus product or ruleIds. List blockers first, then fixes, then checks, and say each fix in plain words. Use match_recalls to show hazards found in similar products; say a recall of a similar product is not about the user's product. watch_recalls returns a watchKey once: tell the user to save it. If status is not_covered, say the product type is outside the seeded list and give the guidance link. Never say a product is compliant or a certificate is correct. End every answer with the notice field text.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
