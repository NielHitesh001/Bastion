---
title: "Canvas Layout Guide"
aliases: ["Canvas Setup", "Visual Navigation"]
tags: ["source-derived", "finance-tech", "knowledge-graph"]
status: complete
type: source-derived-note
source_file: "Canvas_Layout_Guide.md"
related: ["[[FIN Tech MOC Index]]", "[[Graph Connectivity Report]]"]
---
# Obsidian Canvas Layout Guide

## Overview

This guide explains how to create an interactive **Obsidian Canvas** (`.canvas` file) to visualize the relationships between financial institutions, proprietary platforms, and third-party vendors.

---

## Step 1: Create a New Canvas

1. **In Obsidian**, right-click on your vault folder → **New Canvas**
2. Name it: `Financial_Tech_MOC_Canvas.canvas`
3. Obsidian will generate a `.canvas` JSON file

---

## Step 2: Canvas Node Types

Use different node colors and shapes to distinguish institution types:

| Type | Color | Shape | Example |
|------|-------|-------|---------|
| Investment Bank | 🔵 Blue | Rectangle | [[JPMorgan Chase]] |
| Quant Fund | 🟪 Purple | Rectangle | [[Jane Street]] |
| Asset Manager | 🟩 Green | Rectangle | [[BlackRock]] |
| Proprietary System | 🟨 Yellow | Rounded | [[Athena]], [[SecDB]] |
| Third-Party Vendor | 🟧 Orange | Diamond | [[Murex MX3]], [[Charles River]] |
| Market Data Provider | 🔴 Red | Oval | [[Bloomberg Terminal]] |
| Technology Concept | ⚪ Gray | Circle | "OCaml", "FPGA", "Kubernetes" |

---

## Step 3: Canvas Structure (Hierarchical Layout)

### **Recommended Layout**: Multi-Tier Horizontal Arrangement

```
┌─────────────────────────────────────────────────────────────────┐
│                  FINANCIAL INSTITUTIONS (LEFT TIER)             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  [JPMorgan Chase]    [Goldman Sachs]    [BofA]    [Morgan Stanley]
│         │                   │                │              │
│         ↓                   ↓                ↓              ↓
│
│
│  [BlackRock]         [Vanguard]        [State Street]    [Fidelity]
│         │                   │                │              │
│         ↓                   ↓                ↓              ↓
│
│
│  [Jane Street]       [Citadel]         [Two Sigma]    [D.E. Shaw]
│
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│           PROPRIETARY PLATFORMS (MIDDLE TIER)                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  [Athena] [SecDB] [Quartz] [Strateger] [Aladdin] [JaneFlow]     │
│
│  [Gridlock] [Vega] [Alpha] [Proprietary Stacks]                 │
│
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│    THIRD-PARTY VENDORS & DATA (RIGHT TIER)                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│  [Murex MX3]  [Calypso]  [Charles River]  [ION Trading]        │
│
│  [Bloomberg TOMP]  [FlexTrade]  [Bloomberg Terminal]           │
│
│  [FactSet]  [Refinitiv Eikon]  [S&P Capital IQ]               │
│
└─────────────────────────────────────────────────────────────────┘
```

---

## Step 4: Connection Patterns (Edges)

### Edge Styling by Integration Type

| Integration Type | Line Style | Color | Example |
|------------------|-----------|-------|---------|
| Primary Platform | Solid (thick) | Dark | JPMorgan → Athena |
| Secondary Integration | Solid (thin) | Medium | JPMorgan → Bloomberg TOMP |
| Legacy/Declining | Dashed | Gray | Citi → Kondor+ |
| Multi-Vendor | Multiple lines | Multi-color | BofA → Murex, BloombergTOMP |
| Data Flow | Arrow (directional) | Blue | Bloomberg Terminal → Athena |
| Technology Dependency | Curved | Teal | Athena → Python, Kubernetes |

---

## Step 5: Sub-Graphs (Clusters)

Create separate canvas sections for detailed views:

### **Canvas 1: Main MOC**
- High-level overview of all institutions and platforms
- Simplified connections (only primary integrations)
- Use for navigation and broad understanding

### **Canvas 2: Tier-1 Banks Deep Dive**
- Focus: JPMorgan, Goldman Sachs, BofA, Morgan Stanley, Citi
- Include: Proprietary systems, third-party integrations, data flows
- Add tech stack details (Python, C++, Java, proprietary DSLs)

### **Canvas 3: Quant Funds & Market Makers**
- Focus: Jane Street, Citadel, Two Sigma, DE Shaw, Optiver, Virtu
- Include: Custom infrastructure, FPGA/hardware, execution engines
- Add latency targets, strategies

