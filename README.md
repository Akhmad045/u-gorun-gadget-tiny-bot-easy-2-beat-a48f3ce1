# Tiny Bot, Easy $2: Beat Buy-and-Hold on Solana

[![make.fun](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![rank](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/Akhmad045/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/rank.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![reward](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/Akhmad045/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/reward.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)

Entry for the [Tiny Bot, Easy $2: Beat Buy-and-Hold on Solana](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1) bounty on [make.fun](https://make.fun).

`main.py` is a paper-trading bot for the `agent-fun/bars-replay/v1` harness. It trades seven Solana assets (SOL, WETH, RAY, WIF, POPCAT, FARTCOIN, JITOSOL) against USDC on hourly bars, starting from 10,000 simulated USDC. Nothing is traded for real.

## Strategy

Two sleeves, both long-only, deterministic, standard library only:

- **Breakout momentum** — every 6 bars, rank the universe by trailing 12-bar return. When at least 4 of 7 assets are up, buy the top 4 names that are also within 1% of their running window high (a breakout, not a bounce), up to 25% of equity each, built in three tranches (entry bar, +6 bars, +12 bars). Exits: 12% trailing stop, 10% stop below cost, 30% take-profit, with a 24-bar cooldown after a losing exit.
- **Oversold scalp** — buy 12.5% of equity when RSI(14) < 30 and the close is under the lower Bollinger band (20 closes, 2σ). Exit when RSI > 55, or +8% take-profit, or −4% stop.

Why it beats buy-and-hold: in a falling window the momentum sleeve never fires (no new window highs) and the scalp sleeve banks small mean-reversion wins, so the bot stays near cash while buy-and-hold eats the drawdown; in a rising window the momentum sleeve holds the strongest names at full weight and rides them, while the tranches keep the trade count above the 8-trade guardrail even in a slow, steady rise.

## Dev-window results (kit's `grade-local`, process driver)

| Entry | Net PnL (USDC) | Trades | Max drawdown |
|---|---|---|---|
| This bot | **+50.35** | 20 | 3.31% |
| buy-and-hold | −869.34 | 7 | 10.92% |
| macd-cci | −20.22 | 17 | 2.46% |
| rsi-bollinger | −56.49 | 8 | 5.16% |
| adx-momentum | −114.42 | 11 | 1.38% |
| ema-cross | −173.04 | 28 | 2.51% |
| momentum | −268.23 | 19 | 5.66% |
| hold-cash | 0.00 | 0 | 0.00% |

The bot also beats buy-and-hold on a time-reversed (rising) copy of the dev window (+1,014 vs +904) and on both halves of the dev window. The bot's own number is the kit's official `grade-local` result, verified twice with identical `result_sha256` (`d5a1af6ba6ae2c44d7194b6945e63f65533939fedc71b021fbe060e43606611a`); baseline numbers are from the same tool.

Referral code: `4g7knchf`
