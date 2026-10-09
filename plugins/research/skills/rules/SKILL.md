---
name: rules
description: "Read a CFR or eCFR section, or search US federal regulations and the Federal Register. Use when the user asks things like \"What does 21 CFR 11.10 say about electronic records controls?\" or \"Which federal regulations mention respirator fit testing?\"."
license: MIT
---

# US Federal Regulations (CFR)

Read a CFR or eCFR section, or search US federal regulations and the Federal Register. Ask "what does 21 CFR 11.10 say?", "which regulations mention respirator fit testing?" or "what has the FDA proposed on food dyes this year?".

The tools come from the `rules` MCP server, bundled with the `research` plugin.
In Claude Code they are named `mcp__plugin_research_rules__<tool>`, for example `mcp__plugin_research_rules__get_cfr_section`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `rules` is connected.

## When to use

- Reads the current text of any section of the US Code of Federal Regulations (eCFR)
- Shows a section's heading, statutory authority and amendment history with a link
- Searches eCFR text and federal compliance rules by words, in one title or all of them
- Searches the Federal Register for rules, proposed rules, notices, rulemakings and executive orders
- Shows comment deadlines, effective dates and the CFR parts a document changes

## Examples

- "What does 21 CFR 11.10 say about electronic records controls?"
- "Which federal regulations mention respirator fit testing?"
- "Find recent proposed rules about noncompete agreements"
- "What does 21 CFR 11.10 say about controls for closed systems?" calls `get_cfr_section`.
- "Read me 17 CFR 240.10b-5, the securities anti-fraud rule." calls `get_cfr_section`.
- "Which federal regulations in title 29 talk about respirator fit testing?" calls `search_cfr`.
- "What has the FDA proposed recently about food dyes?" calls `search_federal_register`.
- "Find the final rules about noncompete agreements published in 2024." calls `search_federal_register`.

## When not to use

- "Am I allowed to fire an employee for taking sick leave in California?": The plugin reads US federal regulations only and gives general information, not advice on a situation.
- "What did the Supreme Court decide in Chevron v. NRDC?": Court decisions are not in the CFR or the Federal Register.
- "What is the GDPR rule on data retention?": Foreign law is out of scope.

## Tools

- `get_cfr_section`: Read an eCFR section (FDA, OSHA, EPA). Use this when the user asks what a specific US federal regulation says, including an FDA, OSHA, EPA, SEC, FTC or FAA section of the eCFR, for example "what does 21 CFR 11.10 say?", "read 29 CFR 1910.134" or "show me 17 CFR 240.10b-5".
- `search_cfr`: Search federal regulations and compliance. Use this when the user asks which US federal regulations or compliance rules cover a topic, including an OSHA, EPA or FDA rule, for example "which regulations mention respirator fit testing?", "find the rules on electronic signatures in title 21" or "where does the CFR define small business?".
- `search_federal_register`: Search the Federal Register and rulemakings. Use this when the user asks what federal agencies have published, proposed or announced, including a rulemaking, for example "what has the FDA proposed on food dyes?", "find final rules on noncompete agreements" or "which executive orders mention AI this year?".

## How to answer

Use get_cfr_section when the user names a regulation (for example 21 CFR 11.10) or you know its title and section, search_cfr to find which sections of the current regulations cover a topic, and search_federal_register for recent rules, proposals, notices and executive orders. Quote the text as returned and give the citation and the asOf date. If truncated is true, say the section was cut and point to the url. Always say this is general information and not legal advice, that the eCFR is an unofficial edition, and that the user should check the current official text and ask a lawyer before relying on it. Do not tell the user what they must do or whether they comply; explain what the text says. Only United States federal regulations are covered: for state, local, foreign or case law, say it is not covered. If a section is not found, check the title and the section number with the user instead of guessing a different one.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
