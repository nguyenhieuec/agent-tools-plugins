---
name: ad-angle
description: "See how competitors position a product before you write ads. Use when the user asks things like \"Compare the discounts and bundles of allbirds.com and gymshark.com.\" or \"Which angles are missing from these ads? Acme: 50% off. Barkly: recommended by vets.\"."
license: MIT
---

# Ad Angle Map

See how competitors position a product before you write ads. Ad Angle Map reads the public product feeds of Shopify stores you name and groups what they offer: price range, discount depth, bundles, subscriptions, and the persuasion angles in the product-page copy. It also groups ads you paste in, checks whether people discuss an angle on Hacker News or Stack Exchange, and picks the ad type for a buyer's awareness stage.

The tools come from the `ad-angle` MCP server, bundled with the `commerce` plugin.
In Claude Code they are named `mcp__plugin_commerce_ad-angle__<tool>`, for example `mcp__plugin_commerce_ad-angle__map_store_offers`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `ad-angle` is connected.

## When to use

- Reads public Shopify product feeds for stores you name, after robots.txt
- Maps price range, discount depth, bundles and subscription signals
- Groups product-page copy by persuasion angle and lists gaps with handles
- Groups ads you paste in by persuasion angle, with optional image descriptions
- Lists unclaimed and thin angles with counts and ids as evidence
- Checks an angle for public discussion on Hacker News and Stack Exchange
- States that an offer map is product pages and prices, not ads
- Picks the ad type and script beats for a buyer awareness stage, given or read from an audience note

## Examples

- "Compare the discounts and bundles of allbirds.com and gymshark.com."
- "Which angles are missing from these ads? Acme: 50% off. Barkly: recommended by vets."
- "Which ad should I run for my Lumen sleep mask to past customers waiting for a sale?"
- "Compare the discounts and bundles of allbirds.com and gymshark.com." calls `map_store_offers`.
- "Which ad should I run for my Lumen sleep mask to past customers on our email list who are waiting for a sale?" calls `pick_ad_type`.
- "Here are three competitor ads, which angles are they missing? Ad 1 (Acme): 50% off your first box, free delivery. Ad 2 (Acme): 50% off your first box, free delivery. Ad 3 (Barkly): Recommended by vets." calls `map_ad_angles`.
- "Is there public discussion about personalised dog food?" calls `validate_angle_gaps`.
- "Check on the pets Stack Exchange whether people ask about grain free dog food or raw dog food." calls `validate_angle_gaps`.

## When not to use

- "Write ten Facebook ads for my dog food brand and upload them to my ad account.": The plugin is research only; creating and launching ads spends money and is outside its scope.
- "Scrape the Facebook Ad Library and tell me which of my competitor's ads made the most sales.": Scraping an ad library breaks that library's terms, and this plugin has no performance data.
- "What do people on Reddit say about my competitor's mattress ads? Scrape the threads.": Reddit is not a source this plugin may use, and it does not scrape.

## Tools

- `map_store_offers`: Map store offers. Use this when the user wants to compare what competitor stores offer, such as "compare the discounts and bundles of allbirds.com and gymshark.com".
- `map_ad_angles`: Map ad angles. Use this when the user wants to know which persuasion angles a set of ads uses and which are missing, such as "which angles are these competitor ads missing?".
- `validate_angle_gaps`: Check gaps in public discussion. Use this when the user wants to check whether people talk about a topic or an angle in public, such as "is there discussion about personalised dog food?".
- `pick_ad_type`: Pick ad type by buyer stage. Use this when the user wants to know which kind of ad to run for a product and an audience, such as "which ad should I run for my sleep mask to past customers?".

## How to answer

Use these tools to research how competitors position a product, before any ad is made. For offer and price angles call map_store_offers with the store addresses the user named (at most three). For creative angles call map_ad_angles with the ads the user pasted in; do not invent ads and do not fetch an ad library. To check whether people discuss an angle in public call validate_angle_gaps with short phrases. To choose which ad to make for an audience call pick_ad_type with the product and the stage, or a short audience note when the stage is not known; if the stage is unclear, show both plays and ask which fits. Tell the user that an offer map is product-page copy and pricing, not ads, and that a store which refuses is unavailable rather than scraped. Gaps are leads from keyword matching, not proof that an angle works; say so, and cite the handles or ad ids and the counts. Never search Reddit. These tools do not create, upload or launch ads and do not touch any ad account.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
