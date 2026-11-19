---
title: "FpML"
aliases: ["Financial products Markup Language", "Financial_Products_Markup_Language"]
tags: ["reference", "standard", "status/complete"]
type: concept
status: researched
revised: "2026-11-19"
related: ["[[Deal Model]]", "[[Architecture & Workflow]]"]
---
# FpML

> [!abstract] Financial products Markup Language is an industry messaging standard for describing OTC derivatives and related post-trade processes; it is a useful interoperability reference, not a prescribed internal deal-model design.

## Role in the vault

[[Deal Model]] uses FpML as an external vocabulary and lifecycle reference. An Athena-style platform should translate external messages into a versioned canonical domain model, retain the source message and validation result, and never make external schema structure the sole internal pricing contract.

## Open questions

- Which message versions and product families are in scope?
- How will source-document, confirmation, and internal-economic representations be reconciled?

## Sources

[^fpml]: 🟢 [FpML official site](https://www.fpml.org/).
