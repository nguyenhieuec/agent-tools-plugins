---
name: site-check
description: "Check one page for an SEO audit, Open Graph tags, a 301 redirect and broken links. Use when the user asks things like \"Which AI crawlers can read https://example.com/blog?\" or \"Will AI assistants cite https://example.com/blog/guide?\"."
license: MIT
---

# Site SEO & AI Crawler Audit

Check one page for an SEO audit, Open Graph tags, a 301 redirect and broken links. Ask "can AI crawlers read example.com?", "is this product page set up for search?", "will AI assistants cite this page?", "can AI shopping agents read this store?", "are any pages in this store's sitemap broken?" or "what would make this landing page convert better?".

The tools come from the `site-check` MCP server at `https://sitecheck.openkrill.app/mcp`, bundled with the `web-email` plugin.
Call them as MCP tools, never from a shell. In Claude Code they are named `mcp__plugin_web-email_site-check__<tool>`, for example `mcp__plugin_web-email_site-check__check_ai_crawler_access`; if one is not loaded yet, find it with ToolSearch first.
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

- "Which AI crawlers can read https://example.com/blog?"
- "Will AI assistants cite https://example.com/blog/guide?"
- "Review the landing page https://example.com/ for conversion."
- "Is https://www.allbirds.com/products/mens-tree-runners set up for search? Check the title, description and product data." calls `product_page_seo`.
- "Review my landing page https://example.com/ for conversion. What should I fix first?" calls `landing_page_check`.
- "Will AI assistants cite this page, and what should I fix? Check https://www.allbirds.com/pages/our-materials-tree" calls `ai_readiness_check`.
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
- `ai_readiness_check`: Check AI readiness of a store or page. Checks if AI agents can read and cite a site or page.
- `link_sample_check`: Check a sitemap sample for broken links. Finds broken or redirected pages in a sitemap sample.
- `landing_page_check`: Check a landing page for conversion. Checks a landing page for conversion problems.
- `exposure_check`: Check the exposed surface of a site you own. Checks what a site or app you own leaks publicly.

## How to answer

Use check_ai_crawler_access when the user asks whether AI crawlers or a named AI crawler can read a site, check_page_tags for the title, description, canonical and share-preview tags of a page, and trace_redirects for where an address redirects to and its response headers. For online stores and content pages, use product_page_seo for one product page's search tags and Product structured data, ai_readiness_check for whether a store domain publishes llms.txt, agents.md, a UCP file, a sitemap and AI crawler rules, plus five page citation signals (clear summary near the top, author credentials, Q&A format, heading structure, structured data) when a page address is checked, and link_sample_check for broken pages and redirects in a sample of up to 40 sitemap addresses. For a landing page, use landing_page_check for conversion items on one page (headline, call to action, forms, trust signals, pricing or next step, viewport, contact path), each with evidence and a fix. It does not measure speed, rankings or traffic, and it is not a design review. For the exposed surface of a domain or host the user owns, use exposure_check. It refuses a bare IP address; ask the user for a host name that points at the server. The first call usually answers ownership_not_proven with a DNS TXT record and a file to publish; tell the user exactly what to publish, and call again once it is live. Pass the app's page address when the user names one, so that page's scripts are read for keys. Report the answer first (exposed or clean, top risks, one fix each), then the evidence, and say that open ports are not checked. Findings come in three labels: only confirmed ones have a severity; for each one that needs verification, say the fact that is not known and how the owner checks it; mention a rejected one only to explain a 200 answer or a public key the user asks about. The six legal signals are the sign-up age question (COPPA), Google Fonts from Google's servers, session replay scripts, renewal terms next to a subscribe button, the DMCA agent and, for one pasted marketing email, its unsubscribe link and postal address (CAN-SPAM); they need the same ownership proof. For the legal signals, give each one that needs verification with its law, finding, the fact that is not known, fix and source link, list the not checked ones with their reason (not checked never means not applicable), and say once that it is not legal advice; when the user asks about marketing email, pass one email they send as email. Never repeat a key beyond the masked prefix the tool gives. Never use it on a target the user does not own, and never to look up a person, user name or email address. Pass a public page address or domain; a bare domain means https. Do not try addresses that are private, local or internal, and tell the user those are not checked rather than working around it. Report what the tool returned and say it is what the site published to Agent Tools servers at that moment. For crawler access, name each blocked crawler with the rule shown, say that robots.txt is a request and not access control, and say it does not show whether a firewall blocks crawlers. If robots.txt status is unreachable, say there is no verdict. If a store check says robots.txt disallows a page for Agent Tools, report that and do not try to get around it. A link sample is a sample, not every page of the site. For the landing check, report each finding with its evidence and fix, say that content added by JavaScript is not seen, and treat the result as a checklist, not a prediction of conversion rate. Apart from exposure_check on a target the user proved they own, do not use these tools to scan for vulnerabilities, to collect contact details, prices or content from a site, to monitor a site over time, or on sites the user has no reason to look at. Tags and data added by JavaScript are not seen.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
