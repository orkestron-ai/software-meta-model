<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1006
layer_key: drift_monitoring_and_retraining
document_id: spec.data_ai.drift_monitoring
document_type: layer_specification
module_scope: b10
status: accepted
spec_version: 2.0.0
title: Drift, Monitoring and Retraining Layer Specification
<!-- AISMM:META_END -->

# 1006 — Drift, Monitoring and Retraining

## 1. Purpose

Detect and respond to **data and model drift** with explicit signals, checks, and retraining triggers.

This layer answers whether a model is still performing as expected, how drift is detected, and what causes a retraining cycle to start.

---

## 2. Layer Role in AISMM

This layer closes the **operational feedback loop** for models (b10.1004): it monitors deployed versions, raises drift and regression signals, and defines the triggers that lead back into training (b10.1005).

---

## 3. Main Output

Drift signals and performance regressions detected by monitors and checks, with the retraining triggers they activate.

---

## 4. Core Concepts

**drift_signal** — An observation that input data or predictions have shifted from a baseline.

**drift_check** — A scheduled or continuous check that evaluates drift conditions.

**retraining_trigger** — A rule that starts a retraining cycle when conditions are met.

**model_monitor** — A monitor observing a model version's live behavior and metrics.

**performance_regression** — A detected drop in a model version's quality against expectations.

---

## 5. Identifiable Entities

```text
drift_signal.*
drift_check.*
retraining_trigger.*
model_monitor.*
performance_regression.*
```

---

## 6. Required Structure

```yaml
model_monitor:
  id: model_monitor.<model_version>
  model_version: model_version entity_id
  tracked_metrics: [string]

drift_check:
  id: drift_check.<model_version>.<name>
  monitor: model_monitor entity_id
  method: string                    # e.g. PSI, KL-divergence
  threshold: number

drift_signal:
  id: drift_signal.<model_version>.<n>
  model_version: model_version entity_id
  drift_check: drift_check entity_id
  severity: low | medium | high

performance_regression:
  id: performance_regression.<model_version>.<n>
  metric: metric entity_id
  baseline: number
  observed: number

retraining_trigger:
  id: retraining_trigger.<model>.<name>
  condition: string                 # references drift_signal / performance_regression
  action: string
```

---

## 7. Cross-Layer Links

- `b10.1004` — model versions being monitored
- `b10.1005` — training and evaluation cycles started by retraining triggers
- `b6.602` — observability and monitoring (ML monitoring specializes it)
- `b7.702` — risk management (drift is an operational model risk)

---

## 8. Summary

Layer 1006 keeps models **honest in production**, turning drift and regression from silent failures into explicit signals with defined retraining responses.

<!-- AISMM:END -->
