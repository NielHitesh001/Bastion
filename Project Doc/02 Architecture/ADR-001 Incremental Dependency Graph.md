---
title: "ADR-001 Incremental Dependency Graph"
aliases: ["ADR 001 Dependency Graph"]
tags: ["adr", "architecture", "dependency-graph"]
status: complete
type: architecture-decision-record
related: ["[[Architecture & Workflow]]", "[[Athena Detailed Report]]", "[[ADR-003 Reproducible Results]]"]
---

# ADR-001 Incremental Dependency Graph

## Status

Proposed design decision, derived from [[Architecture & Workflow]]; it is not asserted to describe JPMorgan's proprietary implementation.

## Context

The Mini-Athena design requires live updates and batch evaluation from the same core. [[Athena Detailed Report]] describes a reactive dependency graph as public evidence for a related architectural idea, while distinguishing verified facts from inference.

## Decision

Model calculations as a directed acyclic graph with explicit input versions. On a change, invalidate only downstream nodes; evaluate affected nodes in dependency order. Persist a graph snapshot identifier with each result.

## Consequences

- Supports incremental risk recomputation and auditability.
- Requires cycle detection, deterministic topological ordering, and bounded invalidation.
- Does not prove that any named commercial or proprietary platform uses this exact model.
