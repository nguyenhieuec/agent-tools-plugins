---
name: postkit
description: "Posts, video scripts, TikTok slide images, and a launch clip from your page. Use when the user asks things like \"Write posts about our new open-source CLI for X, Bluesky and LinkedIn.\" or \"Write a 25-second video script about our new app for TikTok and Instagram Reels.\"."
license: MIT
---

# Post Kit

Turn a topic into posts that already fit each platform, write a 25-second video script for TikTok, YouTube Shorts, Instagram Reels or X video, adapt a post you wrote for another market, and make a short animated launch clip from your own product page.

The tools come from the `postkit` MCP server, bundled with the `content-media` plugin.
In Claude Code they are named `mcp__plugin_content-media_postkit__<tool>`, for example `mcp__plugin_content-media_postkit__draft_posts`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `postkit` is connected.

## When to use

- Writes one post per platform, up to 6 at a time, for X, Bluesky, Threads, LinkedIn and more
- Checks each draft against the platform's length, hashtag, title and link rules; rewrites one that breaks a rule
- Counts length the way the platform does: X weighted characters, Bluesky graphemes, Threads emoji bytes
- Returns a checklist with image sizes and the source of every rule, dated
- Adapts a post you wrote for another language and culture, not a literal translation
- Lists what changed for the new market and can check the result against a platform's limits
- Writes a 25-second video script (hook 0-3 s, body 4-20 s, call to action by 25 s) with a caption per platform
- Returns official video length limits for TikTok, YouTube Shorts, Instagram Reels and X video, each with its source
- Flags drafts that declare an AI-generated person or voice and names the label that applies, with the platform policy
- Builds a self-contained animated HTML launch clip from your own product page; honours robots.txt
- Never publishes, reads accounts, fetches post statistics or trends, or stores your text
- Makes 3 or 4 TikTok slideshows of 5 slides, each a 1080x1920 PNG, plus caption and hashtags
- Draws your own slide lines for a product account and plans its first 7 to 14 days of posts

## Examples

- "Write posts about our new open-source CLI for X, Bluesky and LinkedIn."
- "Write a 25-second video script about our new app for TikTok and Instagram Reels."
- "Make 4 TikTok slideshows to test this app idea before I code it."
- "Write posts for X, Bluesky and LinkedIn about the launch of our new open-source CLI, branch-tidy. It deletes merged git branches in one command, works on macOS, Linux and Windows, and is MIT licensed." calls `draft_posts`.
- "Write a 25-second video script for TikTok and Instagram Reels about our new budgeting app, Penny Jar. It sorts card spending into envelopes by itself and warns you 3 days before a bill is due." calls `draft_video_script`.
- "Adapt this post for a Japanese audience: 'Our app is a home run for busy parents.'." calls `localize_post`.
- "Make 4 TikTok slideshows to test this idea before I code: a shared grocery list for roommates." calls `draft_tiktok_slides`.
- "Make a launch video from our product page, https://tools.openkrill.app/app-submit." calls `launch_video`.

## When not to use

- "Post this to my X account right now.": The plugin has no way to publish and never touches a user's account.
- "Take this creator's viral video, clone her voice for my new account and repost it as mine.": Post Kit works only on the user's own words and does not reuse other people's content, clone voices or write for persona accounts.
- "How many likes did my last post get, and what is trending on X today?": The plugin fetches neither account statistics nor trend data.

## Tools

- `draft_posts`: Draft posts for each platform. Use this when the user wants social posts written about a topic, such as "write posts about our new CLI for X, Bluesky and LinkedIn".
- `draft_video_script`: Write a short-form video script. Use this when the user wants a script for a short vertical video, such as "write a 25-second TikTok and Reels script about our new app".
- `localize_post`: Adapt a post for another market. Use this when the user wants a post they wrote adapted for readers in another country or language, such as "adapt this for a Japanese audience".
- `launch_video`: Make a launch video clip. Use this when the user wants a short launch video for their own product page, such as "make a launch video from https://example.com/".
- `draft_tiktok_slides`: Make TikTok slideshows for one idea. Use this when the user wants to test an app idea with TikTok slideshows before coding, such as "make 4 slideshows for this idea".

## How to answer

The user publishes every post themselves. Call draft_posts for posts on a topic, localize_post to adapt a post for another market, draft_video_script for a short video script, launch_video for an HTML clip of the user's own product page, and draft_tiktok_slides for TikTok slideshows of one idea. For draft_tiktok_slides, quote the summary field first. For a product or a new account, write 3 or 4 angles of 5 short lines from the user's facts as angle_lines, with plan_days 14 when the user wants a posting plan. Save png_base64 only when the user asks for the files; with angle_lines each slide is a png_url link instead. Show posting_plan days as a table with its rules, and add no posting times or claims. Show each angle's caption and hashtags. The page /slides does the same job. Show each draft with its length and issues. passes false means it breaks a rule named in issues: say so, do not present it as ready. When ai_disclosure.flagged is true, name the label each platform needs and quote its source. Do not call it optional where the rule requires it, and do not decide for the user whether to post. Use only the facts in the user's topic or post: add no claims, numbers or links. capacity_reached means the free daily writing allowance is used up until 00:00 UTC: say so and offer the checklist, which is still returned. Do not use these tools to post for the user, read an account, fetch statistics or trends, or repost or translate someone else's content. Decline those and persona farms, voice cloning of real people or fake-identity accounts. launch_video is for the user's own page: ask before calling it on a page that is not theirs. Offer the returned html as a file to open in a browser.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
