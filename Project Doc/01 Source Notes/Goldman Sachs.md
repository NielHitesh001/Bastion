---
title: "Goldman Sachs"
aliases: ["GS", "Goldman"]
tags: ["source-derived", "finance-tech", "knowledge-graph"]
status: complete
type: source-derived-note
source_file: "Goldman_Sachs.md"
related: ["[[Third-Party Vendors]]", "[[FIN Tech MOC Index]]", "[[Claims, Contradictions & Uncertainty]]"]
---
# [[Goldman Sachs]]

> [!info] Overview
> Goldman Sachs operates one of the most sophisticated proprietary trading and risk platforms in the world: **[[SecDB]]**. Built on **Slang**, a proprietary domain-specific language (DSL) based on OCaml, SecDB powers trading, risk calculation, and portfolio management across all asset classes for the entire firm. Goldman's technology philosophy emphasizes **immutability**, **type safety**, and **mathematical rigor** in quantitative finance.

---

## 🎯 Core Mission

- **Trading Desk Coverage**: Equities, Fixed Income, Currencies, Commodities, Derivatives, Structured Products
- **Risk Management**: Market risk, credit risk, operational risk, real-time Greeks, XVA
- **Portfolio Management**: Client advisory, systematic trading, rebalancing algorithms
- **Prime Services**: Prime brokerage, liquidity provision, capital markets intermediation
- **Public Cloud Initiative**: [[Marquee]] platform (cloud-native, REST API access)

---

## 🏗️ Proprietary Platforms

### [[SecDB]] — The Crown Jewel of GS Technology

**Launch Date**: ~1990s | **Status**: Active (continuous evolution) | **Scope**: Firm-wide

#### What is Slang?

> [!info] Slang DSL
> **Slang** is Goldman Sachs' proprietary domain-specific language, built on top of **OCaml**. It is:
> - **Functional**: Emphasizes immutability and pure functions
> - **Strongly typed**: Compile-time type checking catches errors before runtime
> - **Mathematical**: Designed for quantitative finance workflows (Greeks, risk, PnL attribution)
> - **In-memory database DSL**: Can specify and query in-memory databases in a declarative way
> - **Proprietary & confidential**: Not publicly documented; only taught internally at GS

**Why Slang?**
- Traditional imperative languages (Python, C++) allow mutable state, leading to subtle bugs in risk systems
- Slang's type system and immutability ensure correctness in critical-path risk calculations
- Performance: Compiled to efficient OCaml bytecode or native code
- Mathematical expressiveness: Natural syntax for expressing Greeks, sensitivities, factor models

---

#### SecDB Architecture

```
SecDB Platform (Slang/OCaml Foundation)
├─ Core Risk Engine
│  ├─ Greeks Calculation (delta, gamma, vega, theta, rho, 2nd order sensitivities)
│  ├─ XVA Framework (CVA, DVA, FVA, KVA, COLVA)
│  ├─ Scenario Analysis (historical, hypothetical, reverse stress)
│  ├─ Portfolio analytics (factor decomposition, hedge ratios)
│  └─ Regulatory capital (FRTB, SIMM, SA-CCR, stress VaR)
│
├─ Trading System
│  ├─ Order routing (multi-exchange, smart routing)
│  ├─ Execution algorithms (VWAP, TWAP, POV, adaptive)
│  ├─ Market-making strategies (inventory management, adverse selection)
│  └─ Cross-asset correlation tracking
│
├─ Portfolio Management
│  ├─ Multi-asset portfolio optimization
│  ├─ Rebalancing algorithms
│  ├─ Client advisory analytics
│  └─ Systematic trading engine
│
├─ Data & Feature Engineering
│  ├─ Tick-level market data (real-time + historical)
│  ├─ Greeks computation (real-time, batch)
│  ├─ Factor model construction (PCA, Fama-French)
│  └─ ML feature stores (alpha generation, predictive analytics)
│
├─ In-Memory Database (SecDB Core)
│  ├─ Immutable data structures (persistent BTrees)
│  ├─ Functional queries (map, filter, fold over portfolios)
│  ├─ Type-safe schema validation
│  └─ ACID semantics via OCaml runtime
│
└─ Infrastructure
   ├─ Distributed compute (Hadoop, Spark for batch analytics)
   ├─ Real-time streaming (Kafka for event-driven updates)
   ├─ Storage (custom time-series DB for tick data)
   └─ Observability (tracing, metrics, custom logging)
```

#### Technology Stack

| Layer | Language | Framework | Purpose | Notes |
|-------|----------|-----------|---------|-------|
| **Quantitative Logic** | Slang (OCaml) | Custom DSL | Greeks, XVA, risk | Proprietary, ~15k+ functions |
| **Core Computation** | OCaml | Core libraries | In-memory DB, analytics | Type-safe, immutable data structures |
| **Execution & Algo** | C++ | Boost, custom | Ultra-low-latency paths | Microsecond-range latency |
| **Data Pipeline** | Python | Kafka, Spark | Feature engineering, ML | Batch + streaming |
| **Front-end** | TypeScript/React | Node.js | Trader dashboards | Real-time market data, PnL display |
| **Infrastructure** | Go, Rust | Kubernetes | Orchestration, observability | Container management |

