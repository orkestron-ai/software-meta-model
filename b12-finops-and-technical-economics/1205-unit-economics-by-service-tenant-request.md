<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1205
layer_key: unit_economics_by_service_tenant_request
document_id: spec.finops.unit_economics_by_service_tenant_request
document_type: layer_specification
module_scope: b12
status: accepted
spec_version: 2.0.0
title: Unit Economics by Service, Tenant and Request Layer Specification
<!-- AISMM:META_END -->

# 1205 — Unit Economics by Service, Tenant and Request

## 1. Purpose

Model unit economics by service, tenant and request as first-class AISMM entities for technical financial observability.

## 2. Identifiable Entities

```text
service_unit_cost.*
tenant_unit_cost.*
request_unit_cost.*
cost_driver.*
margin_signal.*
```

## 3. Cross-Layer Links

- `b0.006` — product economics
- `b6.601` — runtime instances (cost sources)
- `b7.702` — financial risks
- `b8.804` — releases (cost attribution)
- `b10.1004` — model inference endpoints
- `b11.1102` — ownership and cost responsibility

## 4. Summary

Layer 1205 provides structured modeling of unit economics by service, tenant and request for AI-native systems in AISMM.
