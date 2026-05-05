<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1204
layer_key: cost_of_quality_and_incident
document_id: spec.finops.cost_of_quality_and_incident
document_type: layer_specification
module_scope: b12
status: accepted
spec_version: 2.0.0
title: Cost of Quality and Incident Layer Specification
<!-- AISMM:META_END -->

# 1204 — Cost of Quality and Incident

## 1. Purpose

Model cost of quality and incident as first-class AISMM entities for technical financial observability.

## 2. Identifiable Entities

```text
cost_of_quality.*
cost_of_incident.*
defect_cost.*
downtime_cost.*
rework_cost.*
```

## 3. Cross-Layer Links

- `b0.006` — product economics
- `b6.601` — runtime instances (cost sources)
- `b7.702` — financial risks
- `b8.804` — releases (cost attribution)
- `b10.1004` — model inference endpoints
- `b11.1102` — ownership and cost responsibility

## 4. Summary

Layer 1204 provides structured modeling of cost of quality and incident for AI-native systems in AISMM.
