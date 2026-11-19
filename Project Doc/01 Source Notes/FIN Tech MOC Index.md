---
title: "FIN Tech MOC Index"
aliases: ["FIN_TECH_MOC_INDEX", "Financial Tech MOC", "Global Financial Institutions & Trading Tech Stack Map"]
tags: ["source-derived", "finance-tech", "knowledge-graph"]
status: complete
type: source-derived-note
source_file: "FIN_TECH_MOC_INDEX.md"
related: ["[[JPMorgan Chase]]", "[[Goldman Sachs]]", "[[Jane Street]]", "[[Third-Party Vendors]]", "[[Athena]]", "[[Canvas Layout Guide]]"]
---
# Global Financial Institutions & Trading Tech Stack Map

> [!info] Purpose
> This Map of Content (MOC) documents proprietary and third-party software platforms used by tier-1 financial institutions for trading, risk management, portfolio management, and back-office operations. Designed for traders, quants, architects, and engineers evaluating trading infrastructure.

---

## 📊 Institution Categories

### [[#Tier-1 Investment Banks|Tier-1 Investment Banks]]
The "Big Four" + extended network of global systemically important financial institutions (G-SIBs) with in-house proprietary trading and risk engines.

### [[#Quant Funds & Market Makers|Quant Funds & Market Makers]]
High-frequency traders and quantitative research shops with custom infrastructure stacks.

### [[#Asset Managers & Index Operators|Asset Managers & Index Operators]]
Large-scale portfolio management, risk analytics, and multi-asset execution platforms.

### [[#Third-Party Vendors & Enterprise Software|Third-Party Vendors & Enterprise Software]]
Middleware, OMS/EMS, risk systems, and middle/back-office platforms used across the industry.

---

## 🏛️ Tier-1 Investment Banks

| Firm                | Primary Trading Engine     | Risk & Analytics       | Portfolio Mgmt | OMS/EMS            | Status |
| ------------------- | -------------------------- | ---------------------- | -------------- | ------------------ | ------ |
| [[JPMorgan Chase]]  | [[Athena]] (Python)        | [[Athena]], [[Condor]] | Proprietary    | [[Athena]]         | Active |
| [[Goldman Sachs]]   | [[SecDB]] (Slang/OCaml)    | [[SecDB]]              | [[Marquee]]    | [[Marquee]]        | Active |
| [[Bank of America]] | [[Quartz]] (Python)        | [[Quartz]]             | Proprietary    | [[Quartz]]         | Active |
| [[Morgan Stanley]]  | [[Strateger]] (C++/Python) | Proprietary            | [[CAMM]]       | Proprietary        | Active |
| [[Citi]]            | [[Sophis]], [[Kondor+]]    | [[Kondor+]] (Murex)    | Proprietary    | [[Kondor+]]        | Active |
| [[Deutsche Bank]]   | [[Condor]] (Python)        | Proprietary            | Proprietary    | [[Kondor+]]        | Active |
| [[Barclays]]        | [[Quartz]] (licensed)      | [[Murex MX3]]          | Proprietary    | [[Murex MX3]]      | Active |
| [[HSBC]]            | [[Murex MX3]]              | [[Murex MX3]]          | Proprietary    | [[Bloomberg TOMP]] | Active |

---

## 🎯 Quant Funds & Market Makers

| Firm | Core Infrastructure | Research Stack | Execution Engine | Language | Status |
|------|---------------------|-----------------|------------------|----------|--------|
| [[Jane Street]] | Custom OCaml stack | [[OCaml]], [[Janestreet ML]] | [[JaneFlow]] | OCaml | Active |
| [[Citadel Securities]] | [[Medallion-class Engine]] | Python/C++/FPGA | Custom HFT | C++/FPGA | Active |
| [[Two Sigma]] | [[Vega]] (distributed) | Python/Jupyter | Custom | Python/C++ | Active |
| [[DE Shaw]] | [[Gridlock]] (distributed) | Python/Scala | Custom HFT | Python/Scala/C++ | Active |
| [[Susquehanna]] | Custom C++ stack | Research notebooks | Custom execution | C++ | Active |
| [[Optiver]] | [[Amsterdam Engine]] | Python/C++ | Custom HFT | C++/Rust | Active |
| [[Virtu Financial]] | [[Firefly]] (proprietary) | Python/ML | HFT execution | C++/Python | Active |
| [[Wintermute]] | [[Blockchain-native]] | Python/Rust | Custom DEX/CEX | Rust/Python | Active |

