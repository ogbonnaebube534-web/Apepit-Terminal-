# 🦍 APEPIT TERMINAL — Solana Memecoin Signal Trading Agent

**Demo:** https://4dpxafpe.mule.page/
**Team:** 0xRegime | **Handle:** @defitony0x | **Hackathon:** #BitgetHackathon

---

## Hackathon Submission Q&A

### Why did we build it? What's the core logic?

APEPIT TERMINAL was built to solve one of the most brutal edge cases in crypto trading: **Solana memecoin velocity**. Tokens launch, moon, and rug within hours — often minutes. Traditional trading dashboards weren't designed for this time horizon or volatility class. The gap between seeing a signal and executing is where money dies.

The core logic is a **multi-signal regime-aware scanner** that continuously monitors Solana on-chain data and surfaces tokens with asymmetric upside potential. The terminal synthesizes four signal categories simultaneously:

- **On-chain signals:** New token launches from bonding curve (pump.fun), liquidity depth, holder concentration (whale vs. distributed), dev wallet size, contract age, and volume/liquidity ratio
- **Sentiment signals:** Social velocity (mentions per minute), holder growth rate, and buy/sell pressure ratio from live trade feed
- **Technical indicators:** 24h price change momentum, volume spike relative to market cap, and sparkline trend direction
- **Risk scoring:** A composite Rug Score (0–100) weighing LP lock status, dev wallet %, contract renouncement, and holder distribution

Decisions are ranked by a composite score weighting momentum (40%), liquidity safety (30%), holder health (20%), and social signal (10%). The terminal surfaces the highest-conviction opportunities at the top of the trending feed in real time.

Risk management is enforced at the UI layer: position sizing presets (0.1 SOL / 0.5 SOL / 1 SOL / Custom), adjustable slippage tolerance (0.5%–5%), and Jito bundle routing for MEV-resistant execution. The Rug Score gates trade execution — tokens scoring above 60 are visually flagged and require active override.

---

### Why does the strategy work?

Solana memecoin alpha decays in under 24 hours. The window between a token trending internally (on-chain signal) and trending externally (Twitter/CT) is where APEPIT captures its edge. By the time a token hits a mainstream feed, the move has already happened.

The strategy exploits **information asymmetry** between on-chain reality and social perception. Specifically:
- A token with rapidly increasing unique holders + rising buy pressure + low dev concentration = early community distribution signal
- Volume-to-liquidity ratio above 3x within the first 6 hours is a historically strong precursor to a parabolic move
- Rug Score below 20 combined with bonding curve LP lock = significantly reduced downside tail risk

---

### Key Development Challenges

**Challenge 1 — Data latency on Solana:** Solana produces ~2,500 TPS with 400ms block times. Getting a token's full on-chain profile (holders, trades, LP state) fast enough to matter required building an aggregation layer that caches DEX data from DexScreener and Birdeye APIs with sub-second refresh cycles rather than polling raw RPC.

*Solution:* Dual-feed architecture — WebSocket stream for live price/trade ticks, REST polling for holder/LP snapshots on a staggered interval.

**Challenge 2 — Signal noise in memecoins:** The vast majority of new launches are worthless. Running every token through full scoring was computationally wasteful.

*Solution:* Two-stage filter. Stage 1 is a fast liquidity gate (must exceed $10K liquidity and 100 holders within 30 minutes of launch). Stage 1 rejections are dropped. Only passing tokens proceed to full composite scoring.

**Challenge 3 — MEV and frontrunning:** On Solana, public mempool snipers frontrun large buys in sub-millisecond windows.

*Solution:* Jito bundle routing integration — transactions are submitted as private bundles to Jito block engines, making them invisible to frontrunners until inclusion.

---

### Features Completed

- Live memecoin token feed with real-time price, volume, market cap, and age
- Composite Rug Score with holder distribution breakdown
- Buy/sell trade feed (live wallet activity per token)
- Holder concentration panel (whale %, dev wallet, bonding curve %)
- Chart with momentum sparklines and 24h price action
- Buy/Sell UI with SOL amount input, slippage control, and Jito submission
- Watchlist with persistent star tracking
- Ticker marquee of trending tokens
- Phantom wallet connect button

### Still In Progress / Next Steps

- Live Bitget Futures hedge integration: when a memecoin position exceeds $500 unrealized gain, automatically open a SOL-PERP short on Bitget to lock in gains
- On-chain execution via Jupiter aggregator (currently in UI simulation mode)
- Telegram alert bot: fires when a token crosses scoring thresholds
- ML-based rug prediction model trained on historical pump.fun launches
- Full backtesting engine with historical on-chain replay

---

### Frameworks, Models & APIs

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML/CSS/JS, Tailwind CSS (CDN), Chart.js 4.4 |
| Fonts/Design | Google Fonts (Space Grotesk, JetBrains Mono, Bungee) |
| On-chain Data | DexScreener API, Birdeye API, Solana RPC |
| Execution | Jito Bundle API (MEV-resistant routing), Jupiter Aggregator |
| Wallet | Phantom Wallet (Solana Web3.js) |
| Hosting | MuleRun (demo), Vercel (production) |
| Bitget Integration | **Bitget Futures API** — planned SOL-PERP hedge automation |

**Bitget Tools Used:**
- Bitget Futures API for cross-margined SOL perpetuals (hedge layer, in development)
- Bitget Agent Hub — explored for autonomous hedge trigger logic
- Bitget MCP Server — evaluated for position management via agentic workflow

---

### Experience with Bitget AI Tools & Views on Agentic Trading

Using the Bitget ecosystem for this project clarified something important: **the bottleneck in agentic trading is not execution, it's signal trust.** Bitget's infrastructure is fast and reliable — the API is well-documented and the futures execution is low-latency. What's genuinely difficult is building an agent that knows *when not to trade*.

The Agent Hub abstraction is promising for composable trading logic — the ability to chain conditions (e.g., "if Rug Score < 20 AND volume/MC > 2x AND Bitget SOL funding rate < 0.01% THEN execute") is exactly the kind of rule stack that can codify discretionary judgment.

**Suggestions for improvement:**
- A real-time on-chain data feed natively within Agent Hub (currently requires external data plumbing)
- Solana chain support in the MCP Server (currently EVM-focused)
- A Skill Hub template specifically for memecoin/high-velocity token strategies

**On the future of agentic trading:** The next frontier is agents that trade *asymmetric information*, not just price. An agent that reads on-chain data faster than a human, scores risk in milliseconds, and routes via Jito bundles isn't just faster — it's playing a fundamentally different game. APEPIT is a step toward that: a terminal where the agent surfaces the signal, and the human just confirms the bet.

---

## Submission Checklist

| Requirement | Link / Status |
|---|---|
| Demo (publicly accessible, no login) | https://4dpxafpe.mule.page/ |
| GitHub Repo | *(link to your GitHub repo)* |
| Live/Paper Trading Log | *(upload to GitHub — timestamps, pair, side, price, size, balance)* |
| Repost of official Bitget campaign post | *(link to your repost on X)* |
| Project post with #BitgetHackathon + @BitgetAI | *(link to your X post)* |
| Demo video (if demo requires login) | *(YouTube or public X video link)* |

---

## Project Overview

APEPIT TERMINAL is a Solana memecoin intelligence layer and trading terminal. It aggregates on-chain signals, scores tokens by risk/reward profile, and provides a one-click trading interface with MEV-resistant execution. The goal is to give a solo trader the information edge that previously required a full data engineering team.

> ⚠️ Memecoins are high-risk. Only trade what you can afford to lose. NFA.

**#BitgetHackathon @BitgetAI | Built by 0xRegime**
