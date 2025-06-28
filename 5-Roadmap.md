---
icon: bars-progress
---

# Roadmap

{% hint style="warning" %}
Roadmap as of 6/27/2025 - Roadmap may change based on company direction/priorities
{% endhint %}

***

### Phase 0 — Foundation & R-and-D (Completed)

| Window                  | Key Goals                                                                       | Major Outputs                                              | Status     |
| ----------------------- | ------------------------------------------------------------------------------- | ---------------------------------------------------------- | ---------- |
| **Dec 2024 – Jan 2025** | - Choose LLM stack- Spin up on-chain indexer                                    | • **VGPT Backend v0.1** skeleton• Repo & CI/CD scaffolding | ✅ Complete |
| **Feb 2025**            | - Prototype data pipes (Etherscan, GoPlus, Dex-Screener)- First inference tests | • **Backend v0.3** (scan < 2 s)• Internal Postman suite    | ✅ Complete |
| **Mar 2025**            | - Build web chatbot with on-chain actions- Harden infra                         | • **Chatbot α** PWA (swap/approve demo)• Load + fuzz tests | ✅ Complete |
| **Apr 2025**            | - Heuristic tuning round-1- Closed beta (100 users)                             | • Accuracy dashboard (92 % target)• Feedback loop to VGPT  | ✅ Complete |

***

### Phase 1 — Token Launch & Core Freeze

| Window       | Key Goals                                             | Major Outputs                                                        | Status     |
| ------------ | ----------------------------------------------------- | -------------------------------------------------------------------- | ---------- |
| **May 2025** | - Ship production backend 0.9- Launch ecosystem token | • **VECTOR TGE** (5 %/5 % tax)• Liquidity seeded, contract renounced | ✅ Complete |

***

### Phase 2 — First-Gen Tools

| Window             | Key Goals                             | Public Assets                                       | Status |
| ------------------ | ------------------------------------- | --------------------------------------------------- | ------ |
| **Early Jun 2025** | - Real-time charting bot              | **Vector Scope Bot** (TG)                           | ✅ Live |
| **Mid Jun 2025**   | - Surface on-chain heuristics in chat | **Vector IQ Analyzer** (free Quick-Scan)            | ✅ Live |
| **Wk 3 Jun 2025**  | - Launch premium deep scan            | **Vector IQ⁺** (holder clusters, social validation) | ✅ Live |

***

### Phase 3 — Scanner & Signals Roll-out _(July 2025)_

| Sprint   | Internal Milestone                       | Public Release                                   | Metrics Gate                  |
| -------- | ---------------------------------------- | ------------------------------------------------ | ----------------------------- |
| **Wk 1** | Code-freeze 👉 QA/Sec review             | **Token Scanner GA**                             | Error ≤ 1 %, latency < 300 ms |
| **Wk 2** | Messier Analytics demo                   | —                                                | —                             |
| **Wk 3** | Signals accuracy review                  | **Smart Token Signals (Live)**                   | ≥ 92 % precision              |
| **Wk 4** | Manual Sniper + Trending internal review | **Vector Manual Sniper + Scope Trending (Live)** | Fill latency < 1 block        |
| **Wk 5** | Auto-Sniper closed beta                  | **Vector Auto-Sniper (Beta ▶ Live)**             | PnL win-rate ≥ 70 %           |

***

### Phase 4 — Mobile Expansion _(August 2025)_

| Week     | Deliverable                               | Scope                                           |
| -------- | ----------------------------------------- | ----------------------------------------------- |
| **Wk 1** | **Vector Mobile App (PWA) – Public Beta** | Quick-Scan, Signals, Manual Sniper, push alerts |
| **Wk 2** | Sprint retro & next-phase planning        | Roadmap refresh for Q4                          |

***

### Phase 5 — Flywheel & Monetisation

> **More scans → smarter VGPT → better signals → higher sniper PnL → more paid subs → more data.**\
> Revenue flows (product fees + 5 %/5 % swap-back taxes) funnel into R-and-D, liquidity adds, and buy-backs — reinforcing growth.

***

### Phase 6 — Horizon (Sep 2025 →)

| Target                  | Concept (draft)                                           |
| ----------------------- | --------------------------------------------------------- |
| **Copy-Trader Vaults**  | Auto-mirror top wallets with risk caps & fee share        |
| **DEX Perps Scanner**   | Extend heuristics to perpetual pairs & on-chain perp DEXs |
| **LP Arbitrage Engine** | Cross-chain liquidity gap sniper using Viper cores        |
| **Partner API Suite**   | Tiered, pay-per-call access to VGPT Deep-Scan             |

***

#### 📌 Status Recap (July 2025)

| Complete                                              | In-Flight                       | Next-Up                                |
| ----------------------------------------------------- | ------------------------------- | -------------------------------------- |
| VGPT backend, Chatbot, VECTOR TGE, Scope Bot, IQ, IQ⁺ | Scanner GA QA, Signals approval | Manual Sniper, Auto-Sniper, Mobile PWA |

**TL;DR** – We shipped the foundation (Dec → Jun), are rolling out Scanner & Sniper layers through July, and go mobile in August — all feeding data back into VGPT for compounding accuracy and revenue.
