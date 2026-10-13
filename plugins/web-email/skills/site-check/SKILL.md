---
name: site-check
description: "Check public pages for SEO tags, AI crawler rules, AI citation signals and landing page conversion. Use when the user asks things like \"Will AI assistants cite https://example.com/blog/guide?\" or \"Is https://example.com/products/blue-mug set up for search?\"."
license: MIT
---

# Site SEO & AI Crawler Audit

Site SEO and AI Crawler Audit checks public web pages and reports what they publish right now: SEO tags, redirects, AI crawler rules, AI citation signals, landing page conversion and, for a site you own, what leaks publicly. People also call it Site Check.

The tools come from the `site-check` MCP server, bundled with the `web-email` plugin.
In Claude Code they are named `mcp__plugin_web-email_site-check__<tool>`, for example `mcp__plugin_web-email_site-check__check_ai_crawler_access`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `site-check` is connected.

## When to use

- Shows whether robots.txt allows or blocks 14 published AI crawlers
- Shows the rule that decided each crawler, the sitemaps and any noindex or noai directives
- Reads a page's title, description, canonical link, language and icons
- Reads Open Graph and Twitter share tags and lists the common ones that are missing
- Follows the redirect chain with each status code and shows the final address and headers
- Checks one product page's title, description, h1, canonical link and Product structured data (price, GTIN, availability)
- Checks a domain for llms.txt, agents.md, the Shopify UCP file, a sitemap and AI crawler rules in robots.txt
- Checks a page for five AI citation signals: summary, author credentials, Q&A format, headings, structured data
- Checks up to 40 sitemap addresses for broken pages and redirect chains, skipping pages robots.txt disallows
- Checks a landing page's h1, call to action, form fields, trust signals, pricing, viewport and contact path
- Tests up to five call-to-action links of a page and gives each finding with evidence and a fix
- Checks the exposed surface of a domain its owner proves: leaked files, dangling subdomains, default pages
- Finds API keys in the JavaScript an owned app ships, public source maps and open admin paths, keys masked
- Flags 6 legal signals on an owned app (COPPA, fonts, session replay, CAN-SPAM, renewals, DMCA) with sources
- Refuses private, local and internal addresses and redirects into them

## Examples

- "Will AI assistants cite https://example.com/blog/guide?"
- "Is https://example.com/products/blue-mug set up for search?"
- "I own example.com: is anything exposed?"
- "Is https://www.allbirds.com/products/mens-tree-runners set up for search? Check the title, description and product data." calls `product_page_seo`.
- "Review my landing page https://example.com/ for conversion. What should I fix first?" calls `landing_page_check`.
- "Will AI assistants cite this page, and what should I fix? Check https://www.allbirds.com/pages/our-materials-tree." calls `ai_readiness_check`.
- "Are any pages of allbirds.com broken? Check the sitemap." calls `link_sample_check`.
- "I own openkrill.app. Is anything on it exposed to the internet?" calls `exposure_check`.

## When not to use

- "List every product and its price from https://competitor.example.com and track the prices every day.": The plugin checks one page or one domain per call and keeps nothing between calls.
- "Log in to my store admin at http://localhost:9292/admin and tell me which orders and customers need attention.": Private and local addresses are refused, and the plugin has no access to store admin data or personal data.
- "Find every account that uses the username jdoe88 and the email jdoe@example.com across social sites.": The exposure check reads certificate, DNS and host records of a proven-owned target only; there is no person or account search.

## Tools

- `check_ai_crawler_access`: Check AI crawler access in robots.txt. Checks whether 14 AI crawlers may fetch a page.
- `check_page_tags`: Check Open Graph and share tags. Reads title, description and share tags of a page.
- `trace_redirects`: Trace a 301 redirect chain. Traces the redirects and headers of a web address.
- `product_page_seo`: Check product JSON-LD and page SEO. Checks the SEO and Product data of a product page.
- `ai_readiness_check`: Check AI readiness of a store or page. Checks if AI agents can read and cite a store or page.
- `link_sample_check`: Check a sitemap sample for broken links. Finds broken or redirected pages in a sitemap sample.
- `landing_page_check`: Check a landing page for conversion. Checks a landing page for conversion problems.
- `exposure_check`: Check the exposed surface of a site you own. Checks what a site or app the user owns leaks publicly.

## How to answer

Pass a public page address or domain, such as https://example.com/pricing; a bare domain means https. Private, local or internal addresses are refused: say so, ask for a public address and do not work around it. Report results as what the site published at that moment. robots.txt is a request, not access control; if it is unreachable there is no verdict. If robots.txt disallows a page for Agent Tools, tell the user and do not retry another way. If the site did not answer, give the failure and suggest checking the address or trying later. A link sample is not every page. Content added by JavaScript is not seen. The landing check is a checklist, not a forecast. exposure_check refuses a bare IP: ask for a host name. The first call usually returns ownership_not_proven with a DNS TXT record and a file: tell the user exactly what to publish, then call exposure_check again with the same target once it is live. Lead with exposed or clean and the top risks with one fix each, then the evidence, and say open ports are not checked. Only confirmed findings have a severity. For needs verification, give the unknown fact and how the owner checks it. For legal signals, give law, finding, fix and source, list not checked ones with their reason, and say once it is not legal advice. Pass one marketing email as email when the user asks about email. Never repeat a key beyond the masked prefix. Never use exposure_check on a target the user does not own, or to look up a person, user name or email. Do not use these tools to scan for vulnerabilities, collect contacts, prices or content, or monitor a site.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
