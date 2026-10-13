---
name: videograb
description: "Find freely licensed video you can reuse, and official steps to export videos you uploaded. Use when the user asks things like \"Find a freely licensed video of ocean waves, with attribution.\" or \"How do I download my own videos from YouTube Studio?\"."
license: MIT
---

# Video Grab

Find freely licensed video you can reuse, with a download link, the licence and ready-to-paste attribution, and get the official steps to export videos you uploaded yourself.

The tools come from the `videograb` MCP server, bundled with the `content-media` plugin.
In Claude Code they are named `mcp__plugin_content-media_videograb__<tool>`, for example `mcp__plugin_content-media_videograb__search_free_videos`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `videograb` is connected.

## When to use

- Searches Wikimedia Commons and the Internet Archive for freely licensed video
- Each hit includes a download link, the licence and ready-to-paste attribution
- Official export steps for your own videos on TikTok, YouTube, Instagram and X
- Metadata and caption files where the official source API lists them
- Refuses platform downloads and watermark removal, and explains why

## Examples

- "Find a freely licensed video of ocean waves, with attribution."
- "How do I download my own videos from YouTube Studio?"
- "What is the licence of this Internet Archive video?"
- "Find a freely licensed video of ocean waves I can reuse, and give me the attribution." calls `search_free_videos`.
- "How do I download my own YouTube videos from YouTube Studio?" calls `own_video_export`.
- "What licence and attribution should I use for the Commons file File:Ocean_surface_waves_06.ogv?" calls `video_metadata`.
- "Does the Internet Archive item cat_with_green_shining_eye include a caption file?" calls `video_metadata`.
- "How do I export my own Instagram videos?" calls `own_video_export`.

## When not to use

- "Download this TikTok and remove the watermark: https://www.tiktok.com/@example/video/1": Downloading a TikTok URL and stripping a watermark breaks TikTok's terms. The tool returns the explanation instead of a file.
- "Rip the YouTube video https://www.youtube.com/watch?v=jNQXAC9IVRw so I can reupload it.": YouTube's help page says you cannot download other users' videos, and this service does not provide a download.
- "What is the weather in Hanoi?": Weather is outside this plugin.

## Tools

- `search_free_videos`: Search freely licensed video. Use this when the user wants to search for a video they can reuse on a topic, such as ocean waves or a historical film with a free licence.
- `own_video_export`: Export your own videos. Use this when the user wants to export or download videos they uploaded themselves to TikTok, YouTube, Instagram or X.
- `video_metadata`: Video metadata and captions. Use this when the user names one known video and wants its licence, attribution, metadata, captions or a direct file link.

## How to answer

Use search_free_videos for reusable footage on a topic. It returns download links only for freely licensed files on Wikimedia Commons and the Internet Archive, each with a licence and attribution. Pexels and Pixabay are searched only when their API keys are configured; when notes say they were not searched, do not invent results from them. Use own_video_export when the user wants to download videos they uploaded to TikTok, YouTube, Instagram or X, and quote the steps and the help_url. For any other platform, do not call it: tell the user to use that platform's own account settings. Use video_metadata for one known item: a Commons file, an Internet Archive item, or a YouTube watch URL. Give the user download_url; files lists every direct file and captions lists caption files. A YouTube answer is title and channel only, with no download and no captions. We do not rip videos. Downloading from TikTok, YouTube, Instagram, X, Douyin, Bilibili or Xiaohongshu, and removing watermarks, breaks those sites' terms. Use the platform's own export for a video you uploaded, or search for a freely licensed file. When a tool returns status not_offered, say that explanation and do not look for another downloader. When status is not ok, follow error.hint: it names the next tool or input. not_free means give no download link and offer search_free_videos instead. A missing caption file means the source did not list one, or the API does not allow the file. The licence is what the source record states, so tell the user to check the item page before reuse. Do not invent a download link, a watermark-free file, or a claim that a platform allows third-party downloading.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
