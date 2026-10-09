---
name: live-card
description: "Turn a cover and a short animation, supplied as HTML, into an Apple Live Photo, an MP4 and a GIF. Use when the user asks things like \"Make a Live Photo of a blue card that says Launch day, with the title fading in\" or \"Turn this cover and this 3 second CSS animation into an Apple Live Photo\"."
license: MIT
---

# Live Card

Turn a cover and a short animation, both supplied as HTML, into an Apple Live Photo plus an MP4 and a GIF.

The tools come from the `live-card` MCP server at `https://livecard.openkrill.app/mcp`, bundled with the `content-media` plugin.
Call them as MCP tools, never from a shell. In Claude Code they are named `mcp__plugin_content-media_live-card__<tool>`, for example `mcp__plugin_content-media_live-card__render_live_card`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `live-card` is connected.

## When to use

- Renders a self-contained HTML cover to a JPEG still
- Renders a self-contained HTML animation of at most 3 seconds to a silent H.264 video
- Pairs the JPEG and MOV with one Apple content identifier and a still-image-time mark
- Also returns an MP4 and a GIF that play without an Apple device
- Returns download links that work for 24 hours and are then deleted
- Refuses remote images, fonts and scripts so a render makes no network request

## Examples

- "Make a Live Photo of a blue card that says Launch day, with the title fading in"
- "Turn this cover and this 3 second CSS animation into an Apple Live Photo"
- "Export a GIF and an MP4 of this self-contained HTML card"
- "Make a Live Photo of a blue card that says Launch day. Use a self-contained HTML cover and a 3 second fade-in animation." calls `render_live_card`.
- "Render a Live Card of a white card where the word Hello slides up in black, at 720 by 1280, 2 seconds, 30 fps, and pair the still to 1 second." calls `render_live_card`.
- "Is my Live Card ready? The job id is the one you just got." calls `live_card_status`.
- "Give me the GIF and the MP4 of that Live Card too, not only the Apple pair." calls `live_card_status`.
- "How many Live Cards can I render today, and how long do the links last?" calls `render_live_card`.

## When not to use

- "Download https://example.com/logo.png and animate it into a Live Photo.": A render that fetched the URL would make a network request from the renderer. The tool returns external_url for any http or https URL in the HTML.
- "Make a 30 second Live Photo of this chart.": duration_ms cannot exceed 3000. A longer value is invalid_input.
- "Save this Live Photo into my Photos library and set it as my lock screen.": The plugin only makes files. It has no access to a Photos library, a camera roll, or a device.

## Tools

- `render_live_card`: Render a Live Photo card. Use this when the user wants an Apple Live Photo, a short animated card, or an MP4 or GIF from a design you can draw in HTML.
- `live_card_status`: Check a Live Card render. Use this when a Live Card render was queued and you have its job_id.

## How to answer

Use render_live_card when the user wants an Apple Live Photo, a short animated card, or an MP4 or GIF made from a design. Write cover_html and motion_html yourself as complete, self-contained HTML. Inline every image as SVG or a data: URL. Do not use http, https, or protocol-relative URLs: those calls are refused. Keep each document under 60,000 characters. Default size is 1080 by 1440. Duration is at most 3000 milliseconds. Drive the animation with CSS animations or requestAnimationFrame over that duration. The renderer advances a virtual clock. Do not depend on wall-clock time or on assets loading from the network. After render_live_card returns status queued, call live_card_status with the same job_id until status is ready. Then give the user all four download links and say the JPEG and MOV share content_identifier and that the links expire at expires_at. The MP4 and GIF are the fallback for non-Apple players. If status is failed, read error_code and message and fix the HTML or tell the user the limit. A renderer_unavailable error means the renderer is off, not that the HTML was wrong. A livecard_network_limit or livecard_global_limit error means the daily cap is spent until 00:00 UTC. These tools only make files. They cannot import into Photos, read a camera roll, or fetch a web page.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
