# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repo contains TradingView Pine Script v5 indicators. Scripts are deployed by pasting directly into the TradingView Pine Script editor — there is no build step, package manager, or test runner.

## Language & Platform

- **Pine Script v5** (`//@version=5`) — TradingView's proprietary scripting language
- Scripts run entirely inside TradingView; they cannot be executed locally
- To test changes: paste the script into the TradingView Pine Editor and click "Add to chart"

## Current Script: `IT_Options`

`IT_Options` is an intraday options signal indicator (`indicator(..., overlay=true)`). It generates CALL and PUT entry signals with ATR-based SL/TP levels.

### Signal logic (all conditions must be true)

**CALL:** price above EMA + HTF bullish + volume spike + ATR expanding + RSI in bull band + strong bullish candle body + in session + cooldown passed + daily cap not hit

**PUT:** price below EMA + HTF bearish + volume spike + ATR flat-or-rising + RSI in bear band + strong bearish candle body + in session + cooldown passed + daily cap not hit

### Key design decisions

- **HTF filter uses `lookahead_on`** — intentional for real-time alerting. This causes future-leak; do not use this script for backtesting.
- **Asymmetric volatility filter** — CALLs require `atr > atr_sma` (expanding); PUTs only require `atr >= atr_sma * 0.85` (flat-or-rising). Bear moves are often slow grinds.
- **RSI bands are mutually exclusive** — CALL floor (52) is above PUT ceiling (48) with an explicit gap plus a `rsi_conflict` guard.
- **Alert messages use named `plot()` values** — Pine `alertcondition` messages must be const strings; dynamic values (ATR, RSI, SL, TP) are injected by TradingView via `{{plot("Name")}}` at alert-fire time.
- **SL/TP anchored to `close`** — best approximation in a non-strategy indicator context; replace with a confirmed entry price variable for live use.
