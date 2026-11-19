---
title: "Athena Detailed Report"
aliases: ["Athena Evidence Review", "Athena - JPMorgan Trading and Risk Platform"]
tags: ["source-derived", "finance-tech", "knowledge-graph"]
status: complete
type: source-derived-note
source_file: "Athena Detailed Report.md"
related: ["[[Athena]]", "[[JPMorgan Chase]]", "[[Architecture & Workflow]]", "[[Claims, Contradictions & Uncertainty]]", "[[ADR-001 Incremental Dependency Graph]]", "[[ADR-002 Bitemporal Store]]", "[[ADR-003 Reproducible Results]]"]
---
# Athena — J.P. Morgan's Trading, Pricing & Risk Platform

> [!abstract] What this note is A source-graded technical breakdown of Athena, the Python-first, cross-asset pricing / trading / risk platform at the heart of J.P. Morgan's Corporate & Investment Bank (CIB) Markets business. Every claim carries a confidence tag. JPMC has never published an architecture white paper, so this is assembled from conference talks, a 2022 AWS session, press, founder bios, job postings, and community accounts.

> [!warning] Corrections to the original brief Check these before reusing anything downstream.
> 
> 1. **Origin direction is reversed.** Athena was _not_ built by people recruited from Bank of America/Merrill. It was started in **2006** by **Kirat Singh and Mark Higgins**, both ex-Goldman (SecDB). Singh left around 2010 to build **Quartz at BofA**, so Quartz is the descendant, not the ancestor. [^beacon-blog][^beacon-bio][^beacon-tl]
> 2. **"Bob" is not a build/dependency system** in any source I found. Community accounts describe it as the **job runner**, and the same name reportedly appears at other banks. 🟡 [^hn]
> 3. **Bi-temporal Hydra is not publicly confirmed.** Public material confirms a _globally replicated object DB_ with _timestamped versions_. Full valid-time + transaction-time modeling is a reasonable inference, not a documented fact. See §3.3.
> 4. **Two scale figures don't hold up as Athena figures.** "Millions of daily trades" has no source. "Hundreds of petabytes" is **firm-wide** (450+ PB across all of JPMC), not Athena. [^aws22]
> 5. **"Crown jewel" is a characterization**, not an official designation. I found no first-party use of it. Section 1.4 covers why it's apt.
> 6. **"Athena Risk" and "affinity-based scheduling"** are not publicly documented names or mechanisms. I treat them as informal or inferred.

> [!info] Confidence legend 🟢 **Primary**: JPMC/ex-JPMC speakers, official pages, job postings from JPMC. 🟡 **Secondary**: press, vendor bios, anonymous community accounts. ⚪ **Inference**: industry-standard practice consistent with the evidence but _not_ documented for Athena.

---

## 0. TL;DR

