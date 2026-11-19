---
title: "ADR-003 Reproducible Results"
aliases: ["ADR 003 Results Provenance"]
tags: ["adr", "architecture", "reproducibility"]
status: complete
type: architecture-decision-record
related: ["[[Architecture & Workflow]]", "[[ADR-001 Incremental Dependency Graph]]", "[[ADR-002 Bitemporal Store]]"]
---

# ADR-003 Reproducible Results

## Status

Proposed design decision for the Mini-Athena reference architecture.

## Decision

Attach a provenance key to every result: code version, input dataset/version, valid time, known time, model configuration, and graph snapshot. Re-runs must return or explicitly explain a different result.

## Evidence Boundary

This is directly aligned with the stated design principles in [[Architecture & Workflow]]. It is an implementation choice for a reference system, not a claim about any firm's production controls.
