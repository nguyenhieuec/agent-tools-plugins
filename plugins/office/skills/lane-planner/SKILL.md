---
name: lane-planner
description: "Lane Planner is a planning tool for people with many interests and limited hours. Use when the user asks things like \"I have 10 h a week for an iOS app, a Swift book, Figma, guitar and Japanese. Plan my week in lanes.\" or \"Here is my ticked plan from last week. Review it and plan the next 7 days.\"."
license: MIT
---

# Lane Planner

Lane Planner is a planning tool for people with many interests and limited hours. Ask "I have 10 hours a week for an iOS app, a Swift book, Figma, guitar and Japanese: plan my week" or "here is last week's list with what I ticked: plan the next one".

The tools come from the `lane-planner` MCP server at `https://lanes.openkrill.app/mcp`, bundled with the `office` plugin.
Call them as MCP tools, never from a shell. In Claude Code they are named `mcp__plugin_office_lane-planner__<tool>`, for example `mcp__plugin_office_lane-planner__plan_lanes`; if one is not loaded yet, find it with ToolSearch first.
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

- `plan_lanes`: Plan interests in lanes. Use this when the user has several interests, projects or hobbies and asks for a plan for the next days or week, for example "I have 10 hours a week for an app, a book, guitar and Japanese: plan my week".
- `review_week`: Review a week and re-plan. Use this when the user comes back after a lane plan and says what got done, for example "here is my list with what I ticked, plan next week".

## How to answer

Use plan_lanes when the user lists several interests, projects or hobbies and wants a plan for the coming days. Send every interest with a few words of note, the weekly hours, and the user's local date as start_date. Send next_steps for the build interest when the user named concrete steps. Set lane only when the user chose one. Show the four lanes, the spine sentence and the day plan, and give todo_text word for word, header lines and #ids included, so the user can paste it into a todo app and review_week can read it back. Ask whether the spine sentence fits; send spine with their words to replace it. Use review_week after the plan's last day: send the pasted plan text with [x] on finished lines as checklist, or send lanes with done and not_done titles. When the user only says in words what got done, plan_lanes with done, not_done and each interest's lane does the same review. Tell the user what got done, the changes, and the new todo_text. It is a planning tool; do not describe it as a treatment for anything.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