- Athena is a **Python-based pricing, trading, risk and analytics platform** with data-science and ML tooling, used across multiple business lines. 🟢 [^pydata]
- Its foundation, per JPMC's own 2017 talk: **Hydra** (globally replicated object DB), **Reactive Athena** (C++/Python reactive dataflow), **Pixie** (DAG dependency graph), a **Qt** app framework, **Athena Web**, a job scheduler, and a compute-grid integration. 🟢 [^pydata]
- It is a **monorepo monolith with continuous delivery**: 10–15K production changes/week (2017) growing to ~20K/week (2022). 🟢 [^pydata][^aws22]
- **Scale (JPMC's 2022 numbers):** 4K+ Python developers, ~50M lines of code, ~5B calculations/day, ~10K batch jobs/day. 🟢 [^aws22]
- It replaced **nine separate fixed-income systems**, then spread to equities and other asset classes. It was later partly exposed to clients via the _Investment Analytics Platform_ and _Risk as a Service_. 🟡 [^cnbc][^raas]
- The cloud story is **hybrid**: on-prem grid and data plus AWS (EKS, Auto Scaling, ElastiCache/Redis, RDS, S3, KMS), documented in detail for the ML hedging/risk use case. 🟢 [^aws22]

---

## 1. Executive Overview, Aliases & Internal Nomenclature

### 1.1 What Athena is

Athena is J.P. Morgan's in-house, cross-asset **front-to-back risk, pricing, and trade-management platform** for Markets. The same codebase supports trade capture, live risk, P&L, and middle/back-office processes. For example, commodities handles everything from trade capture through day-to-day live risk and operations inside Athena. 🟢 [^jobs-comm] Its stated design goal is to give quants, risk managers and technologists one consistent, cross-asset set of models, frameworks and tools. 🟢 [^jobs-qr]

### 1.2 Nomenclature map

|Name|What it is|Confidence|Notes|
|---|---|---|---|
|**Athena**|The overall platform and ecosystem|🟢|Python application layer, C++ at performance-critical layers|
|**Athena Platform Core / "Athena Core"**|Team owning core platform: distributed object storage, data streaming, IDE|🟢|Product-owner postings list these as core products [^jobs-core]|
|**Athena Analytics / QR Athena**|Quant Research developers writing pricing libraries, market models, risk frameworks|🟢|[^jobs-qr]|
|**Hydra**|Globally replicated object database (holds data _and_ code)|🟢|[^pydata]|
|**Pixie** (Pixie Graph)|Directed acyclic dependency graph|🟢|[^pydata]|
|**Reactive / Reactive Athena**|C++/Python reactive dataflow framework; event-driven, forward-propagating graph|🟢|[^pydata][^jobs-qr]|
|**Deal Model**|Trade/deal object model|🟢|Named in a QR job description [^jobs-qr]|
|**Real-Time Risk Reporting framework**|Streaming risk/P&L reporting layer|🟢|[^jobs-qr]|
|**Athena Web**|Web UI stack: tornado, HTML5, websockets, JS, WebAssembly|🟢|[^pydata]|
|**Athena application framework**|Qt-based desktop UI framework|🟢|[^pydata]|
|**Risk Scheduler**|Dispatches risk jobs onto the compute grid/cloud|🟢|Appears in the 2022 AWS diagram [^aws22]|
|**Bob**|Job runner / scheduler (per community)|🟡|Not confirmed by JPMC; see Correction 2 [^hn]|
|**Risk as a Service (RaaS)**|Hosted client offering: streaming and on-demand risk/P&L at portfolio, position or trade level, via web UI and APIs|🟢|Marketed as using the same analytics as J.P. Morgan's own desks [^raas]|
|**Investment Analytics Platform (IAP)**|First _external_ product built on Athena; sold via custody/fund services|🟡|208 clients signed + 42 expected, 2018 [^cnbc]|
|**Athena ML Hedging & Risk framework**|ML-based hedging service on AWS|🟢|[^aws22]|
|"Athena Risk"|Informal label for Athena's risk functionality|⚪|Not a documented product name|

### 1.3 How Athena compares to its siblings

Athena, Quartz and Beacon share a lineage through the same two engineers. SecDB is the common ancestor.

|Dimension|**SecDB** (Goldman)|**Athena** (J.P. Morgan)|**Quartz** (BofA/Merrill)|**Beacon** (vendor)|
|---|---|---|---|---|
|Started|1992 🟡 [^beacon-tl]|2006 🟡 [^beacon-blog]|~2010 🟡 [^beacon-tl]|2014 🟡 [^yespress]|
|Key people|(many)|Kirat Singh, Mark Higgins 🟡|Kirat Singh 🟡|Singh, Higgins (co-founders) 🟡|
|Primary language|Slang (proprietary) ⚪|Python (+C++) 🟢|Python 🟡|Python 🟡|
|Object store|SecDB database ⚪|Hydra 🟢|Reported in community accounts only 🟡|Cloud-native store 🟡|
|Dependency graph|Core design idea ⚪|Pixie + Reactive 🟢|DAG (community) 🟡|Yes (by design lineage) 🟡|
|Scale signal|Firmwide risk view|4K+ Python devs, ~50M LOC (2022) 🟢|5,000+ developers 🟡 [^beacon-bio]|Customer-hosted|
|External access|SecDB-based analytics offered to clients (c. 2017) 🟡 [^risk-open]|IAP (2018), RaaS 🟡|Internal|Commercial; acquired by Clearwater (closed 30 Apr 2025) 🟡 [^yespress]|

> [!note] Aladdin is a different animal BlackRock's Aladdin is a buy-side portfolio-management and risk platform that was licensed outward (reported ~$529M licensing revenue in 2015). It's a commercial benchmark for what JPMC and Goldman hoped to achieve by opening their internal systems, not a sell-side pricing/trading-engine peer. 🟡 [^risk-open]

### 1.4 Why people call it the "crown jewel"

No official designation exists, but the characterization is defensible on four grounds:

1. **Breadth.** It replaced nine separate fixed-income systems and spread to equities and beyond, giving one platform the front-office risk picture across Markets. 🟡 [^cnbc]
2. **Revenue proximity.** JPMC's own deck presents Athena beside the #1 markets-revenue franchise ($29.5B 2020 markets revenue, ~100 markets). 🟢 [^aws22]
3. **Engineering moat.** One of the world's largest Python repositories, with a deployment velocity few banks match. 🟢 [^aws22]
4. **Externalization.** It became the basis for client products, mirroring SecDB at Goldman and Aladdin at BlackRock. 🟡 [^cnbc][^ii]

---

## 2. Origin Story & Historical Evolution

### 2.1 Timeline

|Year|Event|Conf.|
|---|---|---|
|1992|SecDB developed at Goldman Sachs|🟡 [^beacon-tl]|
|1997–98|Singh (programmer) and Higgins (quant strategist) join Goldman and work on SecDB|🟡 [^beacon-tl]|
|**2006**|**Singh and Higgins move to JPMorgan and start Athena** as a cross-asset market-risk, trading and quant platform|🟡 [^beacon-tl][^beacon-blog]|
|~2010|Singh joins BofA as MD / head of global risk systems; builds **Quartz**. Higgins stays, co-heads Quantitative Research, ~8 years at JPM total|🟡 [^beacon-tl][^higgins-bio]|
|2014|Beacon Platform founded by Singh and Higgins|🟡 [^yespress]|
|2017|JPMC's _Managing Python at Scale_ talk at PyData NY|🟢 [^pydata]|
|2018|Athena-based IAP goes to external clients|🟡 [^cnbc]|
|Sep 2019|Press attention to Python 2 end-of-life vs. Athena's migration|🟡 [^techrepublic]|
|Feb 2021|Follow-up reporting: migration still incomplete|🟡 [^cloudspress]|
|2022|JPMC's AWS session: ML hedging/risk on Athena in the cloud|🟢 [^aws22]|
|30 Apr 2025|Clearwater closes its acquisition of Beacon|🟡 [^yespress]|

> [!tip] The "late 2000s / early 2010s" window The real start is **2006**. The 2010 date belongs to Quartz at BofA, which is why the origin story is easy to scramble.

### 2.2 The problem it solved

Banks historically accreted per-desk, per-asset-class systems: separate pricing, trade capture, risk and P&L stacks that couldn't be aggregated or reconciled quickly. The SecDB approach (one shared object model, one dependency graph, one code base) lets a firm compute a unified risk and P&L view, and it's widely credited with helping Goldman through 2008. 🟡 [^cnbc]

At JPMorgan, Athena consolidated nine separate fixed-income systems and later extended to equities. 🟡 [^cnbc] Commodities was an early adopter and runs trade capture, live risk and middle/back-office on the platform. 🟢 [^jobs-comm] Externally facing risk products now cover FX, Rates, Credit and Equities. 🟢 [^jobs-ext]

### 2.3 The Python 2 → 3 migration

**What's documented:**

|Fact|Detail|Conf.|
|---|---|---|
|Size (2017)|~35M lines of Python, 150,000+ modules, 500+ open-source packages, 1,500+ developers|🟢 [^pydata]|
|Size (2022)|~50M lines of code, 4K+ Python developers|🟢 [^aws22]|
|Python 2 community support ended|1 Jan 2020|🟡 [^cloudspress]|
|2019 roadmap|"Most strategic components" Python 3-compatible by end of Q1 2020; all legacy 2.7 components by Q4 2020|🟡 [^techrepublic]|
|Feb 2021|Reporting that migration was still unfinished, with completion expected around Q2 2021|🟡 [^cloudspress]|
|Today|Current status **not confirmed** in anything I found|n/a|

> [!warning] Handle the "35M lines" number carefully It is a _presentation figure from 2017_, not an audited count, and by 2022 JPMC itself cited ~50M. Python 2 end-of-life also didn't make code stop running. It meant upstream stopped shipping fixes. 🟡 [^cloudspress]

**Why an incremental migration was feasible (mix of documented and inferred):**

- Source code lives in Hydra, not a filesystem. A custom importer resolves `module @ timestamp` versions, and "SourceMarkers" exploit DB transactions and timestamps. 🟢 [^pydata]
- Changes are layered (developer layer, shared staging/UAT, production), and the runtime pairs a Python source tree with a C++/third-party "binary train" that can run old/current/new side by side. 🟢 [^pydata]
- A DB-resident importer lets a platform team apply compatibility shims or run dual-version checks centrally. ⚪
- Named pain points in the 2017 talk: open-source upgrades, API changes, changes to pickled/stored representations, numerical differences, runtime/binary dependencies. 🟢 [^pydata]

**The "cloud-native hybrid" shift** is covered in §4. Platform-engineering roles today emphasize Python versioning, dependency governance, grid/compute backbone upgrades, and canary/rollback across hybrid on-prem and public cloud. 🟢 [^jobs-plat]

---

## 3. Core Architecture & Proprietary Tech Stack

### 3.1 Layered view

```
 ── SURFACES ─────────────────────────────────────────────────────────────
    Qt desktop apps · Athena Web (tornado, HTML5, websockets, JS, WASM)
    React/TypeScript risk UIs · RaaS web UI + APIs
 ── BUSINESS COMPONENTS ─────────────────────────────────────────────────
    Deal Model · trade / risk / market-data / reference-data services
    Rates · FX · Credit · Equities · Commodities  (per-LOB libraries)
 ── COMPUTE FRAMEWORKS ──────────────────────────────────────────────────
    Pixie (DAG)   Reactive Athena (C++/Python dataflow)
    Real-Time Risk Reporting   Job scheduling   Risk Scheduler
 ── DATA & CODE STORE ───────────────────────────────────────────────────
    Hydra: globally replicated object DB — holds data AND source code
    replicas: London · New York · Tokyo
 ── RUNTIME & INFRASTRUCTURE ────────────────────────────────────────────
    CPython + C++/3rd-party binary train
    On-prem compute grid (tens of thousands of cores + GPUs, 2017)
    AWS: EKS · Auto Scaling · ElastiCache(Redis) · RDS · S3 · KMS
```

_(layer contents: 🟢 [^pydata][^aws22])_

### 3.2 Python + C++ division of labor

|Concern|Language|Evidence|
|---|---|---|
|Application layer, business logic, trade/risk apps, quant prototyping|**Python**|Commodities app layer is entirely Python 🟢 [^jobs-comm]; QR roles are Python-centric 🟢 [^jobs-qr]|
|Reactive dataflow engine|**C++ + Python**|Named so in the 2017 deck 🟢 [^pydata]|
|Third-party/binary dependencies|**C++**|"Binary train" in the runtime slide 🟢 [^pydata]|
|High-performance pricing kernels|**C++** (likely)|Credit QR roles list C++ and Python together 🟡; numerical kernels in C++ is industry-standard ⚪|
|UI|Qt (desktop), tornado/JS/WASM (web), React/TS (newer risk apps)|🟢 [^pydata][^jobs-ext]|

The pattern is the classic quant split: Python as the composition and orchestration language, with compiled code where the flops are. What's unusual about Athena is that Python is also the **systems integration language** for the entire platform, which explains the source-in-database design below.

### 3.3 Hydra: the object and code store

**Documented 🟢** [^pydata]

- Globally replicated object database, used for **data and code**.
- Python modules are stored as versioned entries (e.g., `lib.bar @ <timestamp>`) and loaded through a custom importer. Every site (LDN/NYC/TKO) holds a replica.
- Changes are **layered** so a developer's view overlays staging, which overlays production:

```
  Developer layer :        B3   C2            ← my uncommitted/branch view
  Shared staging/UAT :  A2      C2  D1
  Production :          A1  B1  C1  D1  E1
  ──────────────────────────────────────────
  Effective runtime :   A2  B3  C2  D1  E1    ← newest visible layer wins per object
```

**Community-reported 🟡** [^hn]: append-only key-value store of pickled objects, spiritually similar to Zope's ZODB. Treat this as color, not specification. The talk's own list of pain points (changes to pickled/stored representations) is consistent with it.

#### Bi-temporal modeling: what it would mean and what's actually known

> [!warning] Not publicly confirmed for Hydra The public evidence supports **transaction-time versioning** (timestamped module versions; "audit and traceability" and "version management" as platform requirements). It does not show a full valid-time + transaction-time model for trade and market data. What follows explains the concept and why a bank platform wants it.

- **Valid time** ("as-of" business date): when something was true in the world.
- **Transaction time** ("known-at" system time): when the platform learned it.

|Question|Valid time|Transaction time|
|---|---|---|
|What was the USD OIS curve at the close on 14 Mar?|14 Mar close|latest|
|What did we _report_ to the regulator for 14 Mar, as it stood that night?|14 Mar close|14 Mar night|
|Why did yesterday's P&L change after a trade amendment?|yesterday|two different knowledge points|

```python
# ILLUSTRATIVE ONLY — not Hydra's API
db.get("curves/USD.OIS", valid_at="2026-03-14", known_at="2026-03-14T23:00Z")  # as originally published
db.get("curves/USD.OIS", valid_at="2026-03-14", known_at="latest")             # after a later correction
```

Why a bank wants this ⚪: backdated trade amendments, curve/market-data restatements, reproducing a regulatory submission exactly, and explaining P&L breaks between two knowledge states.

### 3.4 The reactive dependency graph

**Documented 🟢:** Athena's foundation lists **Pixie** (a DAG) and **Reactive Athena** (a C++/Python reactive dataflow framework). Job descriptions describe a powerful Dependency Graph, a Real-Time Risk Reporting framework, a Deal Model, and a forward-propagating, event-driven graph called Reactive. [^pydata][^jobs-qr] How Pixie and Reactive interlock internally is not public; the following is the standard design this class of system uses ⚪.

```
 LEAVES (inputs)          DERIVED NODES (memoized)                      CONSUMERS
 ───────────────          ─────────────────────────                     ─────────
 OIS quotes ────────┐
                    ├──► OIS curve ──┐
 Swap quotes ───────┘                ├──► Fwd curve ──┐
 FX spot ───────────────────────────-┘                │
 Vol quotes ──► Vol surface fit ───────────────────────┼──► Trade PV ──► Position PV ──► Book P&L ──► Desk UI
 Deal Model (trade terms, from Hydra) ─────────────────┘        │
                                                                └──► Greeks (bumped copies of the subgraph)
                                                                                 └──► Risk ladder / VaR
```

**Mechanics (⚪ industry-standard):**

1. **Tick arrives** at a leaf, which marks its transitive dependents dirty. Nothing else recomputes.
2. **Pull (lazy):** a consumer asks for a value, and only dirty ancestors recompute.
3. **Push (reactive):** subscribed nodes are scheduled for recompute and fire events to UIs and downstream services.
4. **Diamond problem:** if D depends on B and C, and both depend on A, a naive propagation recomputes D twice and may expose a half-updated state. Solved with topological (height-ordered) scheduling.
5. **Early cutoff:** if a recomputed node's value is unchanged, propagation stops there.
6. **Scenario/bump overlays:** a Greek or stress is evaluated by overlaying a shocked leaf and recomputing the affected subgraph. The overlay model matters because it makes "what-if" cheap and keeps production nodes untouched.

> [!note] Greeks methodology Bump-and-reprice versus algorithmic/adjoint differentiation is not documented for Athena. Don't assume either.

### 3.5 SDLC and monorepo culture

|Practice|Detail|Conf.|
|---|---|---|
|Monolithic code base ("monorepo")|One shared tree; anyone can contribute; full visibility of the whole code base|🟢 [^pydata]|
|Continuous delivery|10–15K production changes/week (2017); ~20K/week (2022)|🟢 [^pydata][^aws22]|
|Instant global deployment|Replicated object DB makes a change globally visible|🟢 [^pydata]|
|Flow|Write code + tests → test → commit → request a "bless" → push → run|🟢 [^pydata]|
|Limited branching|Streamlines production but poorly fits some research workflows; full reproducibility requires freezing _all_ code incl. the binary train|🟢 [^pydata]|
|Developer tooling investment|Static analysis, auto-remediation, cloud-based test scheduling, dead-code/duplication detection, AI-assisted development|🟢 [^jobs-athcore]|

```
  developer ─► tests (unit + integration) ─► commit ─► "bless" gate ─► push
                                                                          │
         ┌──────────────────────── layered runtime ──────────────────────┘
         ▼
  dev layer ─► shared staging / UAT ─► production (global replicas) ─► binary train: prod-old │ prod │ prod-new
```

---

## 4. Scale, Distributed Compute & Cloud Modernization (AWS)

### 4.1 Operating scale (as JPMC reported it)

|Metric|2017 [^pydata]|2019 [^stac]|2022 [^aws22]|
|---|---|---|---|
|Lines of code|~35M|35M+|~50M|
|Python developers|1,500+|"thousands"|4K+|
|Production changes/week|10–15K|10–15K|~20K|
|Daily jobs|~270K jobs → ~1M processes|n/a|~10K _batch_ jobs/day|
|Calculations/day|n/a|n/a|~5 billion|
|Compute|tens of thousands of cores + GPUs|n/a|"50K" compute grid (unit unstated)|

> [!warning] Don't compare rows across years naively The 2017 job count and the 2022 batch-job count almost certainly use different definitions. The "50K" grid figure on the 2022 slide has no stated unit.

**Not supported by any source I found as Athena figures:** "millions of daily trades," "hundreds of petabytes." The 450+ PB number is **all of JPMC's data**, alongside 6,000+ applications. [^aws22]

### 4.2 Hybrid AWS architecture (2022 reference deployment)

> [!info] Scope JPMC presented this for the **ML hedging and risk-management** use case. It's the best public view of Athena's cloud topology but isn't necessarily the whole risk grid.

```
 ON-PREMISES (corporate data centers)                    AWS — per-LOB app accounts
 ────────────────────────────────────                    ───────────────────────────
 Trader desk / hedging portfolio                          ┌─ Athena control plane (private subnets)
 On-prem batch scheduler  ──── Direct Connect ──────────► │    EKS + Auto Scaling groups (workers, GPU nodes)
 On-prem NoSQL (system of record)                         │    ElastiCache for Redis · RDS · S3 · KMS
        │                                                 │    ALB · NAT · Route 53 · ACM · CloudWatch
        └── replicate ──► read-only NoSQL on EC2 ◄────────┘
                          (Athena "core" account)  ◄── Transit Gateway ──► each line of business's own account
```

**Documented design details 🟢** [^aws22]:

- Individual AWS account for **each line of business**, plus an Athena core account hosting a **read-only NoSQL replica** of on-prem data.
- Requirements for elastic ML training/risk: minimize end-to-end job duration, **SLA management via dynamic priorities**, support both batch and real-time, minimize resource consumption, tolerate component loss, use modern GPUs.
- Compute specifics: GPU capacity via On-Demand Capacity Reservations, **Multi-Instance GPU (MIG)** virtualization for efficiency, and a **hardware-aware Kubernetes control plane**.
- Stated next steps: certify new GPU categories, tighter autoscaling for training/inference, reserved-instance efficiency and **multi-region** support, cost allocation and per-account limits.
- Firmwide context: JPMC said at re:Invent 2020 that it uses **Lambda and EKS for risk calculations** and EMR for trading analytics. [^beer] By 2024 it described a multi-cloud strategy across three hyperscalers. 🟡 [^sa24]

**Not documented, so treat as inference ⚪:** "affinity-based" task scheduling (placing jobs near warm caches or replicated data is a natural design but isn't described publicly), and the internal algorithm of the risk scheduler beyond "dynamic priorities."

---

## 5. End-to-End Business & Quantitative Capabilities

### 5.1 Reference trade flow through a platform like Athena

```
 ① PRE-TRADE            ② CAPTURE               ③ LIVE RISK              ④ P&L
 price / structure ───► Deal Model object ───► Greeks, ladders ───────► intraday + EOD
 (curves, vol surf,     stored in Hydra,         streaming via            attribution
  model libraries)      booked to a Book         Reactive/Pixie
                                                        │
                        ⑤ VaR / stress ◄────────────────┤
                        ⑥ XVA (CVA/DVA/FVA) ◄───────────┤
                        ⑦ Regulatory & control reporting ◄─ audit trail (as-of / versions)
```

|Step|What's publicly tied to Athena|Conf.|
|---|---|---|
|Pricing & structuring|Pricing libraries, market models, cross-asset frameworks are the core QR remit|🟢 [^jobs-qr]|
|Trade capture|Deal Model; commodities does capture-to-middle/back-office entirely in Athena|🟢 [^jobs-qr][^jobs-comm]|
|Real-time Greeks|RaaS shows Delta/Gamma, Vega, Rho, IR01 and IR Gamma, spot/vol/time views and exotics|🟢 [^raas]|
|Intraday & EOD P&L|RaaS shows total and spot P&L; rates roles cover risk metrics and P&L attribution|🟢 [^raas]|
|VaR, XVA, regulatory|Standard capabilities of a front-office risk platform; Athena's public materials don't itemize them|⚪|

**Math the platform must support (⚪ general quant finance):**

$$\Delta V \approx \Delta,\delta S + \tfrac12,\Gamma,(\delta S)^2 + \mathcal{V},\delta\sigma + \Theta,\delta t + \rho,\delta r + \varepsilon_{\text{unexplained}}$$

$$\text{CVA} \approx (1-R)\sum_{i=1}^{n} \text{DF}(t_i),\text{EE}(t_i),\big[\text{PD}(t_i)-\text{PD}(t_{i-1})\big]$$

The first is the P&L-explain identity. Unexplained P&L is a control signal that something in the pricing, market data or bump setup is off. The second is the textbook unilateral CVA, where counterparty exposure profiles (EE) come from path simulation that is expensive and hence compute-grid-bound.

### 5.2 External offerings

|Offering|What clients get|Notes|
|---|---|---|
|**Risk as a Service**|Streaming and on-demand sensitivities and analytics at portfolio/position/trade level, via web UI or APIs; J.P. Morgan's intraday/EOD marks, discount curves and vol-surface construction; coverage from cash to exotic derivatives; fully hosted|🟢 [^raas]|
|**Investment Analytics Platform**|Portfolio analytics for asset owners (pensions, endowments, insurers); first external Athena-based product|🟡 [^cnbc]|
|**StatPro partnership**|StatPro's multi-asset risk/performance attribution integrated into J.P. Morgan's digital platform, fixed-income focus (2019)|🟡 [^ii]|

---

## 6. Engineering Trade-offs, Challenges & Lessons Learned

### 6.1 Scaling a Python monolith

|Pressure|Why it bites|What JPMC says/does|Conf.|
|---|---|---|---|
|Dependency upgrades|OSS API/numeric changes ripple through 10⁷ lines|Called out as a primary challenge; heavy automated testing|🟢 [^pydata]|
|Stored-object compatibility|Pickled/stored representations break when classes change|Listed as a challenge|🟢 [^pydata]|
|Reproducibility|Limited branching means history lives in versions, not branches|Freeze all code incl. binaries to reproduce|🟢 [^pydata]|
|Fear of change|Instant global deploy raises blast radius|Cited as a continuous-delivery con; mitigated by gates and tests|🟢 [^pydata]|
|Global state|Replicated DB + in-process caches can diverge|Not documented; replication-lag handling is a standard concern|⚪|

### 6.2 Cascading recalculation in the DAG ⚪

Standard failure modes and mitigations for this class of engine:

- **Fan-out storms:** one curve tick dirties thousands of positions. Mitigations: tick conflation/coalescing, bucketing, partial revaluation.
- **Glitches** (inconsistent intermediate state): height-ordered propagation.
- **Wasted work:** value-equality early cutoff, so unchanged nodes don't propagate.
- **Priority inversion:** a risk batch starving live desk updates. Mitigation: separate queues and dynamic priority (the 2022 deck lists dynamic job priorities as a requirement 🟢 [^aws22]).

### 6.3 Zero-downtime change under regulatory scrutiny

- JPMC's own KPIs for the platform include **operational risk and reliability** and **audit and traceability**. 🟢 [^aws22]
- Mechanisms documented: the **bless** gate, layered runtime promotion, a rolling **prod-old / prod / prod-new** binary train, and platform roles built around canary and rollback. 🟢 [^pydata][^jobs-plat]
- Generic regulatory context (model risk governance, books-and-records, reproducibility) shapes why version-as-of access matters. ⚪

### 6.4 Hybrid-cloud friction

|Issue|Evidence|
|---|---|
|Data gravity: the system of record stays on-prem, with a read-only replica in AWS|🟢 [^aws22]|
|GPU supply and cost: capacity reservations, MIG, cost allocation, per-account limits|🟢 [^aws22]|
|Control-plane complexity: per-LOB accounts + a core account|🟢 [^aws22]|

### 6.5 Lessons worth stealing

1. **Code can live in the database.** Storing source and data together in a replicated store makes deployment, rollback and "run the code as it was" cheap. 🟢
2. **Stability and speed aren't opposites** when delivery is automated and gated. 🟢 [^pydata]
3. **A shared platform ≠ shared knowledge.** The 2017 talk lists this explicitly; expect to invest in docs, onboarding and governance. 🟢 [^pydata]
4. **Model the graph, not the batch.** Incremental recomputation over a dependency graph beats nightly full revaluation for any workload with partial updates. ⚪
5. **Plan the language migration at platform level.** Doing it centrally (importer, shims, binary train) is what makes 35M+ lines tractable. ⚪

### 6.6 Open questions (not answerable from public sources)

- Current Python version and migration completion status.
- Hydra's actual temporal model and consistency guarantees across regions.
- Relationship between Pixie and Reactive at runtime.
- Greeks methodology (bump vs. adjoint) and per-asset-class pricing library inventory.
- Actual per-day trade counts and Athena-specific data volume.

---

## Sources

[^pydata]: M. Tselman (J.P. Morgan), _Managing Python at scale without breaking the bank_, PyData NY 2017 — https://www.slideshare.net/slideshow/managing-python-at-scale-without-breaking-the-bank/86425035 [^aws22]: P. Bauerschmidt (J.P. Morgan), _How JPMC modernized its hedging and risk management platform_, AWS Financial Services Cloud Symposium 2022 — https://pages.awscloud.com/rs/112-TZM-766/images/IN02-JPMC.pdf [^cnbc]: CNBC, 5 Nov 2018, J.P. Morgan opening Athena-based analytics to clients — https://www.cnbc.com/amp/2018/11/05/jp-morgan-selling-trading-software-in-glimpse-of-wall-streets-future.html [^beacon-blog]: M. Higgins, Beacon blog (career account: Goldman → JPM 2006) — https://www.beacon.io/?p=2594 [^beacon-bio]: K. Singh bio, Beacon (Athena, Quartz, 5,000+ Quartz developers) — https://www.beacon.io/?p=2995 [^beacon-tl]: Beacon Platform timeline (SecDB 1992 → Athena 2006 → Quartz 2010) — https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RW1bfuj [^higgins-bio]: M. Higgins speaker bio (eight years at JPM; co-headed QR) — https://meettheinnovators.splashthat.com/ [^yespress]: YesPress profile of K. Singh; Clearwater/Beacon closing 30 Apr 2025 — https://yespress.io/kirat-singh [^risk-open]: Risk.net, _JP Morgan to join Goldman in opening up risk system_ — https://www.risk.net/risk-magazine/news/2480360/jp-morgan-to-join-goldman-in-opening-up-risk-system [^techrepublic]: TechRepublic, 12 Sep 2019, Athena and Python 3 — https://www.techrepublic.com/article/jpmorgans-athena-has-35-million-lines-of-python-code-and-wont-be-updated-to-python-3-in-time/ [^cloudspress]: CloudsPress retrospective on Athena's Python 2 migration — https://www.cloudspress.com/?p=618187 [^stac]: STAC Summit, 17 Oct 2019, J.P. Morgan talk abstract — https://stacresearch.com/node/45002 [^jobs-qr]: J.P. Morgan QR "Athena Analytics Developer" posting — https://www.builtinboston.com/job/quantitative-research-athena-analytics-developer-vice-president/6320674 [^jobs-comm]: J.P. Morgan Commodities Athena engineer posting — https://builtinlondon.uk/job/software-engineer-iii-swes03-commodities-uk/6291555 [^jobs-core]: J.P. Morgan Athena Platform Core product-owner posting — https://builtinlondon.uk/job/markets-technology-athena-platform-core-product-manager/6337756 [^jobs-ext]: J.P. Morgan Athena Engineering posting (externally facing cross-asset risk) — https://builtinlondon.uk/job/lead-software-engineer/10309247 [^jobs-athcore]: J.P. Morgan "Athena Core" Python/AI engineering posting — https://careers.outforundergrad.org/companies/jpmorganchase/jobs/79551559-lead-software-engineer-python-ai-athena-core-vice-president [^jobs-plat]: J.P. Morgan Athena Platform Engineer posting — https://careers.ohsonline.com/job/software-engineer-iii-python/82914573 [^raas]: J.P. Morgan, Risk as a Service — https://www.jpmorgan.com/markets/risk-as-a-service and https://www.jpmorgan.com/solutions/cib/securities-services/risk-as-a-service [^ii]: Institutional Investor, JPMorgan/StatPro and Aladdin comparison — https://www.institutionalinvestor.com/index%2Ephp/article/2bswfsm4uqytqqdceh4ow/portfolio/jpmorgan-may-be-building-a-competitor-to-blackrocks-aladdin [^hn]: Community threads on "Bank Python" (anonymous; Hydra/Pixie/Bob naming) — https://news.ycombinator.com/item?id=29104047 and https://news.ycombinator.com/item?id=29104214 [^beer]: JPMC at AWS re:Invent 2020 (CIO L. Beer; Lambda/EKS for risk, 450 PB firmwide) — https://cloudwars.com/digital-business/the-financial-services-data-revolution-at-wells-fargo-jp-morgan [^sa24]: SiliconANGLE, July 2024, JPMorgan multi-cloud migration — https://siliconangle.com/?p=665125

---

## Vault Connections

This source-derived note is connected to [[Athena]], [[JPMorgan Chase]], [[Architecture & Workflow]], [[Claims, Contradictions & Uncertainty]], [[ADR-001 Incremental Dependency Graph]], [[ADR-002 Bitemporal Store]], [[ADR-003 Reproducible Results]]. Its claims are retained as supplied; consult [[Claims, Contradictions & Uncertainty]] for cross-source caveats.
