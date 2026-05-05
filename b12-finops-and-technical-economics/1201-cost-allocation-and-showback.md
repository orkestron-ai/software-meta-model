<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1201
layer_key: cost_allocation_and_showback
document_id: spec.finops.cost_allocation_and_showback
document_type: layer_specification
module_scope: b12
status: accepted
spec_version: 2.0.0
title: Cost Allocation and Showback Layer Specification
<!-- AISMM:META_END -->

# 1201 — Cost Allocation and Showback

## 1. Purpose

Model cost allocation and showback as first-class AISMM entities for technical financial observability.

## 2. Identifiable Entities

```text
cost_center.*
cost_allocation.*
showback_report.*
chargeback_rule.*
```

## 3. Cross-Layer Links

- `b0.006` — product economics
- `b6.601` — runtime instances (cost sources)
- `b7.702` — financial risks
- `b8.804` — releases (cost attribution)
- `b10.1004` — model inference endpoints
- `b11.1102` — ownership and cost responsibility

## 4. Summary

Layer 1201 provides structured modeling of cost allocation and showback for AI-native systems in AISMM.
