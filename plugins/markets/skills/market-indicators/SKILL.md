---
name: market-indicators
description: "Calculate SMA, EMA, RSI, MACD, Bollinger Bands, ATR, stochastic and VWAP over a price series. Use when the user asks things like \"Calculate the RSI and MACD for EUR/USD on the daily chart\" or \"Show the 50-day SMA and Bollinger Bands for USD/JPY\"."
license: MIT
---

# Market Indicators

Calculate technical indicators over a price series: SMA, EMA, RSI, MACD, Bollinger Bands, ATR, stochastic oscillator and VWAP. Each answer gives the latest value and a short series of recent values, with the source and the date of the last price.

The tools come from the `market-indicators` MCP server, bundled with the `markets` plugin.
In Claude Code they are named `mcp__plugin_markets_market-indicators__<tool>`, for example `mcp__plugin_markets_market-indicators__compute_indicators`.
If the tool is not loaded yet, load it with ToolSearch, then call it in your next step as a normal tool call, the same way you call any other tool. It is not a command line program: Bash, curl and scripts cannot reach it.
If the tools are missing, run `/mcp` and check that `market-indicators` is connected.

## When to use

- Calculates SMA, EMA, RSI, MACD, Bollinger Bands, ATR, stochastic oscillator and VWAP
- Works on candles you paste, up to 250, with no outside lookup
- Reads ECB daily euro reference rates for 30 currencies, daily or weekly, and calculates close-based indicators
- Returns the latest value and a short series of recent values, with the settings used
- Names the source, the library and the date of the last price in every answer
- Reports numbers only: no signals, no recommendations, no trading features

## Examples

- "Calculate the RSI and MACD for EUR/USD on the daily chart"
- "Show the 50-day SMA and Bollinger Bands for USD/JPY"
- "Calculate ATR and stochastic from these OHLC candles"
- "Calculate the RSI and MACD for EUR/USD on the daily chart." calls `get_indicators`.
- "Show the 50-day SMA and Bollinger Bands for USD/JPY." calls `get_indicators`.
- "Give me the weekly 10-period EMA for GBP/CHF." calls `get_indicators`.
- "Calculate ATR and the stochastic oscillator from these 20 daily candles: 2026-09-01 H 10.6 L 9.7 C 10.2, 2026-09-02 H 11.1 L 10.2 C 10.7, 2026-09-03 H 11.0 L 10.1 C 10.6, 2026-09-04 H 11.3 L 10.4 C 10.9, 2026-09-05 H 11.7 L 10.8 C 11.3, 2026-09-06 H 11.5 L 10.6 C 11.1, 2026-09-07 H 12.0 L 11.1 C 11.6, 2026-09-08 H 11.8 L 10.9 C 11.4, 2026-09-09 H 12.2 L 11.3 C 11.8, 2026-09-10 H 12.5 L 11.6 C 12.1, 2026-09-11 H 12.3 L 11.4 C 11.9, 2026-09-12 H 12.8 L 11.9 C 12.4, 2026-09-13 H 12.6 L 11.7 C 12.2, 2026-09-14 H 13.0 L 12.1 C 12.6, 2026-09-15 H 12.7 L 11.8 C 12.3, 2026-09-16 H 13.2 L 12.3 C 12.8, 2026-09-17 H 13.5 L 12.6 C 13.1, 2026-09-18 H 13.3 L 12.4 C 12.9, 2026-09-19 H 13.8 L 12.9 C 13.4, 2026-09-20 H 13.6 L 12.7 C 13.2." calls `compute_indicators`.
- "Here are 12 daily candles with volume: 2026-09-01 H 10.6 L 9.7 C 10.2 V 1200, 2026-09-02 H 11.1 L 10.2 C 10.7 V 1350, 2026-09-03 H 11.0 L 10.1 C 10.6 V 980, 2026-09-04 H 11.3 L 10.4 C 10.9 V 1500, 2026-09-05 H 11.7 L 10.8 C 11.3 V 1720, 2026-09-06 H 11.5 L 10.6 C 11.1 V 1100, 2026-09-07 H 12.0 L 11.1 C 11.6 V 1640, 2026-09-08 H 11.8 L 10.9 C 11.4 V 1280, 2026-09-09 H 12.2 L 11.3 C 11.8 V 1900, 2026-09-10 H 12.5 L 11.6 C 12.1 V 2050, 2026-09-11 H 12.3 L 11.4 C 11.9 V 1450, 2026-09-12 H 12.8 L 11.9 C 12.4 V 2200. Calculate the VWAP and a 10-period SMA." calls `compute_indicators`.

## When not to use

- "Forecast the EUR/USD close for next Friday.": The plugin reports numbers and gives no price predictions or recommendations.
- "Backtest a moving average crossover strategy on Bitcoin over the last five years.": The plugin calculates indicator values only; it has no backtesting, order or trading features.
- "What is the RSI of Tesla stock right now?": get_indicators covers only the ECB's currency pairs; there is no stock or crypto price source.

## Tools

- `compute_indicators`: Calculate indicators from your own candles. Calculates indicators from prices the user gives.
- `get_indicators`: Calculate indicators for a currency pair (ECB). Calculates indicators for a currency pair.

## How to answer

Use these tools to calculate technical indicators. compute_indicators works on candles the user provides (oldest first, a numeric close and optional time, open, high, low and volume) and makes no outside lookup; get_indicators reads ECB daily euro reference rates for a currency pair and supports sma, ema, rsi, macd and bollinger. Use compute_indicators for anything that is not a currency pair, and for atr, stochastic and vwap, which need high, low and volume. If the user wants an indicator for a stock or crypto asset, ask them to provide the prices. Report the numbers, the settings used (params), the source and the asOf date. Read skipped and error.hint when something could not be calculated and change the call instead of repeating it. This is informational only and not financial advice. Never say an indicator means buy, sell, overbought, oversold, bullish or bearish for the user, never predict prices and never suggest trades. If asked whether to buy or sell, say the plugin only calculates values. Do not claim the ECB publishes these indicators: it publishes only the rates.
If no tool here fits the task, say so; `index_tools` on the Agent Tools API server (https://api.openkrill.app/mcp) finds another server by task.

## Limits

No sign-up and no API key. Calls are limited per network and per day; a limit comes back as a tool error, not an answer, so wait and try again. Read-only public data. Privacy: https://tools.openkrill.app/privacy. Support: https://tools.openkrill.app/support.
