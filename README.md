# Solana-Dexscreener-Bot

A Python-based crypto momentum scanner that discovers newly listed tokens from DexScreener, tracks liquidity and market activity, scores promising pools, and sends high-quality trading alerts directly to Telegram.

---

## 📌 Overview

This project discovers newly listed tokens across supported blockchains using the DexScreener API.

For every newly discovered token, the bot:
* Retrieves all available trading pairs
* Stores pool information in SQLite
* Records historical pool snapshots
* Calculates momentum metrics
* Scores each pool based on liquidity, price action, volume, FDV, market cap, and pool age
* Sends high-confidence alerts to Telegram
* Prevents duplicate alerts using persistent alert history

The project is designed as a foundation for an automated early token discovery system.

---

# Features

### 🔍 Automatic Token Discovery

* Fetches newly listed tokens from DexScreener
* Detects new tokens automatically
* No predefined watchlist required

---

### 📊 Pool Collection

For each token, the bot collects:

* Pair Address
* Token Address
* Chain
* DEX
* Price
* Liquidity
* Volume (5m / 1h / 24h)
* Market Cap
* FDV
* Price Change
* Pool Creation Time

---

### 📈 Historical Snapshots

Every execution stores a snapshot of:

* Price
* Liquidity
* Volume
* Market Cap
* FDV

This allows historical comparisons instead of relying only on current values.

---

### 📉 Momentum Detection

The bot calculates:

* Price Change %
* Liquidity Change %
* Volume Change %

between snapshots.

---

### ⭐ Smart Scoring Engine

Each pool receives a score based on:

* Price momentum
* Volume growth
* Liquidity
* Liquidity inflow/outflow
* Market Cap
* FDV health
* Volume/Liquidity ratio
* Pool age

Signal classifications include:

* 💎 GEM
* 🚀 EXPLOSIVE
* 🔥 STRONG
* 🟢 GOOD

---

### 🤖 Telegram Alerts

Qualified pools are automatically sent to Telegram with:

* Token Symbol
* DEX
* Price
* Liquidity
* Volume
* Market Cap
* Pool Age
* Signal Strength
* Scoring Reasons
* DexScreener Link

Example:

```text
🚀 EXPLOSIVE

🪙 MOTION
🏦 PANCAKESWAP

💵 Price       $0.00037 (+42%)
💧 Liquidity   $132,000 (+18%)
🔥 Volume      $298,000 (+245%)

⭐ Score: 14

🔗 https://dexscreener.com/...
```

---

### 🚫 Alert Deduplication

The bot stores previous alerts in SQLite.

Duplicate alerts are prevented unless:

* the cooldown period has expired, or
* the pool's score improves significantly.

This keeps Telegram notifications clean and meaningful.

---

# Database Structure

## pools

Stores the latest state of every discovered trading pair.

| Column          |
| --------------- |
| pair_address    |
| token_address   |
| chain_id        |
| symbol          |
| dex_id          |
| price_usd       |
| liquidity_usd   |
| volume_m5       |
| volume_h1       |
| volume_h24      |
| market_cap      |
| fdv             |
| pair_created_at |
| first_seen_at   |
| last_updated_at |

---

## pool_snapshots

Stores historical snapshots for every pool.

Used to calculate momentum metrics over time.

---

## alerts_sent

Stores previous Telegram alerts.

| Column       |
| ------------ |
| pair_address |
| score        |
| last_alert   |

---

# Tech Stack

* Python
* Pandas
* SQLite
* Requests
* Telegram Bot API
* DexScreener API

---

# APIs Used

DexScreener

* `/token-profiles/latest/v1`
* `/token-pairs/v1/{chainId}/{tokenAddress}`

Telegram Bot API

---

# Project Workflow

```text
DexScreener

        │

        ▼

Discover New Tokens

        │

        ▼

Retrieve Trading Pairs

        │

        ▼

Update Pools Database

        │

        ▼

Store Pool Snapshot

        │

        ▼

Calculate Momentum

        │

        ▼

Score Each Pool

        │

        ▼

Remove Duplicate Alerts

        │

        ▼

Send Telegram Alerts
```

---

# Future Improvements

* Automated execution every few hours (Cron / Task Scheduler / GitHub Actions)
* Multi-timeframe momentum analysis
* Wallet tracking
* Smart money detection
* Holder concentration analysis
* Rug pull risk scoring
* Multi-chain support
* Web dashboard
* Performance analytics
* Backtesting engine

---

# Disclaimer

This project is intended for educational and research purposes only.

It does not constitute financial advice. Always perform your own due diligence before making investment decisions.
