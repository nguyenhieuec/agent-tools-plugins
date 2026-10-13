---
name: seo-audit
description: "Find what to fix on a public website, for search engines and for AI answers. Use when the user asks things like \"Audit example.com for SEO problems\" or \"Is example.com ready to be named in AI search?\"."
license: MIT
---

# SEO Audit

Find what to fix on a public website, for search engines and for AI answers.

The tools come from the `seo-audit` MCP server, bundled with the `web-email` plugin.
In Claude Code they are named `mcp__plugin_web-email_seo-audit__<tool>`, for example `mcp__plugin_web-email_seo-audit__audit_site`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `seo-audit` is connected.

## When to use

- Audits up to 25 pages of a site from its homepage and sitemap, with robots.txt respected
- Returns failed rules grouped by priority with the affected pages, the fix and the documentation link
- Checks titles, descriptions, headings, canonicals, noindex, structured data, share tags and image alt text
- Finds broken links, redirect chains, sitemap problems, thin and duplicate content
- Checks local business markup: name, address, phone, hours, map, and that the details match the page
- Compares pages: near-identical templated pages, little own text, hreflang returns, sitemap pages nothing links to
- Adds Bing AI-search steps to a site audit: AI Performance, IndexNow, content, with sources
- Audits one page with the full page rule set
- Optional judgement of page purpose and competing pages, limited per day
- Checks local AI-search readiness: services, locations, prices, FAQ, schema, NAP, proof
- Checks llms.txt, llms-full.txt, an AI page, home links, and name, product and fact consistency
- Drafts an llms.txt from the site's own public pages for the owner to review
- Scores one page from 0 to 100 for citation chance, with fixes by biggest gain
- Compares pages an AI answer cited with yours: opening answer, headings, table, list, date, links out, schema
- Ranks 7+ word searches in a pasted Search Console export, with page, question heading and answer skeleton

## Examples

- "Audit example.com for SEO problems"
- "Is example.com ready to be named in AI search?"
- "Will ChatGPT cite https://example.com/pricing? What should I fix?"
- "Audit example.com for SEO problems." calls `audit_site`.
- "Is example.com ready to be named in AI search? It is a local plumbing business." calls `local_ai_readiness`.
- "Does example.com publish llms.txt, llms-full.txt and an AI instructions page, and does the home page link to them?" calls `check_ai_instructions`.
- "Google's AI Overview for 'best form builder' cites https://a.example.com/best and https://b.example.com/guide. How does https://mine.example.com/forms compare?" calls `compare_citations`.
- "Will this page be cited, and how do I fix it? Here is the HTML." calls `score_page_citation`.

## When not to use

- "Look up what Google's AI Overview cites for 'best form builder' and compare it with my page.": compare_citations reads only the pages the caller names. It never queries Google or an AI engine, so it cannot find the cited pages itself.
- "Audit http://localhost:3000, draft an llms.txt for it, and check http://192.168.1.10.": Local and private addresses are refused by design.
- "Publish the drafted llms.txt to example.com and guarantee it will change our branded AI Overviews.": generate_ai_instructions returns text only. The plugin does not write to the site and does not query AI engines.

## Tools

- `audit_site`: SEO audit (site). Use this to audit a whole site or several pages for SEO; it finds the pages itself.
- `score_page_citation`: Citation score (page). Cite-chance score for one page, 0 to 100.
- `find_ai_questions`: Questions for AI search. Long-search finder for a pasted Search Console export.
- `compare_citations`: Compare cited pages. Citation-pattern check for one query.
- `audit_page`: SEO audit (page). Use this only for a single page address the user gave.
- `local_ai_readiness`: Local AI-search readiness. Local-business readiness for AI search answers.
- `check_ai_instructions`: Check AI instructions. Checks the site's AI instructions files and page.
- `generate_ai_instructions`: Draft an llms.txt. Drafts an llms.txt from the site's own public pages.

## How to answer

Pass a public address; a bare domain means https. Private, local or internal addresses are not checked: say so and do not work around it. Lead with the summary or score, then findings or fixes in the order returned, high priority first, with pages, evidence URL and fix. Cite the source links of rules you mention. For audits, say how many pages were read and that counts describe that sample. Say which rules are heuristics. Relay the notes, including a robots.txt refusal. After audit_site findings, list the bing_ai_search steps with their source links. Leave judge off unless asked what pages are for or which compete; if it is skipped, say why. find_ai_questions: the answer skeleton needs one real number or fact from the owner. Never invent it or offer an example; ask the owner. If summary.hasPages is false, relay the note. Tell the owner to pick one question about what they sell and ask it by hand in ChatGPT or Google AI first. compare_citations: ask the user for 2 to 6 cited addresses; never guess them or claim you read the AI answer. Lead with gaps, then name each unread cited page in the words of cited[].note. If readableCited is under 2, say no pattern can be reported and ask for more addresses. A score, a pass or a shared signal is not a promise of a citation, traffic or a change in AI Overviews. Cite reason.post and reason.asOf, or study.source and study.asOf, when you mention the method. Say profile advice was not fetched. Say a generate_ai_instructions draft was not published. If a daily limit refuses a call, say when it resets. Do not use these tools to collect content or contact details, or for rankings, traffic or backlinks. Do not query AI engines or scrape directories.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