### **Canvas 4: Vendor Ecosystem**
- Focus: Murex, Calypso, Charles River, ION, Bloomberg, FactSet
- Include: Which institutions use which vendors
- Add licensing models, adoption trends

### **Canvas 5: Technology Deep Dive**
- Focus: Programming languages, frameworks, infrastructure
- Include: OCaml (Jane Street), Slang (Goldman), Python (JPM, BofA)
- Add: Kubernetes, Kafka, FPGA, cloud providers

---

## Step 6: Example Canvas File Structure (JSON)

Here's what a minimal `.canvas` file looks like:

```json
{
  "nodes": [
    {
      "id": "1",
      "type": "file",
      "file": "JPMorgan_Chase.md",
      "x": 0,
      "y": 0,
      "width": 250,
      "height": 60
    },
    {
      "id": "2",
      "type": "file",
      "file": "Athena.md",
      "x": 300,
      "y": 0,
      "width": 250,
      "height": 60
    },
    {
      "id": "3",
      "type": "file",
      "file": "Third_Party_Vendors.md",
      "x": 600,
      "y": 0,
      "width": 250,
      "height": 60
    }
  ],
  "edges": [
    {
      "id": "edge1",
      "fromNode": "1",
      "toNode": "2",
      "label": "Primary platform"
    },
    {
      "id": "edge2",
      "fromNode": "1",
      "toNode": "3",
      "label": "Secondary integrations"
    }
  ]
}
```

---

## Step 7: Navigation Tips

### For Exploring Investment Banks:
1. Start at main MOC
2. Click on institution (e.g., [[JPMorgan Chase]])
3. Navigate to proprietary system (e.g., [[Athena]])
4. Explore tech stack (Python, C++, Kubernetes)
5. Jump to third-party integrations (Bloomberg, FactSet)

### For Exploring Quant Funds:
1. Start at Quant Funds cluster
2. Click on fund (e.g., [[Jane Street]])
3. Explore [[OCaml]] technology stack
4. View [[JaneFlow]] execution engine
5. Compare with other quant funds (Citadel, Two Sigma)

### For Exploring Vendor Ecosystem:
1. Start at Vendors canvas
2. Click on vendor (e.g., [[Murex MX3]])
3. See which institutions use it
4. Compare with competing vendors (Calypso, Beacon)
5. Review licensing & adoption trends

---

## Step 8: Interactive Features (in Obsidian)

### Quick Actions:
- **Double-click node** → Open the linked file
- **Right-click node** → Add label, change color, delete
- **Drag node** → Move around canvas
- **Drag edge handle** → Create new connection
- **Zoom in/out** → Mouse wheel or trackpad gesture
- **Pan** → Hold space + drag, or middle-mouse drag

### Search & Filter:
- Use Obsidian's search within canvas
- Filter by tag: `#firm/investment-bank`, `#tech/proprietary`
- Create separate canvases for different views

---

## Step 9: Visual Color Coding

### Institution Colors:
| Institution Type | Hex Color | Obsidian Color |
|------------------|-----------|----------------|
| Investment Bank | #4A90E2 | Blue |
| Quant Fund | #7B68EE | Purple |
| Asset Manager | #50C878 | Green |

### Platform Colors:
| Platform Type | Hex Color | Obsidian Color |
|---------------|-----------|----------------|
| Proprietary System | #FFD700 | Yellow |
| Vendor Software | #FF8C00 | Orange |
| Market Data | #DC143C | Red |
| Infrastructure | #A9A9A9 | Gray |

---

## Step 10: Example Canvas Workflows

### Workflow 1: "Comparing Trading Platforms"
1. Start with [[FIN_TECH_MOC_INDEX]]
2. Open Tier-1 Banks canvas
3. Compare [[Athena]] (JPM) vs [[SecDB]] (Goldman) vs [[Quartz]] (BofA)
4. View tech stack differences (Python vs. Slang vs. Python)
5. Review latency targets, architecture, risk capabilities

### Workflow 2: "Exploring Quant Fund Tech Stacks"
1. Open Quant Funds canvas
2. Compare [[Jane Street]] (OCaml) vs [[Citadel]] (C++/FPGA) vs [[Two Sigma]] (Python)
3. Review execution engines, research platforms, ML infrastructure
4. Analyze language choices and tradeoffs

### Workflow 3: "Vendor Selection for New Institution"
1. Open Vendor Ecosystem canvas
2. View adoption by institution type (buy-side vs. sell-side)
3. Compare [[Charles River]] vs [[ION Trading]] for OMS/EMS
4. Review licensing models, implementation timelines
5. Check references (who else uses this vendor)

