---
name: post-stats
description: "Public views, likes and comments of YouTube, TikTok and X posts, each with its source and read time. Use when the user asks things like \"How many views does https://www.youtube.com/shorts/8bE_lGbtjyY have?\" or \"Compare these 3 posts and tell me which did best.\"."
license: MIT
---

# Post Stats

Post Stats reads the public numbers of social posts: views, likes, comments, shares and saves, and when each post went up. It covers YouTube videos and Shorts, TikTok videos and X posts.

The tools come from the `post-stats` MCP server, bundled with the `content-media` plugin.
In Claude Code they are named `mcp__plugin_content-media_post-stats__<tool>`, for example `mcp__plugin_content-media_post-stats__get_post_stats`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `post-stats` is connected.

## When to use

- Views, likes, comments, shares, saves and post time of one YouTube, TikTok or X post
- The source and read time of every number, and whether the platform rounded it
- Ranks 2 to 10 posts by views across platforms, with likes per 1,000 views
- Lists the latest Shorts and videos of a YouTube channel with their views
- Says which numbers a platform keeps private, and why, instead of guessing

## Examples

- "How many views does https://www.youtube.com/shorts/8bE_lGbtjyY have?"
- "Compare these 3 posts and tell me which did best."
- "List the latest Shorts of https://www.youtube.com/channel/UCt8_HnEp_DmcwHtzOKcUVaw with views."
- "How many views and likes does https://www.youtube.com/shorts/8bE_lGbtjyY have?" calls `get_post_stats`.
- "Get the stats of this TikTok: https://www.tiktok.com/@scout2015/video/6718335390845095173." calls `get_post_stats`.
- "How did this X post do? https://x.com/maubaron/status/2109135676415324185." calls `get_post_stats`.
- "Which of these did best: https://www.youtube.com/shorts/8bE_lGbtjyY and https://www.tiktok.com/@scout2015/video/6718335390845095173." calls `compare_posts`.
- "List the latest Shorts of https://www.youtube.com/@dailychartfacts with their views." calls `list_channel_stats`.

## When not to use

- "Show me the watch time and audience retention of my YouTube channel.": Post Stats reads only numbers shown on public pages and never signs in.
- "List every post on x.com/elonmusk with its views.": X serves timelines only to logged-in sessions, and this tool never uses one.
- "Like and share this TikTok for me.": Post Stats is read-only and cannot act on a post.

## Tools

- `get_post_stats`: Get a post's public stats. Read the public numbers of one post: views, likes, comments, shares, saves and the post time, for a YouTube video or Short, a TikTok video, or an X post.
- `compare_posts`: Compare posts by views. Read 2 to 10 public posts (YouTube, TikTok and X can be mixed) and rank them by views, with likes per 1,000 views for each.
- `list_channel_stats`: List a channel's latest posts. List the latest posts of a YouTube channel with the view count its public channel page shows, newest Shorts first, then videos.

## How to answer

Post Stats reads public numbers of YouTube, TikTok and X posts: views, likes, comments, shares, saves and post time. Use get_post_stats for one post address, compare_posts to rank 2 to 10 posts by views, and list_channel_stats for the latest posts of a YouTube channel or a TikTok profile's totals. Report numbers from stats, and say a number is approximate when its sources entry has exact false. A field in not_public is not shown publicly: give the reason, do not guess it. A row with status error carries a code and message: pass them on, and retry later only when retryable is true. shape_changed means the platform changed its page; say the reader needs an update. It never signs in, so it cannot read private posts, X timelines or account analytics.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
