# TV Scripts

TradingView Pine Script v5 indicators for intraday options trading.

## Scripts

### IT_Options — Intraday Options Signal v2 [Enhanced]

An overlay indicator that generates CALL and PUT entry signals with ATR-based stop loss and take profit levels.

**How to use:**
1. Open TradingView → Pine Script Editor
2. Paste the contents of `IT_Options`
3. Click "Add to chart"

#### Signal Conditions

| Condition | CALL | PUT |
|---|---|---|
| Price vs EMA | Above | Below |
| HTF (1H) trend | Bullish | Bearish |
| Volume | Spike (> multiplier × avg) | Spike (> multiplier × avg) |
| Volatility | ATR expanding | ATR flat or rising |
| RSI | 52–72 | 28–58 |
| Candle body | Bullish & strong | Bearish & strong |

All conditions must be true simultaneously. Additional guards: session time gate, signal cooldown, and daily signal cap.

#### Settings

| Group | Key Inputs |
|---|---|
| Core Filters | Volume multiplier, EMA length, ATR period |
| Momentum & Candle Quality | RSI bands (CALL / PUT), min body ratio |
| Risk Management | SL, TP1, TP2 as ATR multiples |
| Signal Quality | Cooldown bars, HTF timeframe & buffer |
| Session Time Filter | Session open/close (exchange time) |
| Daily Risk Limits | Max signals per day |

#### ATR-Based Levels

- **SL:** 1.0× ATR from entry
- **TP1:** 1.5× ATR from entry
- **TP2:** 3.0× ATR from entry

Levels are anchored to the close price of the signal bar.

#### Alerts

Alert conditions are pre-configured for CALL and PUT signals. Messages are JSON-formatted for webhook/bot consumption:

```json
{"signal":"CALL","ticker":"...","tf":"...","price":...,"atr":...,"rsi":...,"sl":...,"tp1":...,"tp2":...}
```

> **Note:** The HTF trend filter uses `lookahead_on` for accurate real-time signals. Do not use this script for backtesting — it will produce unrealistically good historical results due to lookahead bias.