---

## 💼 Asset Managers & Index Operators

| Firm             | Portfolio Mgmt         | Risk System   | OMS/EMS            | Key Platforms        | Status |
| ---------------- | ---------------------- | ------------- | ------------------ | -------------------- | ------ |
| [[BlackRock]]    | [[Aladdin]]            | [[Aladdin]]   | [[Aladdin]]        | Bloomberg, FactSet   | Active |
| [[Vanguard]]     | Proprietary            | [[Murex MX3]] | [[Bloomberg TOMP]] | Custom + third-party | Active |
| [[State Street]] | [[State Street Alpha]] | [[Kondor+]]   | [[ION Trading]]    | Bloomberg, Murex     | Active |
| [[Fidelity]]     | Proprietary            | Proprietary   | Custom             | Bloomberg, FactSet   | Active |
| [[Invesco]]      | Proprietary            | [[Calypso]]   | [[Charles River]]  | Bloomberg, Murex     | Active |
| [[Lazard]]       | Proprietary            | Proprietary   | [[Charles River]]  | Bloomberg            | Active |

---

## 🔧 Third-Party Vendors & Enterprise Software

### Risk Management & Analytics

| Platform | Vendor | Core Use | Language | Clients | Status |
|----------|--------|----------|----------|---------|--------|
| [[Murex MX3]] | Murex Group | XVA, risk, portfolio analytics | Java/Python | 40+ global banks | Active |
| [[Kondor+]] | Murex Group | Scenario analysis, risk aggregation | Java/C++ | 15+ tier-1 banks | Active |
| [[Calypso]] | Calypso Technology | Multi-asset risk & PnL | Java/Python | 50+ institutions | Active |
| [[Beacon]] | ION Analytics | Portfolio analytics, reporting | Python/Java | 200+ firms | Active |
| [[SunGard APT]] | FIS | Derivatives risk, XVA | Java/C++ | Legacy (declining) | Waning |
| [[Maat]] | Maat Exchange | FX risk, portfolio analytics | Python | Niche (FX shops) | Niche |

### Order Management & Execution

| Platform | Vendor | Core Use | Language | Clients | Status |
|----------|--------|----------|----------|---------|--------|
| [[Charles River]] | State Street | OMS/EMS, portfolio management | Java | 500+ firms | Active |
| [[ION Trading]] | ION | OMS/EMS, middle-office orchestration | Java/C++ | 200+ firms | Active |
| [[Bloomberg TOMP]] | Bloomberg | Algo execution, real-time trading | Proprietary | 400+ firms | Active |
| [[FlexTrade]] | FlexTrade Systems | Algo execution, smart order routing | Java/C++ | 200+ firms | Active |
| [[Portware]] | Portware Inc. | Algo execution, OMS | C++/Java | 100+ hedge funds | Active |
| [[TradingScreen]] | TS Holdings | OMS/EMS, multi-asset | Java/JavaScript | 50+ firms | Active |

### Data & Research Infrastructure

| Platform | Vendor | Core Use | Language | Clients | Status |
|----------|--------|----------|----------|---------|--------|
| [[Bloomberg Terminal]] | Bloomberg LP | Market data, news, analytics | Proprietary | 350,000+ seats | Active |
| [[FactSet]] | FactSet Research Systems | Portfolio analytics, data aggregation | Python/Java | 150,000+ users | Active |
| [[Refinitiv Eikon]] | Refinitiv (LSEG) | Market data, research, analytics | Proprietary/REST API | 200,000+ users | Active |
| [[S&P Capital IQ]] | S&P Global | Fundamentals, credit data | Proprietary | 100,000+ users | Active |

---

## 🏗️ Proprietary Trading Engine Deep Dives

### Investment Banks

#### [[JPMorgan Chase]] — [[Athena]]

```
Athena (Python-based, 2010–present)
├─ Trading Engine (order routing, execution, PnL)
├─ Risk Aggregation (market risk, credit risk, operational risk)
├─ Portfolio Analytics (Greeks, scenario analysis)
└─ Back-Office Integration (settlement, clearing, reporting)

Ancillary Systems:
├─ [[Condor]] (Latin American derivatives trading)
├─ [[Tsunami]] (High-frequency execution, FX)
└─ [[Athena Mobile]] (Mobile trading dashboard)
```

