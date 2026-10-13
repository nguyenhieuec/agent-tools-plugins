---
name: tenrung
description: "Score how AI-native a team is on 10 lines and get a 90-day plan and an AI champion role card. Use when the user asks things like \"Score how AI-native my team is and give me a 90-day plan.\" or \"We have an AI policy and a champion but no training. Where do we stand?\"."
license: MIT
---

# Tenrung

Tenrung scores how far a team has come from paying for AI seats to working with AI every day. The user scores 10 lines from 0 to 3, so the total runs 0 to 30.

The tools come from the `tenrung` MCP server, bundled with the `office` plugin.
In Claude Code they are named `mcp__plugin_office_tenrung__<tool>`, for example `mcp__plugin_office_tenrung__score_ai_native_team`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `tenrung` is connected.

## When to use

- Scores 10 lines from 0 to 3 and returns a total out of 30 with one of 4 bands
- Picks the 3 first moves from the weakest lines
- Builds a 90-day plan with every step marked start, tighten or keep
- Returns the AI champion role card with 5 jobs and the weekly hours
- Gives a shareable result link that holds only the 10 numbers

## Examples

- "Score how AI-native my team is and give me a 90-day plan."
- "We have an AI policy and a champion but no training. Where do we stand?"
- "What does an AI champion do each week?"
- "Score my team: owner 3, reps 2, policy 3, training 2, tracks 1, library 2, backlog 1, wins 2, blocked time 1, hours saved 0. Team name Northwind." calls `score_ai_native_team`.
- "We pay for ChatGPT seats and that is it. No owner, no policy, no training, nothing measured. Score us 0 on everything." calls `score_ai_native_team`.
- "Rate us 3 on every line of the AI-native scorecard and tell me what to keep doing." calls `score_ai_native_team`.
- "Our scorecard code is 2222222222. What band is that and what should month 1 look like?" calls `score_ai_native_team`.
- "We have a champion with time (3), reps meet (3), policy pinned (3), monthly training (3), 3 tracks (2), library (2), ranked backlog (2), wins posted (2), blocked time (2), hours shown (2). What is our score and the champion's weekly jobs?" calls `score_ai_native_team`.

## When not to use

- "Write me a company AI policy for a 40-person law firm.": The scorecard scores a team; it does not write policies.
- "What is 17 times 24?": Arithmetic has nothing to do with AI adoption.
- "Which AI model is the best for coding right now?": The scorecard has no model rankings.

## Tools

- `score_ai_native_team`: Score how AI-native a team is. Score a team or company on the 10-line AI-native scorecard and get a 90-day plan.

## How to answer

Tenrung scores a team on 10 lines, each 0 to 3. Before you call score_ai_native_team, ask the user to score every line, and read them the 0 to 3 meaning from the input schema when they are unsure. Never invent a score. If the user gives a story instead of numbers, propose a score per line and ask them to confirm it. After the call, lead with the total and the band, then the 3 first moves, then offer the 90-day plan and the champion role card. Give the share_url when the user wants to share the result.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
