---
name: lane-planner
description: "Turn a list of interests into a day-by-day plan for 3 to 7 days, with checkbox text for a todo app. Use when the user asks things like \"I have 10 h a week for an iOS app, a Swift book, Figma, guitar and Japanese. Plan my week in lanes.\" or \"Here is my ticked plan from last week. Review it and plan the next 7 days.\"."
license: MIT
---

# Lane Planner

Lane Planner turns a list of interests into a day-by-day plan for the next 3 to 7 days, plus checkbox text you can paste into any todo app. It is for people with many interests and limited hours. Ask "I have 10 hours a week for an iOS app, a Swift book, Figma, guitar and Japanese: plan my week" or "here is last week's list with what I ticked: plan the next one".

The tools come from the `lane-planner` MCP server, bundled with the `office` plugin.
In Claude Code they are named `mcp__plugin_office_lane-planner__<tool>`, for example `mcp__plugin_office_lane-planner__plan_lanes`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `lane-planner` is connected.

## When to use

- Sorts a list of interests into build, reading, open and parking-lot lanes
- Writes one spine sentence that points the lanes at the same direction
- Plans the next 3 to 7 days with minutes per task that add up to the weekly hours
- Returns the plan as plain checkbox text to paste into any todo app
- Reviews a ticked week and returns the re-plan with unfinished steps carried forward
- No account and no stored data: the plan text carries everything the review needs

## Examples

- "I have 10 h a week for an iOS app, a Swift book, Figma, guitar and Japanese. Plan my week in lanes."
- "Here is my ticked plan from last week. Review it and plan the next 7 days."
- "Plan 5 days for my novel, a design course and running, 6 hours total, Sunday off."
- "I have 10 hours a week for: an indie iOS app (ship a habit tracker to TestFlight), a Swift concurrency book, Figma auto layout, running, guitar and Japanese. Plan my next 7 days from 2026-10-08." calls `plan_lanes`.
- "Plan 5 days for my novel (finish draft two), a design course and running. 6 hours a week, Sunday off, starting 2026-10-09." calls `plan_lanes`.
- "Here is last week's plan with what I ticked. Review it and plan the next 7 days." calls `review_week`.
- "I only finished 1 of my 4 app tasks last week but did the guitar side quest. Re-plan the week." calls `review_week`.
- "My spine is 'ship a game by spring'. Plan my week for a Godot game, a game design book, pixel art and piano, 8 hours." calls `plan_lanes`.

## When not to use

- "Add a dentist appointment to my calendar for Tuesday at 3 pm.": The plugin plans interests into lanes; it has no calendar access.
- "Can this planner diagnose why I cannot focus?": The plugin plans hours and tasks; it makes no diagnosis of any kind.
- "What is the capital of Japan?": General knowledge is outside a planning tool.

## Tools

- `plan_lanes`: Plan interests in lanes. Use this for a first plan: the user lists several interests, projects or hobbies and asks for a plan for the next days or week, such as "10 hours a week for an app, a book, guitar and Japanese: plan my week".
- `review_week`: Review a week and re-plan. Use this when a plan from plan_lanes has ended and the user says what got done, such as "here is my list with what I ticked, plan next week".

## How to answer

Use plan_lanes when the user lists several interests, projects or hobbies and wants a plan for the coming days. Send every interest with a few words of note, the weekly hours, and the user's local date as start_date. Send next_steps for the build interest when the user named concrete steps. Set lane only when the user chose one. Show the four lanes, the spine sentence and the day plan, and give todo_text word for word, header lines and #ids included, so the user can paste it into a todo app and review_week can read it back. Ask whether the spine sentence fits; send spine with their words to replace it. Use review_week after the plan's last day for any plan that is in this chat: send the pasted plan text with [x] on finished lines as checklist, or lanes with done, not_done and hours_per_week. Use plan_lanes with done and not_done only when the earlier plan is not in this chat and the user retells the week in words; set each interest's lane to its lane last week. Tell the user what got done, the changes, and the new todo_text. It is a planning tool; do not describe it as a treatment for anything.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