> [!warning] Note
> Athena is JPM's crown jewel—built for sub-millisecond latency across asset classes. Estimated 10,000+ developers contribute. Modular Python architecture allows rapid deployment across trading desks.

#### [[Goldman Sachs]] — [[SecDB]]

```
SecDB (Slang/OCaml-based, 1990s–present)
├─ Slang Language Runtime (in-memory database + DSL)
├─ Risk Engine (XVA, Greeks, sensitivities)
├─ Portfolio Management (multi-asset, cross-border)
└─ Trading Systems (algo execution, PnL attribution)

Ancillary Systems:
├─ [[Marquee]] (Public cloud platform, REST API)
├─ [[Marquee APIs]] (Third-party data/execution access)
└─ [[Goldman Sachs Engineering]] (Infrastructure + ML)
```

> [!info] Slang DSL
> Slang is GS's proprietary domain-specific language (OCaml-based). Extremely powerful for risk calculations but steep learning curve. Only GS and select partners use it.

#### [[Bank of America]] — [[Quartz]]

```
Quartz (Python-based, 2012–present)
├─ Python Execution Engine (order routing, algo execution)
├─ Real-Time Risk (market risk, credit risk)
├─ Portfolio Analytics (PnL, Greeks, stress testing)
└─ Data Pipeline (tick data, market microstructure)

Ancillary Systems:
├─ [[Quartz ML]] (Machine learning for trading signals)
├─ [[Quartz Cloud]] (Multi-cloud risk aggregation)
└─ [[BofA Analytics]] (Proprietary charting/dashboards)
```

#### [[Morgan Stanley]] — [[Strateger]] & Proprietary Stack

```
Strateger (C++/Python hybrid, 2000s–present)
├─ C++ Core (ultra-low latency order routing)
├─ Python Layer (strategy development, backtesting)
├─ Risk Calculation (Real-time Greeks, VaR, CVaR)
└─ Portfolio Management ([[CAMM]], multi-asset)

Ancillary Systems:
├─ [[Morgan Stanley Infrastructure]] (Kubernetes, Hadoop)
├─ [[MS Prime Services Platform]] (Multi-currency, OTC)
└─ [[Eaton Vance Integration]] (Asset manager suite)
```

#### [[Citi]] — [[Kondor+]] + Proprietary Stack

```
Kondor+ (Murex, multi-asset risk platform)
├─ Scenario Analysis
├─ Counterparty Risk
├─ XVA Calculations
└─ Regulatory Reporting (FRTB, SIMM)

Proprietary Trading Stack:
├─ [[Sophis]] (Murex—equity derivatives, structured products)
├─ Custom OMS (Citi-built, order routing + execution)
└─ Citi Proprietary Risk Engine (Cross-asset consolidation)
```

---

### Quant Funds & Market Makers

#### [[Jane Street]] — OCaml Stack

```
Jane Street Infrastructure (OCaml-first, 1999–present)
├─ [[OCaml Runtime]] (Core trading logic, consensus)
├─ [[Flow Computation]] (Real-time market microstructure)
├─ [[Order Book Simulation]] (Latency modeling, risk)
└─ [[Janestreet ML]] (Custom ML research environment)

Execution Layers:
├─ [[JaneFlow]] (Custom HFT execution engine)
├─ [[FPGA Accelerators]] (Latency-critical paths)
└─ [[Custom Network Stack]] (Colocation optimization)

Tech Stack:
├─ 90%+ OCaml (type safety, immutability)
├─ Some C++ (kernel drivers, networking)
└─ Custom Lisp dialects (quantitative research)
```

> [!note] OCaml Philosophy
> Jane Street's reliance on OCaml is deliberate: strong type system catches bugs at compile-time, immutability ensures thread-safe data structures, and pattern matching aligns with complex quantitative logic.

#### [[Citadel Securities]] — Proprietary HFT Engine

```
Citadel Securities Infrastructure (2001–present)
├─ [[Medallion-scale Execution Engine]] (C++/CUDA)
├─ [[Proprietary Matching Engine]] (Microsecond latency)
├─ [[ML/AI Research Stack]] (Python, TensorFlow, PyTorch)
└─ [[Global Market Microstructure Engine]] (Multi-exchange coordination)

Execution Layers:
├─ [[FPGA Accelerators]] (Ultra-low latency paths)
├─ [[Custom Network Protocols]] (Proprietary exchange communication)
└─ [[Latency Arbitrage Engine]] (Statistical edge extraction)

Data Infrastructure:
├─ [[Petabyte-scale Tick Data]] (Custom time-series DB)
├─ [[Real-time Feature Computation]] (ML feature engineering)
└─ [[High-frequency Signal Generation]] (Alpha extraction)
```

