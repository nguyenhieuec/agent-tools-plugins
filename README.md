# Agent Tools plugins

Plugins for Claude Code and Cursor that connect an agent to the [Agent Tools](https://tools.openkrill.app/) MCP servers by OpenKrill.
Each plugin is one tool family: a skill per server that says when to use it, plus the remote MCP config for those servers.
9 plugins bundle 54 public MCP servers. None needs an API key, a sign-up or a local process.

## Install in Claude Code

```bash
claude plugin marketplace add nguyenhieuec/agent-tools-plugins
claude plugin install dev-security@openkrill
```

Start a new session and ask: "Is lodash 4.17.15 vulnerable, and which version fixes it?"
Install any other family the same way.
Inside a session, `/plugin marketplace add nguyenhieuec/agent-tools-plugins` and `/plugin` do the same.

| Plugin | What it covers | Install |
| --- | --- | --- |
| `dev-security` | Package health, CVE triage, software end-of-life and licence facts, DESIGN.md lint and diff, and AI coding agent news. | `claude plugin install dev-security@openkrill` |
| `web-email` | Audit a site's SEO and AI crawler access, read a web page as clean Markdown, check a domain's mail delivery, see if a name's domain is free, and report email engagement. | `claude plugin install web-email@openkrill` |
| `commerce` | Store price tracking, catalog and feed checks, HS codes, duty and landed cost, US product-safety certificates, and competitor ad angles. | `claude plugin install commerce@openkrill` |
| `office` | Business days and holidays, US tax figures, EU VAT and company registries, document files, ready-to-send links, weekly lane plans and an AI-native team scorecard. | `claude plugin install office@openkrill` |
| `sales-hiring` | Buyer signals, an inbound pitch scoreboard, open jobs at named companies, remote jobs by country, and a hire-or-automate split. | `claude plugin install sales-hiring@openkrill` |
| `research` | Find papers, check references and DOIs, read US federal regulations, compare country statistics, and make a school digest. | `claude plugin install research@openkrill` |
| `markets` | Technical indicators over a price series, candlestick charts for a ticker, liquidity pool fee and volume checks, and token red-flag screens. | `claude plugin install markets@openkrill` |
| `consumer` | US recalls, car reports by VIN, food facts by barcode, and travel advice with live airport status. | `claude plugin install consumer@openkrill` |
| `content-media` | Trending topics and post ideas, post and video script drafts, AI-writing checks, transcripts, free video, Live Photos, design taste, public views and likes of posts, plus YouTube transcripts, app reviews and X posts. | `claude plugin install content-media@openkrill` |

## Install in Cursor

Cursor reads the same plugins through `.cursor-plugin/marketplace.json`.
A team admin can import this repository under Dashboard, Plugins & MCPs, Team Marketplaces, Import from Repo, and members install from Customize.
To try one family alone, clone this repository and copy `plugins/<family>` to `~/.cursor/plugins/local/<family>`, then run Developer: Reload Window.
The Cursor CLI loads a family directly: `cursor-agent --plugin-dir plugins/<family>`.

Each family also has a plain `mcp.json` you can merge into `~/.cursor/mcp.json` without the skills.

## Not sure which plugin?

Add the API server alone and ask it: its `index_tools` tool searches every Agent Tools server by task in plain words and answers with the tool name and the MCP address to add.

```bash
claude mcp add --transport http openkrill https://api.openkrill.app/mcp
```

The plugin servers do not list `index_tools`: each one describes only its own tools.

## Every tool on one server

The gateway at `https://api.openkrill.app/gateway/mcp` lists the tools of every server, named `<server>__<tool>` (for example `packages__check_package`), and runs each call on the owning server with its usual per-network limits.
The full list has every tool of every server, about 200 KB, so a client that loads every tool into the model should pick a family with `?servers=`:

```bash
claude mcp add --transport http openkrill-dev-security "https://api.openkrill.app/gateway/mcp?servers=dev-security"
```

`?servers=` takes a family name or server names, comma-separated.

The same tools work in Zapier without code: join the OpenKrill Tools integration with [this invite link](https://zapier.com/developer/public-invite/247678/f010eca6378be3e50a9410dfce17b4df/).
It is early and private until Zapier reviews it; every action is one gateway call and needs no key.

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

There are no accounts. Most servers keep only daily call counts and a short cache of public data.
Some keep what their tools say they keep: Store Price Tracker keeps public store snapshots, Taste Profile and Outreach Desk keep a profile under a key only you hold, and Transcript Kit keeps a transcript for 24 hours.
What each server receives, keeps and for how long: https://tools.openkrill.app/privacy#per-server. Terms: https://tools.openkrill.app/terms.
Questions and bugs: https://tools.openkrill.app/support or support@openkrill.app.

This repository is generated from the Agent Tools plugin listings. Edits made here are overwritten on the next release.

## License

MIT
