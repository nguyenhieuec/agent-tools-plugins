---
name: catalog-qa
description: "Check an online store's product catalog before you import it or send it to a shopping feed. Use when the user asks things like \"Will this Shopify product CSV import cleanly?\" or \"What would Google Merchant Center reject in my catalog?\"."
license: MIT
---

# Catalog QA Desk

Check an online store's product catalog before you import it or send it to a shopping feed.

The tools come from the `catalog-qa` MCP server, bundled with the `commerce` plugin.
Call them as MCP tools. Do not call the server with curl or any other shell command: the MCP tool is the supported path. In Claude Code they are named `mcp__plugin_commerce_catalog-qa__<tool>`, for example `mcp__plugin_commerce_catalog-qa__csv_import_preflight`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `catalog-qa` is connected.

## When to use

- Checks a Shopify product CSV before import and lists blocking rows and warnings, each with the fix
- Finds products Merchant Center would reject: bad or missing GTINs, long or promotional titles, empty descriptions
- Finds images without alt text, copied or thin descriptions and title problems
- Builds a CSV of corrected titles and alt text to review and import; changes nothing in the store
- Reads a store's public product feed once on request, after robots.txt, instead of a CSV

## Examples

- "Will this Shopify product CSV import cleanly?"
- "What would Google Merchant Center reject in my catalog?"
- "Which of my products have no image alt text?"
- "I edited my Shopify product CSV by hand. Will it import cleanly? Here is the file." calls `csv_import_preflight`.
- "What would Google Merchant Center reject in this product export?" calls `merchant_feed_check`.
- "Check the catalog of allbirds.com for feed problems. It is a Shopify store." calls `merchant_feed_check`.
- "Which of my products have no image alt text, and are any descriptions copied from another product? Here is my product CSV." calls `listing_text_audit`.
- "Write me a sheet that fills in the missing alt text in this product CSV using the product title." calls `listing_fix_sheet`.

## When not to use

- "Update the titles in my live Shopify store directly.": The plugin reads and reports; it has no access to any store and writes nothing.
- "Submit my feed to Google Merchant Center and approve the products.": The plugin checks products; it does not connect to or submit anything to Google.
- "Pull the whole catalog of every store that sells running shoes and index it for me.": Domain mode is a single user-requested read of one store's public feed, not a crawler or a cross-store index.

## Tools

- `csv_import_preflight`: Check a product CSV before import. Check the text of a Shopify product CSV against the import rules before it is imported.
- `merchant_feed_check`: Check products for Merchant Center rules. Check products against Google Merchant Center feed rules: missing or invalid GTIN barcodes, long, promotional or all-capital titles, missing descriptions, links, images and brand.
- `listing_text_audit`: Audit product listing text. Audit the text of product listings: images without alt text, descriptions copied between products, descriptions that are too short and title problems.
- `listing_fix_sheet`: Build a CSV of listing text fixes. Build a CSV of corrected titles and image alt text from a catalog, for the user to review and import.

## How to answer

Use csv_import_preflight with the text of a Shopify product CSV before an import: report blocking items first, then warnings, naming row and fix. Use merchant_feed_check and listing_text_audit with either csv (the file text) or domain (a store address), never both; domain reads the store's public product feed once and a large store may refuse it, in which case ask for the CSV. Use listing_fix_sheet to produce a CSV of corrections; it is advisory text for the user to review and import themselves, nothing is changed in the store. Counts in totals are before lists were cut; say so when truncated is true. Do not call a GTIN valid beyond its check digit.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
