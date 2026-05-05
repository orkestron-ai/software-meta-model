<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1206
layer_key: carbon_and_sustainability_accounting
document_id: spec.finops.carbon_and_sustainability_accounting
document_type: layer_specification
module_scope: b12
status: accepted
spec_version: 2.0.0
title: Carbon and Sustainability Accounting Layer Specification
<!-- AISMM:META_END -->

# 1206 — Carbon and Sustainability Accounting

## 1. Purpose

Model carbon and sustainability accounting as first-class AISMM entities for technical financial observability.

## 2. Identifiable Entities

```text
carbon_metric.*
energy_usage.*
co2e_estimate.*
sustainability_policy.*
```

## 3. Cross-Layer Links

- `b0.006` — product economics
- `b6.601` — runtime instances (cost sources)
- `b7.702` — financial risks
- `b8.804` — releases (cost attribution)
- `b10.1004` — model inference endpoints
- `b11.1102` — ownership and cost responsibility

## 4. Summary

Layer 1206 provides structured modeling of carbon and sustainability accounting for AI-native systems in AISMM.
