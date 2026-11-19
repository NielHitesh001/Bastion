---
title: "Bitemporal Data"
aliases: []
tags: ["reference", "architecture", "status/complete"]
status: complete
type: reference-node
related: ["[[Vault Audit & Gap Report]]"]
---
# Bitemporal Data

## Role in this vault

Bitemporal modeling records both the business-effective time of a fact and the time at which the system knew/recorded it.

## Mechanics and design considerations

It supports corrections and as-of replay, but requires inclusive/exclusive interval conventions, immutable history, late-arrival policy, and query APIs that carry both temporal coordinates.

## Incoming contexts

- [[Vault Audit & Gap Report]]

## Evidence boundary

This node was researched from an authoritative public product or project page: [source](). Claims in the supplied source notes may be broader, historical, or different; retain their local qualification.

## Navigation

Return to [[00_Index]] and [[Vault Audit & Gap Report]].
