---
name: transcript-kit
description: "Turn a recording you own into a timestamped transcript with SRT and VTT captions and clips. Use when the user asks things like \"Transcribe this recording and give me SRT captions\" or \"Find the best 60 second clips in my talk\"."
license: MIT
---

# Transcript Kit

Turn a recording you own into a timestamped transcript with SRT and VTT captions, search what you have transcribed, find the best short clips in a talk, and translate the captions with a dub script.

The tools come from the `transcript-kit` MCP server, bundled with the `content-media` plugin.
In Claude Code they are named `mcp__plugin_content-media_transcript-kit__<tool>`, for example `mcp__plugin_content-media_transcript-kit__transcribe_media`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `transcript-kit` is connected.

## When to use

- Transcribes an uploaded audio or video file or a direct file link, with timestamps
- Returns SRT and VTT caption files ready to use
- Accepts files up to 15 MB and 15 minutes in mp3, m4a, mp4, wav, ogg, flac or webm
- Searches your own transcripts for topics, hooks and repeated themes
- Suggests the best 30 to 60 second clips with start, end and a hook line, as text
- Translates captions into about 100 languages and writes a dub script sized to each segment
- Free with daily limits of minutes and characters; shows how much is left
- Keeps transcripts 24 hours, then deletes them; never keeps the media
- Does not download from YouTube, TikTok, Douyin or other platforms

## Examples

- "Transcribe this recording and give me SRT captions"
- "Find the best 60 second clips in my talk"
- "Translate my captions to Spanish with a dub script"
- "This is a talk our team recorded and published, so we have the rights to transcribe it. Transcribe it and give me SRT captions: https://tools.openkrill.app/samples/clipper-talk.mp4." calls `transcribe_media`.
- "This LibriVox recording is in the public domain, so I have the rights to transcribe it. Please transcribe it in English: https://archive.org/download/short_poetry_001_librivox/because_I_could_not_stop_for_death_dickinson_64kb.mp3." calls `transcribe_media`.
- "Which of my transcripts mention backups, and at what time?" calls `search_transcripts`.
- "Translate the captions of that talk into Spanish and write me a dub script." calls `translate_subtitles`.
- "Find me the best 2 clips of 10 to 20 seconds in that talk, with a hook line for each." calls `suggest_clips`.

## When not to use

- "Download every video from this TikTok creator and transcribe them all.": The plugin refuses platform pages and bulk downloads; it works on an uploaded file or a direct file link the user may use.
- "Transcribe this YouTube video: https://www.youtube.com/watch?v=abc123": A video page is not a media file, and the plugin does not fetch YouTube, TikTok, Douyin or similar pages.
- "Cut my video into a 60 second clip with subtitles burned in and post it to my channel.": The plugin returns text only: it cuts no video and posts nothing.

## Tools

- `transcribe_media`: Transcribe an audio or video file. Use this when the user wants the words of a recording they own or may use, for example "transcribe this podcast episode", "make subtitles for my video", "what is said in this file".
- `search_transcripts`: Search your saved transcripts. Use this when the user wants to find something across the transcripts they made in the last day, for example "where did I talk about pricing", "which hooks did I open with", "what themes keep coming up".
- `suggest_clips`: Suggest short clips from a transcript. Use this when the user wants the best short stretches of a talk to cut as clips, for example "find the best 60 seconds", "which parts would make good shorts".
- `translate_subtitles`: Translate subtitles and make a dub script. Use this when the user wants captions or a spoken script in another language, for example "translate these subtitles to Spanish", "make a French dub script for my talk".

## How to answer

Use transcribe_media when the user wants the words of an audio or video file they uploaded or a direct link to an audio or video file. First make sure the user owns the recording or has the rights to use it; ask if it is unclear, and set confirm_rights true only after they say so. Never use it for a page of YouTube, TikTok, Douyin or any other platform, and never try to fetch other people's videos. Tell the user to upload the file instead. Use translate_subtitles for captions or a dub script in another language, from a transcript_id or the text of an SRT or VTT file the user owns (then set source_lang and confirm_rights). Say that translations are machine output to check before publishing. Use search_transcripts to find passages or repeated themes in the transcripts the user made earlier, and suggest_clips for the best short stretches of one transcript. If transcribe_media returns a library_id, pass it to the other tools. Say that transcripts are deleted after 24 hours and the media is never kept. Report usage.seconds_left_today when the daily allowance is low, and if a call is refused for the daily limit say when it resets (00:00 UTC). Transcripts can contain mistakes: say so when the user plans to publish one. Clip scores only rank clips against each other and do not predict views. These tools only read and rank text: they cannot cut, edit or publish video.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
