<div align="center">
<br>

# 🦀 FinClaw

**Stop writing trading strategies. Evolve them.**

A genetic algorithm engine that breeds and walk-forward validates trading strategies across 484+ market factors. No manual rules, no PhD required.

<p align="center">
  <img src="https://img.shields.io/badge/evolvable_factors-484+-orange" alt="484+ Evolvable Factors">
  <img src="https://img.shields.io/badge/markets-US_Stocks_%7C_Crypto-blue" alt="Markets">
  <img src="https://img.shields.io/badge/validation-Walk--Forward_%7C_Monte_Carlo-green" alt="Validation">
  <a href="https://discord.gg/kAQD7Cj8"><img src="https://img.shields.io/discord/1488800950696284272?color=7289da&label=Discord&logo=discord&logoColor=white" alt="Discord"></a>
</p>

<p>
  <a href="#how-it-works">How It Works</a> ·
  <a href="#live-evolution-results">Results</a> ·
  <a href="#anti-overfitting">Robustness</a> ·
  <a href="#484-factors">Factors</a> ·
  <a href="#get-access">Get Access</a> ·
  <a href="https://discord.gg/kAQD7Cj8">Discord</a>
</p>

</div>

---

## What Is FinClaw?

Most quant tools make you **write** the strategy. You pick indicators, set thresholds, run a backtest, tweak, repeat.

FinClaw takes the opposite approach: it **evolves** strategies. A genetic algorithm searches across 484+ factors and finds weight combinations that actually work out-of-sample.

```
You write the rules        →  FinClaw discovers the rules
You tune parameters        →  GA tunes parameters  
You test on one period     →  Walk-forward tests on multiple windows
You hope it generalizes    →  Monte Carlo measures if it does
```

You don't pick the indicators. The algorithm does. You decide how long to let it search.

---

## How It Works

```
  Random DNA population (484 factor weights + risk parameters)
       │
       ▼
  ┌──────────────────────┐
  │  Walk-Forward Test   │  Multi-window out-of-sample validation
  │  each DNA candidate  │  Real fees, slippage, position caps
  └──────────┬───────────┘
             │
             ▼
  Keep the survivors (Sharpe × Return / MaxDD)
             │
             ▼
  Mutate + Crossover → next generation
             │
             ▼
  Repeat for N generations
```

Each DNA is a weight vector across 484+ factors **plus** risk/position parameters — all evolvable:

| Parameter | Range | What it controls |
|-----------|-------|-----------------|
| Factor weights (×484) | 0.0–1.0 | Which factors matter and how much |
| `hold_days` | 2–60 | Day trades through swing trades |
| `trailing_stop` | % | Lock in profits, trail below peak |
| `market_regime` | sensitivity | Auto-reduce exposure in bear markets |
| `kelly_fraction` | 0–1 | Position sizing based on recent win rate |

The GA tries combinations you wouldn't. That's the point.

---

## Live Evolution Results

> **Actual numbers** from our running evolution engines. Not cherry-picked. Updated regularly.

### 🇺🇸 US Stocks (100 S&P 500 stocks — Gen 94)

| Metric | Best DNA |
|:------:|:--------:|
| Annual Return | **33.5%** |
| Sharpe Ratio | **1.47** |
| Max Drawdown | 17.0% |
| Win Rate | 55.5% |
| Profit Factor | 1.75 |
| Total Trades | 179 |

### ₿ Crypto (17 assets — Gen 44)

| Metric | Best DNA |
|:------:|:--------:|
| Annual Return | **77.2%** |
| Sharpe Ratio | **3.25** |
| Max Drawdown | 15.7% |
| Win Rate | 52.3% |
| Profit Factor | 1.73 |
| Total Trades | 194 |

**Caveats:** These are backtests with walk-forward validation, not live trades. Real results will differ. We publish results window-by-window so you can judge for yourself — no hidden bad periods.

---

## Anti-Overfitting

We learned the hard way. An early version showed 25,000% returns — it was a bug (look-ahead bias). Here's what prevents that now:

| Defense | What it does |
|---------|-------------|
| **Walk-Forward** | Multi-window OOS validation. Must profit on data it never trained on. |
| **Monte Carlo** | 1,000 shuffled iterations. p-value < 0.05 or it's luck. |
| **CPCV** | Combinatorial Purged Cross-Validation — industry standard for backtest integrity. |
| **Arena Mode** | Multiple strategies compete simultaneously. Crowded signals get penalized. |
| **Correlation Guard** | Rejects picks correlated above 0.85. No fake diversification. |
| **Factor Diversity** | HHI-based penalty for over-relying on one factor group. |
| **Turnover Penalty** | Excessive trading gets punished in fitness. Real transaction costs modeled. |
| **Bias Detection** | Look-ahead, snooping, survivorship — flagged automatically. |

