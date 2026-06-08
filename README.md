# Auto Anchored VP - True TPO Logic

A TradingView Pine Script v6 indicator that replicates TradingView's native Anchored Volume Profile as closely as possible within the constraints of Pine Script. Published for educational purposes - both as a useful tool and as a reference for understanding why accurate volume profile calculation is difficult to achieve in Pine Script.

## The Problem - Why Pine Script Cannot Build a True AVP

TradingView's native Anchored Volume Profile is calculated on the backend with access to tick-by-tick trade data. Every individual transaction is placed at its exact price, producing a true tick-level volume distribution. This is the gold standard - the most accurate possible representation of where volume actually traded.

Pine Script does not have access to tick data. It only sees OHLCV bars - one open, high, low, close, and volume value per candle. There is no way to know, within Pine Script, how much of a bar's volume traded at any specific price within the bar's range. This is a hard platform limitation, not a solvable coding problem.

As a result, every Pine Script volume profile implementation is an approximation. The question is not whether it is perfect - it cannot be - but how close it can get.

## The Approach - Even TPO Distribution

This indicator takes the most statistically neutral approach available in Pine Script: each bar's volume is distributed evenly across every price bin it spans based on its high-low range.

If a bar trades from 75,000 to 77,000 and covers 10 bins, each bin receives one tenth of that bar's volume. This makes no assumptions about where within the bar the volume concentrated - it treats every price the bar visited as equally likely. Across hundreds of bars, this averaging effect produces a profile that closely approximates the true tick-level distribution.

## The Dual-Bin Value Area Algorithm

The Value Area expansion uses the standard dual-bin comparison - at each step, the combined volume of the next 2 bins above is compared against the next 2 bins below, and the Value Area expands toward whichever side has more volume. This matches the original Steidlmayer methodology used in professional TPO profiles.

## How to Read the Levels

| Level | Description |
|---|---|
| POC | Point of Control. The price bin with the highest volume for the previous week. The market's fairest price - price gravitates back toward it when it deviates. |
| VAH | Value Area High. Upper boundary of the 70% volume zone. Acts as support when price is above it, resistance when price breaks below and retests from underneath. |
| VAL | Value Area Low. Lower boundary of the 70% volume zone. Acts as resistance on rallies from below, support when price holds above it. |

## Why Previous Week?

The indicator anchors to the complete prior weekly range - a fully closed, committed profile. The current week's profile is incomplete until Friday close. Reacting to a closed profile means every level has been fully tested and accepted by the market.

## Settings

| Setting | Description | Default |
|---|---|---|
| Number of Rows | Bin resolution. Higher = more precision, more computation. | 200 |
| Value Area % | Percentage of total volume the Value Area must contain. | 70% |
| POC Color | Color for the Point of Control line. | Yellow-green |
| VAH/VAL Color | Color for the Value Area High and Low lines. | Purple |

## How to use

Add the published indicator directly from TradingView, no copy-paste needed:

1. Open it here: [Auto Anchored VP: True TPO Logic](https://www.tradingview.com/script/2trJlI1x-Auto-Anchored-VP-True-TPO-Logic/)
2. Click the star to add it to your favorites, then add it to any chart from your indicators list.

The full source (`AutoAnchoredVP.pine`) lives in this repo under the MPL 2.0 license if you want to read or fork it.

## License

Mozilla Public License 2.0
