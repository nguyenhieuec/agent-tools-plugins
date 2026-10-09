---
name: seo-audit
description: "Audit a website for technical and on-page SEO problems. Use when the user asks things like \"Google's AI answer for 'best form builder' cites these 4 pages. How does my page compare?\" or \"Audit example.com for SEO problems\"."
license: MIT
---

# SEO Audit

Audit a website for technical and on-page SEO problems. Ask "audit example.com", "what SEO problems does https://example.com/pricing have?" or "which pages on my site have missing titles or duplicate descriptions?".

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

- "Google's AI answer for 'best form builder' cites these 4 pages. How does my page compare?"
- "Audit example.com for SEO problems"
- "Here is my Search Console export. Which long questions should I answer on my pages?"
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

- `audit_site`: Audit a site for SEO problems. Use this to audit a whole site or several pages for SEO; it finds the pages itself.
- `score_page_citation`: Score one page for citation chance. Cite-chance score for one page, 0 to 100.
- `find_ai_questions`: Find the questions to answer for AI search. Long-search finder for a pasted Search Console export.
- `compare_citations`: Compare cited pages with yours. Citation-pattern check for one query.
- `audit_page`: Audit one page for SEO problems. Use this only for a single page address the user gave.
- `local_ai_readiness`: Local AI-search readiness. Local-business readiness for AI search answers.
- `check_ai_instructions`: Check AI instructions. Checks the site's AI instructions files and page.
- `generate_ai_instructions`: Draft an llms.txt. Drafts an llms.txt from the site's own public pages.

## How to answer

Use score_page_citation when the user asks whether a page will be cited, how likely a citation is, or how to fix that. Pass exactly one of url, html, text, or sample. Lead with the score, then the fix list in the order returned, and say the score is not a promise of a citation. Use find_ai_questions when the user pastes Search Console Performance rows (or a CSV export) and wants the questions people ask, which page to edit, or how to get named in ChatGPT or Google AI answers. Pass the text as csv, or the rows as rows. Lead with the top questions: rank, query, impressions, the page, and the heading. Say the answer skeleton has a placeholder that the owner must fill with one real number or fact, and never invent that number yourself or offer an example figure: ask the owner for the real one. If summary.hasPages is false, no page is shown: relay the note that says how to find it in Search Console. Tell the owner to pick the one question that matches something they really sell, and to ask it by hand in ChatGPT or Google AI before editing the page (the reminder field). Use compare_citations when the user has the pages an AI answer or AI Overview cited for one query and wants to know what those pages share and what their own page lacks. The user must give the cited addresses: ask for them (2 to 6) and never guess them, and never claim you read the AI answer. Pass query, cited_urls and exactly one of url, html or text for their page. Lead with gaps (each has cited, yours and fix), then name any cited page that was not read from cited[].note, in the words of the note (a robots.txt that could not be read is not always a block). If readableCited is under 2, gaps is empty on purpose: say no pattern can be reported, name each unread page, ask for more addresses, and do not build a gap list from one page. Say a shared signal is a pattern in a small sample, not a proven reason, and that nothing is promised. Cite reason.post and reason.asOf if you mention the method, and give the track line if they ask about change over days. Use audit_site when the user asks for an SEO audit or health check of a website, audit_page for one page, and local_ai_readiness when they ask whether a local business site is ready to be named in AI search. Use check_ai_instructions when they ask whether a site publishes llms.txt, llms-full.txt or an AI instructions page, whether the home page links to them, or whether that file matches the site's name, products and facts. Use generate_ai_instructions when they want a draft llms.txt to review. Pass a public site or page address; a bare domain means https. Do not try addresses that are private, local or internal, and tell the user those are not checked rather than working around it. After the findings of audit_site, give the bing_ai_search steps in one short list with its source links. Say a citation is not traffic and nothing is promised. Report the high priority findings first, then medium and low, with the affected pages and the fix, and cite the source link of the rules you mention. Say how many pages were read (summary.pagesRead) and that the counts describe that sample, not the whole site. Say which rules are heuristics when you rely on them. Relay the notes, including a refusal by robots.txt. For local_ai_readiness, lead with summary, then each check's status, evidence URL and fix. Cite study.source and study.asOf when you mention engine shares or profile advice. Say that profile advice was not fetched, and that a pass is not a promise of being named. If the call is refused for a daily or network limit, say when it resets and do not work around it. Leave judge off unless the user asks what a page is for, how specific its content is, or which pages compete for the same searches. If the judgement comes back skipped, say why from its note and give the rule results. For check_ai_instructions, lead with summary, then each check's status, evidence URL and fix. For generate_ai_instructions, show the draft and say it was not published. Cite reason.post and reason.asOf. Say effect.promised is false. Do not promise a change in AI Overviews, and do not query an AI engine. Do not use these tools to crawl sites the user has no reason to audit, to collect a site's content or contact details, or for rankings, traffic and backlinks, which they do not measure. Do not query Gemini, ChatGPT, Perplexity, Claude or other AI engines for rankings, and do not scrape Google Business Profile, Yelp or other directories.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