---

#### SecDB Capabilities

**Real-Time Risk Calculation**:
- Greeks: Delta, gamma, vega, theta, rho (on all derivative positions, firm-wide, <1 second latency)
- XVA: CVA, DVA, FVA, KVA (counterparty exposure, funding costs, capital charges)
- VaR/CVaR: Market risk quantification under multiple scenarios
- Regulatory capital: FRTB sensitivities, SIMM initial margin, SA-CCR exposure

**Multi-Asset Trading**:
- Equities: Market-making, systematic strategies, client execution
- Fixed Income: Bond trading, repo, interest rate derivatives
- FX: Currency trading, carry strategies, emerging market flows
- Commodities: Physical + derivatives, structured products
- Derivatives: Exotic options, structured products, OTC hedging

**Portfolio Management**:
- Real-time PnL attribution (market impact, trading, counterparty)
- Factor model decomposition (systematic risk, idiosyncratic risk)
- Hedge portfolio optimization
- Client advisory analytics

---

### [[Marquee]] — Cloud-Native, Public-Facing Platform

**Launch Date**: ~2020 | **Status**: Active & Growing | **Scope**: Client-facing API + web platform

> [!note] Strategic Shift
> Marquee represents GS's modernization: moving from internal-only SecDB to a cloud-native, API-first platform that allows external clients (asset managers, corporate treasuries, hedge funds) to access GS execution, analytics, and market data.

#### Architecture

```
Marquee Platform (Cloud-Native)
├─ REST API Layer
│  ├─ Authentication (OAuth2, API keys)
│  ├─ Rate limiting & quota management
│  ├─ Request logging & audit trail
│  └─ GraphQL API option (for complex queries)
│
├─ Data Services
│  ├─ Market data (real-time quotes, OHLCV, implied vols)
│  ├─ Analytics (Greeks, risk metrics, factor exposures)
│  ├─ Fundamentals (company financials, ownership)
│  └─ Alternative data (ML features, sentiment, indicators)
│
├─ Execution Services
│  ├─ Smart order routing (equities, FI, FX)
│  ├─ Algorithmic execution (VWAP, TWAP, POV)
│  ├─ Single-dealer platforms (structured products)
│  └─ Prime services (financing, securities lending)
│
├─ Portfolio Analytics
│  ├─ Real-time PnL
│  ├─ Greeks & risk metrics
│  ├─ Factor decomposition
│  ├─ Backtesting engine
│  └─ Scenario analysis
│
└─ Cloud Infrastructure (GS Preferred Cloud)
   ├─ AWS (primary) or Azure (alternate)
   ├─ Kubernetes for orchestration
   ├─ Managed databases (PostgreSQL, time-series)
   └─ Event streaming (Kafka for real-time updates)
```

#### Marquee API Endpoints (Illustrative)

```python
# Example: Marquee REST API usage (conceptual)

# Authentication
POST /auth/login
  → Returns JWT token

# Market Data
GET /api/v1/quotes?symbol=AAPL&product=equity
  → Returns real-time bid/ask quotes

# Risk & Analytics
GET /api/v1/portfolio/risk?portfolio_id=xyz
  → Returns VaR, Greeks, factor exposures

# Execution
POST /api/v1/orders/create
  → Submits order to GS execution engine
  → Returns order ID, estimated execution cost

# Analytics
GET /api/v1/analytics/backtest?strategy=momentum
  → Runs backtest, returns Sharpe, max drawdown, etc.
```

**Current Marquee Users**: 
- Tier-1 asset managers (BlackRock, Vanguard, Invesco)
- Hedge funds (Citadel, D.E. Shaw, Two Sigma)
- Corporate treasuries (Apple, Microsoft, Berkshire Hathaway)
- Insurance companies (AXA, Allianz, Munich Re)

---

## 🔗 Third-Party Integrations

| Platform | Purpose | Status | Notes |
|----------|---------|--------|-------|
| [[Bloomberg Terminal]] | Market data, news | Primary | Enterprise license |
| [[FactSet]] | Fundamental data | Primary | Equity research, analytics |
| [[Refinitiv Eikon]] | Market data, algos | Secondary | Backup data feeds |
| [[Murex MX3]] | XVA, risk (legacy) | Waning | Being phased for SecDB |
| [[Calypso]] | Multi-asset risk | Legacy | Deprecated in favor of SecDB |

---

## 🌐 Organizational Structure (Technology)

