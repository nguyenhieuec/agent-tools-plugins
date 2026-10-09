---
name: packages
description: "Check an npm or Python package, or a package.json, for vulnerabilities and upkeep. Use when the user asks things like \"Is lodash 4.17.15 vulnerable?\" or \"Is the npm package request still maintained, and is there a safer alternative?\"."
license: MIT
---

# Package Health Check

Check an npm or Python package before you depend on it, or the dependencies of a package.json you already have, and get the facts from public sources.

The tools come from the `packages` MCP server, bundled with the `dev-security` plugin.
In Claude Code they are named `mcp__plugin_dev-security_packages__<tool>`, for example `mcp__plugin_dev-security_packages__check_package`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `packages` is connected.

## When to use

- Lists known vulnerabilities of an npm or PyPI package version with severity and the fixed version
- Shows the license, deprecation notice, last release date and release pace of a package
- Shows how many packages depend on a package and its weekly npm downloads
- Checks every dependency of a pasted package.json for known vulnerabilities
- Checks candidate replacements for a deprecated or abandoned package

## Examples

- "Is lodash 4.17.15 vulnerable?"
- "Is the npm package request still maintained, and is there a safer alternative?"
- "Check my package.json for known vulnerabilities"
- "Is lodash 4.17.15 vulnerable?" calls `check_package`.
- "Is the npm package left-pad still maintained?" calls `check_package`.
- "What license is the Python package requests under?" calls `check_package`.
- "Is there a safer alternative to request?" calls `check_package`.
- "Check my package.json for known vulnerabilities." calls `check_package_json`.

## When not to use

- "Write a function that removes duplicates from an array in TypeScript.": A coding task names no package whose health needs to be looked up.
- "Here is my repository source code and my npm token. Audit all of it for security problems.": The plugin never needs source code or credentials, and it does not scan code.
- "Is the Rust crate serde vulnerable?": Only the npm and PyPI ecosystems are supported, so a crate cannot be checked.

## Tools

- `check_package`: Check a package's health. Use this when the user asks whether an npm or PyPI package is vulnerable, maintained, deprecated or safe to use, or what license it has: "is lodash 4.17.15 vulnerable?", "is this npm package maintained?", "what license is this package?", "safer alternative to request".
- `check_package_json`: Check package.json for vulnerabilities. Use this when the user asks to check their package.json, or the dependencies of a project, for known vulnerabilities: "check my package.json for known vulnerabilities".

## How to answer

Use check_package for one npm or PyPI package: pass the exact name, ecosystem (npm unless the user says Python or PyPI) and the version when the user gave one; without a version the latest is checked. Use check_package_json when the user pastes a package.json. Pass only the package.json text and never ask for source code, tokens or private package names. Quote the version that was checked. A severity of null means the advisory publishes no rating. Maintenance is release recency only, so say that a stable package with few releases can still be fine. When a package is deprecated or stale, suggest alternatives from the deprecation notice or your own knowledge, say that they are suggestions, and call check_package on each before recommending it. Ranges in a package.json are checked at their lowest version; say so.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
