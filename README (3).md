# 🎯 BASE BREAKOUT & ACCUMULATION SCANNER — V9

Goal: fundamentally strong NSE stocks that have **consolidated for 6-24 months** and are now **breaking out (or about
to) with volume, strength and momentum** — then a full trade plan (entry, stop, targets, quantity).

## Files (all 4 in the GitHub repo root)
`app.py` · `engine.py` · `requirements.txt` · `README.md`
Upload to your repo (same names replace the old ones), then **Reboot** the Streamlit app.

## How to use (simple mode, default)
1. Press **🚀 RUN FULL SCAN** after market close (~4 PM IST). Data = free end-of-day Yahoo data; the screen shows the
   "price data as of" date.
2. **🎯 Top trades** = breakouts that passed every gate + fundamentals. Each card shows the base (months, width,
   ceiling), momentum (RSI/MFI/CCI), fundamentals score, Entry / Stop / Targets / Qty.
3. **⏳ Coiled (ready)** = stocks still under the base ceiling (within 5%) — set alerts at the trigger, buy only after a
   close above it on volume.
4. **👀 Watchlist** shows why stocks were rejected, incl. "good chart but weak fundamentals" and "fundamental data missing".

## What counts as a base (engine.py → `_base_setup`)
For windows of ~6, 9, 12, 18 and 24 months (ending yesterday) ALL of these must hold; the longest valid window is reported:
* **Width:** high-to-low band no wider than "Max base width" (40% for a 12-month base; slightly more room for longer ones).
* **Flat, not a trend:** net price change over the window ≤ half the band width, and the ceiling was already reached in
  the OLDER half of the window (so a stock that just keeps making new highs is NOT a base).
* **Coiling high:** the previous close sat in the upper half of the band.
* **CO (breakout):** today's close is the first close above the ceiling, within 6% of it (not extended). One signal per breakout.
* **Coiled:** below the ceiling but within 5% of it.

## Breakout gates (all must pass)
Volume ≥ 2× (slider) · strong bullish candle · trend (price > EMA20/50/200, EMA200 not falling) · momentum
(RSI 55-82, MFI ≥ 55, CCI ≥ 60) · liquidity (turnover) · market cap ≥ ₹1,000 Cr · price ≥ ₹20 · not a frozen/circuit
bar · stop ≤ 8% away · reward:risk room ≥ 2.5R to the next overhead swing high (24-month look-back) · score ≥ 70 ·
market regime / sector strength (top-down, as in V8) · **fundamental score ≥ 60**.

## Fundamental score (0-100, transparent)
ROE (20) · debt/equity (15, skipped for banks/NBFCs) · EPS growth (20) · sales growth (15) · profit margin (10) ·
current ratio (5, not for banks) · PE sanity (10) · insider holding (5, rough promoter proxy). Only data that exists is
scored; if less than 60% of the attainable points can be measured the result is **NA** (goes to a "check manually" list,
never silently passed). Labels: STRONG ≥ 75 · GOOD ≥ 60 · AVERAGE ≥ 45 · WEAK.

## Entry / Stop / Targets
* **Entry:** buy-stop just above the signal-day high (other styles selectable).
* **Stop:** the tighter of "below the last 3 lows" and "3% under the base ceiling" (a failed breakout = back inside the
  base), capped by Max stop distance.
* **Targets:** T1 = 2R, T2 = 3R, plus the classic **base-height target** (ceiling + base height). Sell half at T1, move
  the stop to entry, trail the rest below EMA20.
* **Quantity:** from your capital and risk %, capped so one stock never exceeds your max % of capital.

## Honest limits
* **Fundamentals come from Yahoo and are patchy for Indian small caps** and are *latest* values, not point-in-time —
  so the Backtest tab does NOT replay fundamentals, market cap, sector or regime rules. It tests the chart rules only.
* The base definition is mine (a reasonable, testable one), checked on synthetic data (clean bases detected, steady
  uptrends and random walks mostly ignored) — NOT yet validated on real NSE history. Run the Backtest tab on 5y data and
  read the "By setup" table for the Base Breakout row before trusting it. Score ≠ probability.
* Survivorship bias (today's index members), free data, no slippage beyond the cost setting.
* No live/intraday feed. Verify price, results dates and corporate actions on NSE / your broker before trading.
* Educational tool, not investment advice.
