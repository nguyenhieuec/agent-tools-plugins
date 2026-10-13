---
name: agent-trace
description: "Read a tool-call log from an AI agent run and see which calls failed, looped or were slow. Use when the user asks things like \"Analyze this agent tool-call log and tell me why the run failed.\" or \"Find retry loops and slow calls in this Claude Code session JSONL.\"."
license: MIT
---

# Agent Trace Check

Agent Trace Check reads a tool-call log from an AI agent run and says what went wrong: which tools failed and how often, where the agent retried the same call in a loop, which calls it repeated with the same arguments, and which calls were slow.

The tools come from the `agent-trace` MCP server, bundled with the `dev-security` plugin.
In Claude Code they are named `mcp__plugin_dev-security_agent-trace__<tool>`, for example `mcp__plugin_dev-security_agent-trace__analyze_trace`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `agent-trace` is connected.

## When to use

- Reads OpenAI, Anthropic, Claude Code, MCP, LangSmith, OpenTelemetry and flat JSON tool-call logs
- Pairs each tool call with its result by id, or in order when there are no ids
- Counts failures per tool and names the most common error
- Finds retry loops: back-to-back calls that repeat or follow a failure
- Finds the same call made again with the same arguments
- Lists slow calls by a fixed limit and by each tool's median time

## Examples

- "Analyze this agent tool-call log and tell me why the run failed."
- "Find retry loops and slow calls in this Claude Code session JSONL."
- "Which tools failed most in this MCP log, and with what error?"
- "Why did my agent run get stuck? Here is the tool log." calls `analyze_trace`.
- "Find failing tool calls in these OpenAI messages." calls `analyze_trace`.
- "Check this Claude conversation for tool errors." calls `analyze_trace`.
- "Which MCP calls were slow? Use 2 seconds as slow." calls `analyze_trace`.
- "Did my agent repeat itself? [." calls `analyze_trace`.

## When not to use

- "Run my agent again and see if it works this time.": The plugin has no access to the user's agent or tools.
- "Connect to my LangSmith project and pull yesterday's traces.": The plugin does not log in to LangSmith or any tracing service.
- "What is the capital of France?": General knowledge is outside a tool-call log.

## Tools

- `analyze_trace`: Analyze an agent tool-call log. Use this when the user pastes a log of an AI agent's tool calls and wants to know which calls failed, where the agent retried in a loop, which calls it repeated, or which were slow.

## How to answer

Use analyze_trace when the user pastes or attaches a log of an AI agent's tool calls and asks why the run failed, where it looped, or what was slow. Send the log text unchanged as trace. Change slow_ms or retry_min only when the user names a different limit. Show the findings first, worst first, then the per-tool table if the user wants detail. Do not claim the tool re-ran any call or saw anything outside the pasted log. It only reads the text it is given.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