---

## Step 11: Best Practices

### ✅ DO:
- Use consistent naming conventions for files (e.g., `[[Firm Name]]`, `[[Platform Name]]`)
- Color-code by category (firm type, platform type, technology)
- Add descriptive labels to edges ("Primary platform", "Secondary integration")
- Create multiple canvases for different abstraction levels
- Keep nodes roughly same size for visual consistency
- Arrange spatially by category (left: firms, middle: platforms, right: vendors)

### ❌ DON'T:
- Overcrowd single canvas (keep <20 nodes for readability)
- Mix multiple abstraction levels on same canvas
- Use ambiguous edge labels
- Create too many colors (limit to 5-6 per category)
- Forget to update canvas when adding new notes

---

## Step 12: Example Canvas Arrangements

### Arrangement 1: Linear Flow (Left → Right)

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│ Institution │ --> │  Proprietary│ --> │   Vendors   │
│             │     │   Platform  │     │             │
└─────────────┘     └─────────────┘     └─────────────┘
      (Data)             (Processing)        (Output)
```

### Arrangement 2: Hierarchical (Top → Bottom)

```
        ┌──────────────┐
        │  Tier-1 Bank │
        └───────┬──────┘
                │
    ┌───────────┼───────────┐
    ▼           ▼           ▼
┌────────┐ ┌────────┐ ┌────────┐
│Platform│ │Vendor-1│ │Vendor-2│
└────────┘ └────────┘ └────────┘
```

### Arrangement 3: Radial (Hub & Spoke)

```
                ┌─────────────┐
                │  Murex MX3  │
                └─────────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
    ┌────────┐   ┌────────┐   ┌────────┐
    │  Citi  │   │  BofA  │   │Deutsche│
    └────────┘   └────────┘   └────────┘
```

---

## Step 13: Advanced Canvas Features

### Dataview Integration:
Embed Dataview queries in canvas nodes to auto-populate vendor lists:

```dataview
TABLE WITHOUT ID
  file.link as Vendor,
  clients_count as "# of Clients",
  category as Category
WHERE contains(tags, "#vendor")
SORT clients_count DESC
```

### Timeline Canvas:
Create a timeline of platform launches and migrations:

```
1990s:  SecDB (Goldman) launches
2000s:  Murex MX3, Charles River established
2010s:  Calypso, Beacon, ION Trading emerge
2020s:  Cloud-native platforms (Marquee, modern alternatives)
```

---

## Step 14: Exporting & Sharing

### Export Canvas as Image:
1. In Obsidian Canvas, click ⋮ menu → Export as PNG
2. Share in presentations, documentation, wikis

### Export Canvas as PDF:
1. Use browser print-to-PDF on canvas (Ctrl+P)
2. Save as shareable document

### Share Canvas Link:
1. In Obsidian, right-click canvas → Copy URL
2. Share with team members (they need Obsidian vault access)

---

## 📚 Canvas Files to Create

| Canvas Name | Focus | Nodes | Purpose |
|-------------|-------|-------|---------|
| **Main MOC** | Overview | 30+ | Navigation hub |
| **Tier-1 Banks** | Investment banks | 20 | Deep dive on big 4 + tier-1 |
| **Quant Funds** | Quant shops | 15 | Compare OCaml, C++, Python |
| **Vendors** | Third-party | 25 | Vendor ecosystem, adoption |
| **Tech Stack** | Technology | 20 | Languages, frameworks, infra |
| **Timeline** | History | 15 | Platform launches, evolution |

---

## 🎯 Quick Start Checklist

- [ ] Create main canvas: `Financial_Tech_MOC_Canvas.canvas`
- [ ] Add 5-10 nodes (start with JPMorgan, Goldman, BofA, BlackRock, Jane Street)
- [ ] Connect with edges (platform relationships)
- [ ] Color-code by institution type
- [ ] Add sub-canvas for vendors (Murex, Calypso, Charles River)
- [ ] Add sub-canvas for tech stacks (OCaml, Python, C++, Slang)
- [ ] Test navigation (click nodes, open linked files)
- [ ] Export as image for presentations

---

**Last Updated**: 2026-09-16 | **Status**: Ready for implementation | **Difficulty**: Intermediate

---

## Vault Connections

This source-derived note is connected to [[FIN Tech MOC Index]], [[Graph Connectivity Report]]. Its claims are retained as supplied; consult [[Claims, Contradictions & Uncertainty]] for cross-source caveats.
