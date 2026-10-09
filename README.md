# Agent Tools plugins

Plugins for Claude Code and Cursor that connect an agent to the [Agent Tools](https://tools.openkrill.app/) MCP servers by OpenKrill.
Each plugin is one tool family: a skill per server that says when to use it, plus the remote MCP config for those servers.
9 plugins bundle 44 public MCP servers. None needs an API key, a sign-up or a local process.

## Install in Claude Code

```bash
claude plugin marketplace add nguyenhieuec/agent-tools-plugins
claude plugin install consumer@openkrill
```

Install any other family the same way, then start a new session.
Inside a session, `/plugin marketplace add nguyenhieuec/agent-tools-plugins` and `/plugin` do the same.

| Plugin | What it covers | Install |
| --- | --- | --- |
| `dev-security` | Package health, CVE triage, software end-of-life and licence facts, and AI coding agent news. | `claude plugin install dev-security@openkrill` |
| `web-email` | Audit a site's SEO and AI crawler access, check a domain's mail delivery, see if a name's domain is free, and report email engagement. | `claude plugin install web-email@openkrill` |
| `commerce` | Store price tracking, catalog and feed checks, HS codes, duty and landed cost, and competitor ad angles. | `claude plugin install commerce@openkrill` |
| `office` | Business days and holidays, US tax figures, EU VAT and company registries, document files, ready-to-send links and weekly lane plans. | `claude plugin install office@openkrill` |
| `sales-hiring` | Buyer signals, an inbound pitch scoreboard, open jobs at named companies, remote jobs by country, and a hire-or-automate split. | `claude plugin install sales-hiring@openkrill` |
| `research` | Find papers, check references and DOIs, read US federal regulations, compare country statistics, and make a school digest. | `claude plugin install research@openkrill` |
| `markets` | Technical indicators over a price series, candlestick charts for a ticker, and liquidity pool fee and volume checks. | `claude plugin install markets@openkrill` |
| `consumer` | US recalls, car reports by VIN, food facts by barcode, and travel advice with live airport status. | `claude plugin install consumer@openkrill` |
| `content-media` | Post and video script drafts, AI-writing checks, transcripts, free video, Live Photos, design taste, plus YouTube transcripts, app reviews and X posts. | `claude plugin install content-media@openkrill` |

## Install in Cursor

Cursor reads the same plugins through `.cursor-plugin/marketplace.json`.
A team admin can import this repository under Dashboard, Plugins & MCPs, Team Marketplaces, Import from Repo, and members install from Customize.
To try one family alone, clone this repository and copy `plugins/<family>` to `~/.cursor/plugins/local/<family>`, then run Developer: Reload Window.
The Cursor CLI loads a family directly: `cursor-agent --plugin-dir plugins/<family>`.

Each family also has a plain `mcp.json` you can merge into `~/.cursor/mcp.json` without the skills.

## Layout

```
.claude-plugin/marketplace.json   Claude Code marketplace
.cursor-plugin/marketplace.json   Cursor marketplace
plugins/<family>/
  .claude-plugin/plugin.json      Claude Code plugin manifest
  .cursor-plugin/plugin.json      Cursor plugin manifest
  .mcp.json                       MCP servers for Claude Code
  mcp.json                        MCP servers for Cursor
  skills/<server>/SKILL.md        One skill per MCP server
```

## Privacy and support

The servers are read-only and keep no account data. See https://tools.openkrill.app/privacy and https://tools.openkrill.app/terms.
Questions and bugs: https://tools.openkrill.app/support or support@openkrill.app.

This repository is generated from the Agent Tools plugin listings. Edits made here are overwritten on the next release.

## License

MIT