#### [[Two Sigma]] — Distributed Research & Execution

```
Two Sigma Infrastructure (2001–present)
├─ [[Vega]] (Distributed backtesting + simulation engine)
├─ [[Jupyter-based Research]] (Interactive ML development)
├─ [[Custom OMS/EMS]] (Multi-asset execution)
└─ [[Real-time Risk Engine]] (Microsecond-latency Greeks)

Tech Stack:
├─ Python (research, strategy development, 80% of codebase)
├─ C++ (critical-path execution, networking, 15%)
├─ Java (data orchestration, 5%)
└─ Scala (distributed computing, stream processing)

Data Infrastructure:
├─ [[Kafka Streaming]] (Tick-level market data)
├─ [[Hadoop/Spark]] (Backtesting, analytics)
├─ [[Custom Time-Series DB]] (PB-scale OHLCV storage)
└─ [[ML Feature Store]] (Real-time signal computation)
```

#### [[DE Shaw]] — Gridlock Distributed Engine

```
D.E. Shaw & Co. Infrastructure (1988–present)
├─ [[Gridlock]] (Distributed compute platform, Python/Scala)
├─ [[Custom Backtesting Engine]] (Scalable simulation)
├─ [[Real-time Signal Engine]] (ML/statistical arbitrage)
└─ [[Multi-exchange Execution]] (Smart order routing)

Tech Stack:
├─ Python (research, ML, 60% of codebase)
├─ Scala (distributed computing, streaming, 20%)
├─ C++ (core execution, latency-critical paths, 15%)
└─ Java (infrastructure, orchestration, 5%)

Research Paradigm:
├─ [[Shaw Quantitative Methods]] (Proprietary statistical models)
├─ [[ML/AI Research]] (Deep learning, reinforcement learning)
└─ [[Systematic Trading]] (Rules-based + adaptive algorithms)
```

---

## 📡 Communication & Data Flow (Simplified)

```
Market Data Sources (Exchanges, Feeds)
    ↓
Data Ingestion Layer (Kafka, custom protocols)
    ↓
Feature Engineering (ML, Greeks computation)
    ↓
Signal Generation / Strategy Engine
    ↓
Risk Calculation (Real-time PnL, Greeks, VaR)
    ↓
Order Management System (OMS)
    ↓
Execution (Smart routing, algo execution)
    ↓
Trade Confirmation → Back-office (Settlement, Clearing)
    ↓
Reporting (Risk reports, regulatory, P&L)
```

---

## 🎯 Tech Stack Patterns by Institution Type

### Tier-1 Investment Banks
- **Primary Languages**: Python, C++, Java
- **Risk Systems**: Proprietary (SecDB, Athena, Quartz, Strateger) or Murex/Calypso
- **Data Fabric**: Bloomberg Terminal, custom real-time tick databases
- **Execution**: Proprietary OMS or [[Bloomberg TOMP]], [[ION Trading]]
- **Infrastructure**: Kubernetes, Hadoop, custom colocation networks

### Quant Funds & Market Makers
- **Primary Languages**: Python, C++, OCaml (Jane Street), Scala
- **Risk Systems**: Custom, ultra-low-latency in-memory databases
- **Data Fabric**: Direct exchange feeds, custom tick databases, proprietary ML feature stores
- **Execution**: Proprietary HFT engines, custom matching engines
- **Infrastructure**: Ultra-low latency (co-location, custom kernel modules, FPGA)

### Asset Managers
- **Primary Languages**: Python, Java, JavaScript
- **Risk Systems**: [[Aladdin]] (BlackRock), Murex, Calypso, proprietary
- **Data Fabric**: Bloomberg Terminal, FactSet, Refinitiv, proprietary fundamental data
- **Execution**: [[Charles River]], [[ION Trading]], [[Bloomberg TOMP]]
- **Infrastructure**: Multi-cloud, Hadoop for analytics, Kubernetes for orchestration

---

## 📊 Dataview Queries

### Query 1: All firms using proprietary Python-based engines

