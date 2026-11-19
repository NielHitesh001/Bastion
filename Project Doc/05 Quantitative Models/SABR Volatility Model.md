---
title: "SABR Volatility Model"
aliases: ["SABR_Volatility_Model"]
tags: ["quant-finance", "model", "status/researched"]
type: model
status: researched
revised: "2026-11-19"
related: ["[[Architecture & Workflow]]", "[[Athena Detailed Report]]", "[[ADR-003 Reproducible Results]]"]
---
# SABR Volatility Model

> [!abstract] SABR is a stochastic-volatility model commonly used to interpolate/extrapolate implied-volatility smiles. It answers how sparse option quotes become a coherent surface.

> [!note] Confidence legend: 🟢 primary/regulatory or project documentation; 🟡 published practitioner material; ⚪ implementation inference.

## TL;DR

- 🟢 It addresses **smile interpolation** in an Athena-style pricing and risk graph.
- 🟢 The model contract requires versioned market inputs and conventions.
- 🟡 Numerical choices determine stability, latency, and explainability.
- 🟢 Independent oracle tests are required before optimization.
- ⚪ Live use needs cached/incremental evaluation; batch use needs isolated as-of replay.

## Intuition and math

SABR is a stochastic-volatility model commonly used to interpolate/extrapolate implied-volatility smiles. It answers how sparse option quotes become a coherent surface.

$$dF_t=alpha_tF_t^eta dW_t,qquad dalpha_t=
ualpha_t dZ_t,quad dW_tdZ_t=ho dt$$

Assumptions must be explicit in the result provenance; a correct equation under the wrong market convention is not a correct result.

## Inputs, calibration and numerical method

**Inputs:** market smile quotes, beta policy, shift, alpha/rho/nu calibration, expiry and strike.

Calibrate or bootstrap against market observables, retain residuals and conventions, and reject stale or internally inconsistent inputs. Complexity is governed by the instrument count, market-factor dimension, scenario count, and whether calibration is nested inside risk.

## Sensitivities and model risk

For smooth deterministic pricers, central bumps provide a transparent baseline. Algorithmic/adjoint differentiation is preferable when many sensitivities are required, but it differentiates the implemented path and does not remove discontinuities, policy choices, or calibration risk. Key failure modes are convention mismatch, extrapolation, stale data, invalid curves/surfaces, unstable calibration, and aggregation across incompatible as-of times.

## Platform placement

This is a graph node beneath [[Architecture & Workflow]]. In live mode it consumes versioned events and caches valid dependencies; in batch mode it runs under an immutable as-of context. Target module: `core/pricing`. Interface: `calibrate_sabr(smile, convention) -> Surface`. Validate against QuantLib where applicable, then add properties such as monotonicity, parity, zero-position, and replay determinism.

## Illustrative Python (not Athena implementation)

```python
# Illustrative only: make provenance an explicit input.
def evaluate(deal, market, model):
    assert market.as_of is not None
    value = model.price(deal, market)
    return {"value": value, "as_of": market.as_of, "model": model.version}

# Oracle tests compare this contract to an independent library.
```

## Comparison and context

Calibration is ill-conditioned with sparse quotes; constrain parameters, retain residuals, and validate against price-space errors. See [[Deal Model]], [[Yield Curve Construction]], [[Monte Carlo Pricing]], [[Greeks: Bump and Adjoint Differentiation]], and [[Exposure, CVA and XVA]] for adjacent concerns.

## Open questions

- Which conventions and regulatory versions are in scope?
- What latency, accuracy, and explainability budgets govern live use?
- Which independent oracle and tolerances are accepted for each product?

## Sources

[^q1]: 🟢 [QuantLib documentation](https://www.quantlib.org/)
[^q2]: 🟢 [Basel Framework / market-risk materials](https://www.bis.org/bcbs/basel3/b3_risk.htm)
[^q3]: 🟢 [ISDA SIMM methodology](https://www.isda.org/a/DRfgE/ISDA-SIMM_v2.72412_PUBLIC.pdf)
[^q4]: 🟡 [Black and Scholes (1973)](https://www.jstor.org/stable/1831029)
