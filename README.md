# AI Trading System — Scanner · Engine · Backtester · Live Paper Trading

A full-stack, AI-assisted trading platform that turns a two-year discretionary supply-and-demand methodology into an **end-to-end automated system**: it screens the market, detects supply/demand structure with a human-calibrated engine, backtests every setup across markets and years, and paper-trades the survivors live against a real brokerage API — with a validation pipeline built to prove an edge before a dollar is ever risked.

> Built solo through spec-driven, AI-assisted development (Anthropic Claude Code) — **407 commits in ~7 weeks, 360+ production deployments.** Research tooling, not financial advice.

---

## The numbers

| | |
|---|---|
| **122,000+** | backtested trades (stocks + futures) in the latest campaign — **~320,000** simulated across all runs |
| **60,000+** | additional **out-of-sample** trades on an unseen year — held through a full regime shift |
| **63** | exit models simulated per trade, with full context recording |
| **3,000+** | expert chart corrections training the detection engine (human-in-the-loop calibration) |
| **8** | CME futures markets modeled (ES, NQ, YM, RTY, CL, GC, NG, SI) plus a broad equity universe |
| **20+** | live paper-trading books across equities and futures, real-fill accounting |

The point of these isn't the size — it's the **discipline**: rulesets are only promoted to live trading if they hold up *out-of-sample on data they were never fitted to.*

---

## Architecture

```
                     ┌─────────────────────────────────────────────┐
                     │            MARKET DATA PIPELINE             │
                     │   Alpaca · Polygon · Databento · TradingView │
                     └───────────────────────┬─────────────────────┘
                                             │
        ┌────────────────────────────────────┼────────────────────────────────────┐
        ▼                                    ▼                                    ▼
┌──────────────────┐          ┌───────────────────────────┐          ┌──────────────────────┐
│   AI SCREENER    │          │    BACKTESTING ENGINE     │          │  LIVE PAPER RUNNERS  │
│                  │          │                           │          │                      │
│ 20,000 tickers   │          │ 120k+ trades · 63 exit    │          │ Stocks (cloud) +     │
│  → FINVIZ / TV   │          │   models · full context   │          │  futures (local)     │
│    filters       │          │ out-of-sample promotion   │          │ 20+ books · risk     │
│  → Claude API +  │          │ EV · profit factor · win% │          │  ladder · real fills │
│    web search    │          │ "freeze" winning rulesets │          │ shadow-track capacity│
│  → scored list   │          │                           │          │ Alpaca API execution │
└────────┬─────────┘          └─────────────┬─────────────┘          └──────────┬───────────┘
         │                                  │                                   │
         └──────────────────────────────────┼───────────────────────────────────┘
                                            ▼
                          ┌───────────────────────────────────┐
                          │        FLASK WEB DASHBOARD        │
                          │  Command Center · Backtest Lab ·  │
                          │  Paper Trading · Stats Explorer · │
                          │  Markup Editor · Rulebook         │
                          └───────────────────────────────────┘
```

---

## Screenshots

| Command Center — live market scan | Markup Editor — the engine trainer |
|---|---|
| ![Command Center](command-center.png) | ![Markup Editor](markup-editor.png) |

---

## How it works

**1 · AI screening.** A pipeline pulls a 20,000-ticker universe through FINVIZ and TradingView filters (trend, relative strength, volume, durability), then hands the shortlist to an AI analysis layer (Claude API + web search) that reads each candidate for durability and catalyst quality and returns a scored verdict. A forward-return feedback loop grades past picks and uses leave-one-out analysis to reveal which filters actually add edge.

**2 · The engine.** A human-calibrated detector encodes an institutional supply-and-demand methodology — zone/structure detection, multi-timeframe nested trend state, and geometric tiering — into deterministic, testable rules with defined entries, stops, and targets. A markup editor lets me correct the engine against hand-drawn ground truth, and every correction feeds back as calibration.

**3 · Backtesting & validation.** Every ruleset is replayed across 120,000+ historical trades with 63+ exit models and full context recording (structure depth, timeframe position, session, trend state). Rules are promoted to live use only when they hold up **out-of-sample on never-tested tickers and unseen market regimes** — measured by expected value, profit factor, and win rate.

**4 · Live paper trading.** Cloud and local runners watch live watchlists intraday, size positions from a configurable risk-and-scaling ladder, place paper orders through the Alpaca API across equities and futures, and shadow-track every signal they can't fill to measure true strategy capacity — all with real-fill accounting and journaled on the dashboard.

**5 · Web dashboard.** A Flask front end ties it together: a Command Center, a Backtest Lab (league tables, top picks, a prop-firm rule simulator), a live paper-trading page, a stats explorer, and the markup/calibration editor.

---

## Tech stack

| Layer | Tools |
|---|---|
| **Language / Web** | Python, Flask, HTML / CSS / JavaScript, SQLite |
| **Market data** | Alpaca, Polygon, Databento (1-min CME futures), TradingView, FINVIZ |
| **AI** | Anthropic Claude API (+ web search) for analysis; built with Claude Code |
| **Infra** | GitHub Actions (CI/CD), Railway (cloud deploy), automated tests, process watchdogs |

---

## How it was built

This system was built **solo through AI-assisted, spec-driven development** with Anthropic Claude Code. Rather than hand-writing every line, I authored detailed strategy and architecture specifications that encoded my trading methodology and system design, then directed AI coding agents to implement, test, and iterate — running the calibration loops, backtesting campaigns, and deployments myself across 400+ commits. Every design decision, trading rule, and validation gate is mine; the AI was the implementation engine.

It's a deliberate way of working: it let me ship a production-grade quantitative system — data pipeline, backtester, live execution, and web app — far faster than writing it by hand, and it's how I build.

---

## Note

The full source and the specific trading rules are kept private to protect the methodology. This README documents the system's scope, engineering, and validation approach. *Research tooling only — not financial advice; verify every number before risking capital.*
