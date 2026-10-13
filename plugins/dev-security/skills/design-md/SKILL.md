---
name: design-md
description: "Lint and diff DESIGN.md files with the official Google linter, or write one from a web page. Use when the user asks things like \"I will paste my DESIGN.md file next. Lint it and tell me what to fix\" or \"I will paste an old and a new DESIGN.md file. Tell me if the change is breaking\"."
license: MIT
---

# DESIGN.md Linter & Extractor

DESIGN.md is the open format, started by Google, that tells a coding agent how a product should look: YAML tokens for colors, typography, rounded corners, spacing and components, then short prose sections that say how to use them.

The tools come from the `design-md` MCP server, bundled with the `dev-security` plugin.
In Claude Code they are named `mcp__plugin_dev-security_design-md__<tool>`, for example `mcp__plugin_dev-security_design-md__extract_design_md`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `design-md` is connected.

## When to use

- Lints a DESIGN.md file with the official @google/design.md 0.4.0 linter
- Reports broken token references, WCAG AA contrast failures, unused colors and section order problems
- Diffs two DESIGN.md files token by token and flags breaking removals and lint regressions
- Writes a starter DESIGN.md from the real CSS of a public webpage
- Lint and diff run on your text only, with no network calls
- Honors robots.txt and blocks private and local addresses

## Examples

- "I will paste my DESIGN.md file next. Lint it and tell me what to fix"
- "I will paste an old and a new DESIGN.md file. Tell me if the change is breaking"
- "Write a starter DESIGN.md from the styles of https://example.com"
- "Lint this DESIGN.md file and tell me what to fix." calls `lint_design_md`.
- "Is this DESIGN.md change breaking?" calls `diff_design_md`.
- "Write a starter DESIGN.md from the styles of https://example.com." calls `extract_design_md`.
- "Check my DESIGN.md against the official Google linter: ---." calls `lint_design_md`.
- "Extract the colors and fonts of https://www.w3.org as a DESIGN.md file." calls `extract_design_md`.

## When not to use

- "Extract the design tokens from our internal server at http://192.168.1.100:8080.": The extractor reads only public pages, so it cannot be used to reach a private network.
- "Open this website in a browser, log in with my account and record a video of the dashboard.": Signing in and browser automation are outside what the app does.
- "Get the saved passwords of the users of https://example.com/login.": Private user data is out of scope and not allowed.

## Tools

- `extract_design_md`: Extract a DESIGN.md file from a public webpage. Writes a starter DESIGN.md file from the CSS of one public webpage.
- `lint_design_md`: Lint a DESIGN.md file with the official linter. Lints one DESIGN.md file with the official @google/design.md 0.4.0 linter, the same check as `npx @google/design.md lint DESIGN.md`.
- `diff_design_md`: Diff two DESIGN.md files for token changes and lint regressions. Compares two versions of a DESIGN.md file the way `npx @google/design.md diff` does.

## How to answer

Use lint_design_md to check a DESIGN.md file against the official @google/design.md linter; report the errors first, then the warnings, and quote the token path of each. Use diff_design_md to compare an old and a new DESIGN.md file; say whether the change is breaking or a lint regression, then list the token changes. Use extract_design_md to write a starter DESIGN.md from a public webpage; tell the user that the type size ramp is a default and that values marked default were not found on the page. Only public HTTP and HTTPS pages can be extracted: private addresses, local hostnames and localhost are blocked, and pages that robots.txt disallows are not fetched. lint_design_md and diff_design_md need the pasted file text, not a path or URL; if the user has not pasted it, ask for it before you call.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
