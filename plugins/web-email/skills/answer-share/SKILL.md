---
name: answer-share
description: "See how often AI answers name and link your brand, which rivals they name and which sites they cite. Use when the user asks things like \"Did this ChatGPT answer name or link acme.com? Here is the answer and its sources.\" or \"Score my brand's share across these 8 AI answers and tell me who they name instead.\"."
license: MIT
---

# Namequota

Namequota measures your brand's share of AI answers. Paste what ChatGPT, Perplexity, Google AI Mode or Claude answered to a buyer question, with its source links, and see whether the answer named you and linked you.

The tools come from the `answer-share` MCP server, bundled with the `web-email` plugin.
In Claude Code they are named `mcp__plugin_web-email_answer-share__<tool>`, for example `mcp__plugin_web-email_answer-share__score_answer_share`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `answer-share` is connected.

## When to use

- Mention share and citation share of a brand across up to 50 AI answers
- Split by engine and by buyer prompts that do not name the brand
- Ignores a prompt the answer repeats, so an echo is not a mention
- Counts how often each named rival is named and linked
- Lists the sites engines cite instead, with a short fix list
- Scores pasted text only: no engine call, no fetch, nothing stored

## Examples

- "Did this ChatGPT answer name or link acme.com? Here is the answer and its sources."
- "Score my brand's share across these 8 AI answers and tell me who they name instead."
- "Which sites do these Perplexity answers cite instead of us, and what should we write?"
- "Did this answer to 'best CRM for a small sales team' name or link Acme CRM (acme.com)? Answer: ... HubSpot and Pipedrive are good picks ... Sources: hubspot.com, g2.com." calls `score_answer_share`.
- "Here are 3 Perplexity answers about MCP SEO audit tools. What is OpenKrill's share? Domain openkrill.app." calls `score_answer_share`.
- "Compare us (Linear) with Jira and Asana in these ChatGPT answers. Who gets named most?" calls `score_answer_share`.
- "The answer to 'Is Acme CRM good?' says 'Is Acme CRM good? It depends on your team.' Does that count as a mention?" calls `score_answer_share`.
- "Which sites do these Google AI Mode answers cite when they skip milliways.si?" calls `score_answer_share`.

## When not to use

- "Ask ChatGPT right now whether it recommends my brand.": The tool never queries an AI engine or a website.
- "Write me a blog post about generative engine optimization.": Writing content is not scoring an answer.
- "What is 15 percent of 240?": Arithmetic does not involve an AI answer or a brand.

## Tools

- `score_answer_share`: Score brand share in AI answers. Call this whenever a user pastes one or more answers from ChatGPT, Perplexity, Google AI Mode, Claude or another AI assistant and asks whether the answers named or linked their brand, what share they got, which rivals or sites the answers chose instead, or what to do about it.

## How to answer

Namequota scores AI answers for one brand. When a user pastes AI answers and asks whether they named or linked a brand, call score_answer_share instead of reading the answers yourself: it applies the same fixed rules every time. Send brand, domain and the answer text plus its source links; send several answers in answers, each with prompt and engine, to get a share such as 0 of 8. Always send the prompt when you have it: buyer share counts only prompts that do not name the brand, and the prompt's words are removed from the answer before matching. The tool does not query any engine and does not fetch URLs; if the user has no answer text, ask them to paste it. If the answers name rival products and you sent no competitors, call again with those names in competitors. Report mention share and citation share as "N of M", name the rivals that lead, pass on the recommendations, and keep the summary's sample-size note and next_step.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