**Transparency note:** Our early versions had inflated returns from look-ahead bias. We fixed it, disclosed it publicly, and built automated bias detection so it can't happen again. Honest 33% > fake 25,000%.

---

## 484+ Factors

| Category | Count | Examples |
|----------|------:|---------|
| Crypto-Native | 200 | Funding rate, session effects, whale detection, liquidation cascade |
| Momentum | 14 | ROC, acceleration, trend strength |
| Volume & Flow | 13 | OBV, smart money, Wyckoff VSA |
| Volatility | 13 | ATR, Bollinger squeeze, vol-of-vol |
| Mean Reversion | 12 | Z-score, Keltner channel position |
| Trend Following | 14 | ADX, EMA golden cross, MA fan |
| Qlib Alpha158 | 11 | KMID, KSFT, CNTD (Microsoft Qlib compatible) |
| Risk Warning | 11 | Consecutive losses, death cross, gap-down |
| Quality Filter | 11 | Earnings momentum, relative strength |
| Price Structure | 10 | Candlestick patterns, support/resistance |
| Sentiment | 2 | EN/ZH keyword sentiment |
| DRL Signal | 2 | Q-learning buy probability |

All factor weights are discovered by evolution — no manual tuning required.

---

## Strategy Styles

When FinClaw evolves a strategy, the resulting DNA falls into recognizable styles based on which factors dominate:

| Style | What the Algorithm Found |
|-------|-------------|
| **Value Seeker** | Buys cheap, holds patient. Benjamin Graham would recognize this DNA. |
| **Momentum Rider** | Chases what's moving, cuts what isn't. Trends persist. |
| **Mean Reverter** | Bets on bounce-backs. Statistical arbitrage without the PhD. |
| **Flow Reader** | Follows the money. Volume precedes price — the DNA figured that out. |
| **Volatility Hunter** | Profits from volatility expansion. Sits during calm, strikes during storms. |
| **Crypto Native** | 200 factors built for a 24/7 market that doesn't follow TradFi rules. |

These aren't templates — they're what the algorithm actually converged on.

---

## Multi-DNA Ensemble

Instead of betting on a single evolved strategy, FinClaw can run **multiple DNAs** and aggregate their signals:

```
  Multi-DNA Analysis: AAPL
  ═══════════════════════════════════════
  
  Strategy Votes:
    Value Seeker       → BUY   (score: 0.72, confidence: high)
    Momentum Rider     → BUY   (score: 0.85, confidence: high)  
    Mean Reverter      → HOLD  (score: 0.45, confidence: medium)
    Flow Reader        → BUY   (score: 0.68, confidence: medium)
    Volatility Hunter  → HOLD  (score: 0.51, confidence: low)

  Consensus: BUY (3/5 bullish, 0/5 bearish)
```

Each strategy was independently evolved. Agreement = stronger signal.

---

## Supported Markets

| Market | Data Source | Coverage |
|--------|-----------|----------|
| 🇺🇸 US Stocks | Yahoo Finance | S&P 500+ |
| ₿ Crypto | ccxt (100+ exchanges) | BTC, ETH, SOL, + 14 more |
| 🇨🇳 A-Shares | AKShare + BaoStock | Full coverage |

---

## Get Access

FinClaw is available as a **Pro product** with paper trading, signal generation, and evolution-as-a-service.

**What's included:**
- Full evolution engine with 484+ factors
- Walk-forward + Monte Carlo + CPCV validation
- Paper trading with real exchange data
- Evolved DNA library (continuously updated)
- Live exchange connectors (OKX, Binance)
- Priority support via Discord

📧 **Contact:** [neuzhou@outlook.com](mailto:neuzhou@outlook.com)  
💬 **Discord:** [discord.gg/kAQD7Cj8](https://discord.gg/kAQD7Cj8)

---

## Technical Papers

- [GT-Score: A Generalizable Fitness Function for Walk-Forward Strategy Evolution](https://arxiv.org/abs/2602.00080) — Our approach to anti-overfitting in genetic strategy optimization.

---

## Roadmap

- [x] 484-factor evolution engine
- [x] Walk-forward + Monte Carlo + Arena mode
- [x] Multi-DNA ensemble voting
- [x] Strategy style classification  
- [x] Paper trading
- [x] Live exchange connectors
- [x] Market regime detection
- [ ] Public paper trading dashboard (live P&L tracking)
- [ ] Signal subscription service
- [ ] DEX execution (Uniswap V3)
- [ ] Multi-timeframe evolution

---

<div align="center">

**Built by [NeuZhou](https://github.com/NeuZhou)**

*The strategies evolve. So do we.*

</div>
