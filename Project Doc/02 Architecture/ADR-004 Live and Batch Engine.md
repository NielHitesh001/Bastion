---
title: "ADR-004 Live and Batch Engine"
aliases: ["ADR 004 Unified Engine"]
tags: ["adr", "architecture", "execution"]
status: complete
type: architecture-decision-record
related: ["[[Architecture & Workflow]]", "[[ADR-001 Incremental Dependency Graph]]", "[[ADR-003 Reproducible Results]]"]
---

# ADR-004 Live and Batch Engine

## Status

Proposed design decision for the Mini-Athena reference architecture.

## Decision

Use one pure calculation core in both live and batch modes. Stateful services own subscriptions and cached graph state; batch workers construct an isolated as-of evaluation context.

## Rationale

This directly follows the two-mode, one-engine principle in [[Architecture & Workflow]]. It limits business-logic divergence while allowing distinct operational behavior.