```dataview
TABLE WITHOUT ID
  file.link as Firm,
  prop_system as "Proprietary System",
  year_launched as "Year Launched"
WHERE contains(tags, "#tech/python") AND contains(tags, "#tech/proprietary")
SORT Firm ASC
```

### Query 2: Third-party vendors by category

```dataview
TABLE WITHOUT ID
  file.link as "Vendor Platform",
  vendor as "Vendor",
  category as "Category",
  clients_count as "Approx. Clients"
WHERE contains(tags, "#vendor") AND category != null
SORT clients_count DESC
```

### Query 3: Firms using specific third-party platforms (Murex example)

```dataview
TABLE WITHOUT ID
  file.link as Firm,
  murex_modules as "Murex Modules",
  other_vendors as "Other Vendors"
WHERE contains(third_party, "Murex")
SORT Firm ASC
```

### Query 4: Execution engines by language

```dataview
TABLE WITHOUT ID
  file.link as "Engine",
  primary_language as "Primary Language",
  latency_target as "Latency Target (μs)",
  owner as "Owner"
WHERE contains(tags, "#tech/trading-engine")
SORT latency_target ASC
```

---

## 🎨 Obsidian Canvas Layout Guide

### Recommended Canvas Structure (`.canvas` file)

```
┌─────────────────────────────────────────────────────────────────────┐
│                    TIER-1 INVESTMENT BANKS                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  JPMorgan Chase ─────→ [[Athena]]                                   │
│         │                   ├─→ [[Condor]]                          │
│         └──────────────────→ [[Bloomberg TOMP]]                     │
│                                                                       │
│  Goldman Sachs ──────→ [[SecDB]] (Slang/OCaml)                      │
│         │                   ├─→ [[Marquee]]                         │
│         └──────────────────→ Third-party data (FactSet, Bloomberg)  │
│                                                                       │
│  BofA ───────────────→ [[Quartz]] (Python)                          │
│         │                   ├─→ [[Murex MX3]]                       │
│         └──────────────────→ [[Charles River]] (OMS)                │
│                                                                       │
│  Morgan Stanley ─────→ [[Strateger]] (C++/Python)                   │
│         │                   ├─→ [[CAMM]] (Portfolio Mgmt)           │
│         └──────────────────→ [[Bloomberg TOMP]]                     │
│                                                                       │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│              QUANT FUNDS & MARKET MAKERS                             │
├─────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  Jane Street ────────→ [[OCaml Stack]]                              │
│         │                   ├─→ [[JaneFlow]] (HFT)                  │
│         └──────────────────→ [[FPGA Accelerators]]                  │
│                                                                       │
│  Citadel Securities ─→ [[Medallion-class Engine]] (C++/CUDA)        │
│         │                   ├─→ Proprietary Matching Engine         │
│         └──────────────────→ [[ML/AI Research]] (TensorFlow)        │
│                                                                       │
│  Two Sigma ──────────→ [[Vega]] (Distributed engine)                │
│         │                   ├─→ [[Jupyter Research]]                │
│         └──────────────────→ [[Kafka]], [[Hadoop/Spark]]            │
│                                                                       │
│  D.E. Shaw ──────────→ [[Gridlock]] (Distributed compute)           │
│         │                   ├─→ Custom Backtesting                  │
│         └──────────────────→ [[Python/Scala/C++]] stack             │
│                                                                       │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│            ASSET MANAGERS & INDEX OPERATORS                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  BlackRock ──────────→ [[Aladdin]]                                  │
│         │                   ├─→ Risk aggregation                    │
│         │                   ├─→ Portfolio analytics                 │
│         └──────────────────→ Data (Bloomberg, FactSet)              │
│                                                                       │
│  Vanguard ──────────→ Proprietary Portfolio Platform                │
│         │                   ├─→ [[Murex MX3]] (Risk)                │
│         └──────────────────→ [[Bloomberg TOMP]] (Execution)         │
│                                                                       │
│  State Street ──────→ [[State Street Alpha]] + [[Kondor+]]          │
│         │                   ├─→ [[ION Trading]] (OMS)               │
│         └──────────────────→ [[Bloomberg Terminal]]                 │
│                                                                       │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│         THIRD-PARTY VENDORS & ENTERPRISE SOFTWARE                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                       │
│  ┌─ Risk Systems                                                     │
│  │   ├─ [[Murex MX3]] ──→ Used by Citi, BofA, HSBC, Barclays       │
│  │   ├─ [[Kondor+]] ────→ Used by Citi, Deutsche Bank, Invesco     │
│  │   ├─ [[Calypso]] ────→ Used by 50+ institutions                 │
│  │   └─ [[Beacon]] ─────→ Portfolio analytics                       │
│  │                                                                    │
│  ├─ OMS/EMS                                                          │
│  │   ├─ [[Charles River]] ──→ State Street, 500+ firms              │
│  │   ├─ [[ION Trading]] ────→ Multi-asset orchestration             │
│  │   ├─ [[Bloomberg TOMP]] ─→ Algo execution, 400+ firms            │
│  │   └─ [[FlexTrade]] ──────→ Smart order routing                   │
│  │                                                                    │
│  └─ Data & Market Data                                              │
│      ├─ [[Bloomberg Terminal]] ──→ 350,000+ users                   │
│      ├─ [[FactSet]] ────────────→ Fundamental data                  │
│      ├─ [[Refinitiv Eikon]] ────→ Market data & research            │
│      └─ [[S&P Capital IQ]] ─────→ Credit data                       │
│                                                                       │
└─────────────────────────────────────────────────────────────────────┘
```

