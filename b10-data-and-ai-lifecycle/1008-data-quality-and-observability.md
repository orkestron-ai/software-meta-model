<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1008
layer_key: data_quality_and_observability
document_id: spec.data_ai.data_quality
document_type: layer_specification
module_scope: b10
status: accepted
spec_version: 2.0.0
title: Data Quality and Observability Layer Specification
<!-- AISMM:META_END -->

# 1008 — Data Quality and Observability

## 1. Purpose

Measure the **quality and reliability of data** with quality rules, metrics, and observability signals.

This layer answers whether a data asset is reliable enough to use, which rules define "good", and how quality incidents are recorded.

---

## 2. Layer Role in AISMM

This layer provides the **quality signal** that data products (b10.1001) and features (b10.1003) depend on. It turns data reliability into measurable rules, metrics, and incidents rather than tribal knowledge.

---

## 3. Main Output

Data quality rules and metrics, observability signals over data assets, and recorded data incidents.

---

## 4. Core Concepts

**data_quality_rule** — A defined expectation a data asset must satisfy (e.g. non-null, range, uniqueness).

**data_quality_metric** — A measured value of a quality dimension (completeness, accuracy, freshness).

**data_observability_signal** — A monitored signal over a data asset (volume, latency, anomaly).

**data_incident** — A recorded event where data quality or availability was breached.

---

## 5. Identifiable Entities

```text
data_quality_rule.*
data_quality_metric.*
data_observability_signal.*
data_incident.*
```

---

## 6. Required Structure

```yaml
data_quality_rule:
  id: data_quality_rule.<asset>.<name>
  name: string
  asset: entity_id
  dimension: completeness | accuracy | consistency | freshness | validity | uniqueness
  expectation: string

data_quality_metric:
  id: data_quality_metric.<asset>.<name>
  name: string
  rule: data_quality_rule entity_id
  value: number

data_observability_signal:
  id: data_observability_signal.<asset>.<name>
  asset: entity_id
  signal_type: string               # volume, latency, schema, anomaly

data_incident:
  id: data_incident.<asset>.<n>
  title: string
  asset: entity_id
  severity: low | medium | high | critical
  status: open | mitigated | resolved
```

---

## 7. Cross-Layer Links

- `b10.1001` — data products whose quality and SLAs this layer measures
- `b6.602` — observability and monitoring (data observability specializes it)
- `b6.603` — incident management and response (data incidents feed it)
- `b7.702` — risk management (data quality risk)

---

## 8. Summary

Layer 1008 makes **data reliability explicit and measurable**, providing the quality signal every downstream data product, feature, and model depends on.

<!-- AISMM:END -->
