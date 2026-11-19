---
title: "ADR-002 Bitemporal Store"
aliases: ["ADR 002 Bitemporal Data"]
tags: ["adr", "architecture", "data"]
status: complete
type: architecture-decision-record
related: ["[[Architecture & Workflow]]", "[[Athena Detailed Report]]", "[[ADR-003 Reproducible Results]]"]
---

# ADR-002 Bitemporal Store

## Status

Proposed design decision for the Mini-Athena reference architecture.

## Context

[[Architecture & Workflow]] specifies results by valid time and known time. [[Athena Detailed Report]] explicitly says that full bitemporal modeling for Hydra is an inference rather than publicly confirmed fact.

## Decision

Store both valid-time and system/known-time ranges for canonical business objects. Queries must accept an as-of context; corrections append a new version instead of overwriting a historical record.

## Consequences

- Enables restatement, replay, and reproducible valuation.
- Adds schema and query complexity.
- This vault does not represent the decision as evidence of Athena's internal data model.
