---
name: trade-chart
description: "A candlestick chart for a public ticker, with volume, the indicators you name and a plain read. Use when the user asks things like \"Show a daily candlestick chart of AAPL with the 20-day SMA and RSI\" or \"Chart BTC-USD on the 1 hour timeframe with Bollinger Bands\"."
license: MIT
---

# Trade Chart

Shows a candlestick price chart for a public ticker, with volume and the indicators you name, plus a short plain-language read of what is on the chart.

The tools come from the `trade-chart` MCP server, bundled with the `markets` plugin.
In Claude Code they are named `mcp__plugin_markets_trade-chart__<tool>`, for example `mcp__plugin_markets_trade-chart__chart`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `trade-chart` is connected.

## When to use

- Draws a candlestick price chart with volume for a public ticker
- Adds SMA, EMA, RSI, MACD, Bollinger Bands, ATR, stochastic or VWAP
- Returns a self-contained interactive HTML chart, dark or light
- Gives a plain-language read of the last price and the indicator values
- Reads Yahoo Finance, Coinbase or ECB reference rates, with no key
- Names the source, the as-of time and Vela on every chart
- Places no orders and gives no buy or sell signal

## Examples

- "Show a daily candlestick chart of AAPL with the 20-day SMA and RSI"
- "Chart BTC-USD on the 1 hour timeframe with Bollinger Bands"
- "Show the weekly price chart of EUR/USD with MACD"
- "Show a daily candlestick chart of AAPL with the 20-day SMA and RSI." calls `chart`.
- "Chart BTC-USD on the 1 hour timeframe with Bollinger Bands." calls `chart`.
- "Show the weekly price chart of EUR/USD with MACD." calls `chart`.
- "Show TSLA 15-minute candles with a 10-period EMA and ATR." calls `chart`.
- "Chart gold on the daily timeframe, light theme, with stochastic." calls `chart`.

## When not to use

- "Buy 10 shares of AAPL at market and set a stop.": The plugin has no orders, broker or account.
- "Connect my brokerage and show my open positions.": Only public prices are read. Nothing about a person or an account is stored.
- "Will Bitcoin go up tomorrow? Give me a buy signal from the RSI.": The plugin reports prices and indicator values. It does not predict or recommend.

## Tools

- `chart`: Price chart. Candlestick price chart with indicators for a public ticker.

## How to answer

Use the chart tool to show a candlestick price chart. Pass symbol, and timeframe and indicators when the user names them. Defaults are 1d, sma and rsi, dark. An inline card shows the symbol, timeframe, last close, change and indicator readings, with a link to openUrl; do not repeat those numbers. Add the source, the asOf time and the notice. The html is the full chart page; pass it on unchanged or give the openUrl, and never rewrite it. This is informational only and not financial advice. Never say an indicator means buy, sell, overbought, oversold, bullish or bearish, never predict a price, and never suggest a trade or an order. If asked to buy, sell or connect a broker, say the plugin only draws a chart of public prices. If status is not ok, read error.hint and follow it instead of repeating the call. An unknown or misspelled symbol returns no_data. Volume is always on the candle chart; do not ask for a separate volume tool.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