### Canvas File Connections (Text Summary)

**Node Types:**
- 🟦 **Blue**: Investment banks, quant funds
- 🟩 **Green**: Proprietary systems (Athena, SecDB, Quartz, OCaml)
- 🟨 **Yellow**: Third-party vendors (Murex, Calypso, Charles River)
- 🟧 **Orange**: Market data providers (Bloomberg, FactSet)
- 🟥 **Red**: Critical dependencies (Execution engines, risk systems)

**Edge Patterns:**
- Solid lines → Direct primary integration
- Dashed lines → Secondary or legacy integration
- Color-coded edges → Technology category (Python, C++, Java, proprietary DSLs)

---

## 🔗 Related Ecosystems

- [[#Financial Regulatory Reporting|Financial Regulatory Reporting]] (FRTB, SIMM, regulatory PnL)
- [[#Cloud Infrastructure for FinTech|Cloud Infrastructure]] (AWS, Azure, GCP adoption in finance)
- [[#ML/AI in Trading|ML/AI in Trading]] (Feature engineering, signal generation, risk prediction)
- [[#FX & Payments Infrastructure|FX & Payments Infrastructure]] (Real-time gross settlement, SWIFT alternatives)

---

## 📚 Key Sources & References

- **Bloomberg Terminal Guides** (institutional trading)
- **Murex & Calypso Documentation** (risk management)
- **Jane Street Technical Talks** (OCaml in production trading)
- **Goldman Sachs Engineering Blog** (Slang, SecDB philosophy)
- **JPMorgan Chase Technology Blog** (Athena evolution, ML in risk)
- **Citadel Securities Careers** (HFT architecture insights)
- **Two Sigma & DE Shaw Whitepapers** (Research infrastructure, distributed computing)

---

## 🚀 Quick Navigation

| Section | Purpose |
|---------|---------|
| [[#Tier-1 Investment Banks\|Investment Banks]] | JPMorgan, Goldman, BofA, Morgan Stanley, Citi, Deutsche, Barclays, HSBC |
| [[#Quant Funds & Market Makers\|Quant Funds]] | Jane Street, Citadel, Two Sigma, DE Shaw, Optiver, Virtu, Susquehanna |
| [[#Asset Managers & Index Operators\|Asset Managers]] | BlackRock, Vanguard, State Street, Fidelity, Invesco |
| [[#Third-Party Vendors & Enterprise Software\|Vendors]] | Murex, Calypso, Charles River, Bloomberg, ION, FlexTrade |
| [[#Dataview Queries\|Dataview Queries]] | Automated filtering, cross-firm tech stack analysis |
| [[#Obsidian Canvas Layout Guide\|Canvas Guide]] | Visual layout recommendations |

---

**Last Updated**: 2026-09-16 | **Maintained by**: Quant Finance Tech Knowledge Base | **Public**: Reference only—institutional details subject to change.

---

## Vault Connections

This source-derived note is connected to [[JPMorgan Chase]], [[Goldman Sachs]], [[Jane Street]], [[Third-Party Vendors]], [[Athena]], [[Canvas Layout Guide]]. Its claims are retained as supplied; consult [[Claims, Contradictions & Uncertainty]] for cross-source caveats.
