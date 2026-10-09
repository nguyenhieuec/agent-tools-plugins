---
name: ai-text
description: "Paste a draft and see the habits that make writing read like it came from an AI model. Use when the user asks things like \"Which parts of this email sound like AI wrote them?\" or \"Check my blog post draft for filler and stock phrases\"."
license: MIT
---

# AI Text Check

Paste a draft and see the habits that make writing read like it came from an AI model, each marked at its exact place with a plain-language fix.

The tools come from the `ai-text` MCP server at `https://aitext.openkrill.app/mcp`, bundled with the `content-media` plugin.
Call them as MCP tools, never from a shell. In Claude Code they are named `mcp__plugin_content-media_ai-text__<tool>`, for example `mcp__plugin_content-media_ai-text__check_ai_writing`; if one is not loaded yet, find it with ToolSearch first.
If the tools are missing, run `/mcp` and check that `ai-text` is connected.

## When to use

- Flags overused words, stock openers and filler phrases
- Flags hedging openers, generic closers and chatbot sign-offs
- Flags sycophantic praise and inflated claims
- Flags lists of three, heavy em-dash use and decorative bold bullets
- Flags runs of sentences with the same length or opening
- Gives start and end offsets, a severity and a fix for each finding
- Gives a 0 to 100 score and states how it is worked out
- Uses fixed rules, with no AI model, and never stores the text

## Examples

- "Which parts of this email sound like AI wrote them?"
- "Check my blog post draft for filler and stock phrases"
- "Does this cover letter lean on clichés? Show where."
- "Which parts of this sound like AI? 'Great question! The meeting moved to Thursday at 3pm. I hope this helps! Let me know if you'd like me to expand on any section.'" calls `check_ai_writing`.
- "Check this for filler: 'The new bakery opened on Main Street — it is small — and the bread is good — especially the rye — though the queue is long — and the coffee is average.'" calls `check_ai_writing`.
- "Does this read like AI? 'Our platform helps teams to leverage a robust, seamless and innovative toolkit. It fosters collaboration, boosts productivity, and enhances well-being across the whole organization.'" calls `check_ai_writing`.
- "Find the stock phrases in this: 'It's important to note that results may vary. In conclusion, the future looks bright for local bakeries.'" calls `check_ai_writing`.
- "Is the rhythm of this too uniform? 'The bakery opens early on weekdays for the morning rush. Most customers come in for the seeded rye loaf. A small team works the ovens before dawn each day. The owner checks every batch before it goes on sale. Prices stay the same through the whole winter season. Regulars know the staff by name after a few visits.'" calls `check_ai_writing`.

## When not to use

- "Rewrite this paragraph so it sounds more human.": The plugin only flags patterns and suggests fixes; it never rewrites.
- "Was this essay written by ChatGPT? Give me a yes or no and a probability.": The plugin is not an AI detector and its score does not measure who wrote a text.
- "Check this essay for plagiarism and fact-check the claims.": The plugin has no source database and does not judge facts, grammar or originality.

## Tools

- `check_ai_writing`: Check text for AI-writing tells. Flags writing habits.

## How to answer

Use check_ai_writing when the user wants to see which parts of a text read like AI writing, or to polish a draft. Pass the text exactly as the user wrote it; never paraphrase or shorten it, because the offsets refer to it. Show the findings in text order with the excerpt, the problem and the fix, and give the score together with its basis. Say that a finding is a place to look at, not proof of who wrote the text, and never tell the user a text was or was not written by an AI. If summary.level is too_short, say the text is too short for a score and show the findings only. If truncated is true, say that more findings exist than are listed. Apply a fix only when the user asks you to rewrite; the tool itself changes nothing. If the text is longer than 12,000 characters, check it in parts and say so. The rules cover English-language habits and do not judge facts, grammar or plagiarism.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