```
Chief Information Officer (CIO) — Goldman Sachs
├─ Chief Technology Officer (CTO) — Rob Lewin (as of 2024)
│  ├─ SecDB Platform Team (~2,000 engineers)
│  │  ├─ Slang Runtime & Compiler (~200 engineers)
│  │  │  ├─ Type system design & implementation
│  │  │  ├─ Performance optimization (JIT compilation)
│  │  │  └─ Library development (quantitative functions)
│  │  │
│  │  ├─ Risk Calculation Engine (~400 engineers)
│  │  │  ├─ Greeks computation (options, fixed income, FX)
│  │  │  ├─ XVA framework (CVA, DVA, FVA, KVA)
│  │  │  └─ Regulatory capital (FRTB, SIMM, SA-CCR)
│  │  │
│  │  ├─ Trading Systems (~600 engineers)
│  │  │  ├─ Order routing & execution
│  │  │  ├─ Algo development
│  │  │  └─ Market connectivity
│  │  │
│  │  └─ Infrastructure & Data (~800 engineers)
│  │     ├─ Kafka event streaming
│  │     ├─ Data pipeline (Hadoop, Spark)
│  │     └─ Observability & monitoring
│  │
│  ├─ Marquee Platform Team (~500 engineers)
│  │  ├─ API & Cloud services
│  │  ├─ Frontend (React, TypeScript)
│  │  └─ DevOps & cloud infrastructure
│  │
│  └─ Emerging Technologies
│     ├─ ML/AI research (predictive models, trade execution optimization)
│     ├─ Quantum computing exploration
│     └─ Blockchain research (stablecoins, DeFi)
```

---

## 📊 Key Metrics & Stats

| Metric | Value | Notes |
|--------|-------|-------|
| **Total Assets** | $2.5 trillion | As of Q2 2026 |
| **Daily Trading Volume** | $1.5-2 trillion | Across all asset classes |
| **Technology Headcount** | ~15,000 | ~30% of total workforce |
| **SecDB Users** | ~3,000 | Traders, quants, risk managers |
| **Slang Functions** | 15,000+ | Core quantitative library |
| **Marquee API Calls/Day** | 500M+ | Client analytics & data queries |

---

## 🔐 Risk & Regulatory Compliance

> [!warning] SecDB's Regulatory Role
> SecDB is critical to GS's regulatory compliance:
> - Real-time FRTB position tracking and sensitivity calculation
> - Daily SIMM computation for OTC derivatives counterparties
> - SA-CCR exposure monitoring
> - Stress testing (historical & hypothetical scenarios)
> - Trade audit trail and forensics

**Compliance Features**:
- Immutable audit trail (SecDB's functional programming prevents data mutation)
- Type-safe schema validation (Slang's type system enforces data integrity)
- Real-time alerting (regulatory limits, position breaches)
- Automated reporting (SEC filings, Federal Reserve, ECB, ESMA)

---

## 🚀 Recent Innovations

### Marquee Evolution (2023–2026)

- **GraphQL API**: More flexible, efficient data queries for clients
- **Websocket Streaming**: Real-time market data & PnL updates to clients
- **ML-driven Execution**: Reinforcement learning for optimal trade execution
- **White-label Services**: Allowing fintech partners to embed GS analytics

### Slang & SecDB Modernization (2024–2026)

- **JIT Compilation**: Real-time compilation of Slang code for 2x performance boost
- **Distributed SecDB**: Horizontal scaling of SecDB across multiple datacenters
- **GPU-accelerated Greeks**: CUDA-based computation for large portfolios
- **Federated Learning**: Cross-desk analytics without centralizing sensitive data

---

## 🎓 Lessons & Best Practices

> [!tip] Type Safety & Correctness in Risk Systems
> Goldman Sachs' investment in Slang and functional programming demonstrates the power of **correctness-first** engineering:
> 
> 1. **Type System**: Compile-time type checking catches 70% of bugs before runtime
> 2. **Immutability**: Prevents subtle state mutation bugs (critical for risk calculations)
> 3. **Functional Composition**: Encourages building complex systems from simple, proven components
> 4. **Performance**: OCaml's efficient runtime delivers microsecond-level latency
> 5. **Domain-Specific Language**: Allows quants to express complex financial logic without general-purpose programming knowledge
> 6. **Audit Trail**: Immutability ensures tamper-proof records (regulatory requirement)

---

## 🔗 See Also

- [[SecDB]]
- [[Slang]]
- [[Marquee]]
- [[Goldman Sachs Technology]]
- [[JPMorgan Chase]] (comparison: Athena vs. SecDB)
- [[Bank of America]] (comparison: Quartz)
- [[Third-Party Vendors]] (Murex, Calypso legacy)

---

**Last Updated**: 2026-09-16 | **Status**: Active | **Confidence**: High

---

## Vault Connections

This source-derived note is connected to [[Third-Party Vendors]], [[FIN Tech MOC Index]], [[Claims, Contradictions & Uncertainty]]. Its claims are retained as supplied; consult [[Claims, Contradictions & Uncertainty]] for cross-source caveats.
