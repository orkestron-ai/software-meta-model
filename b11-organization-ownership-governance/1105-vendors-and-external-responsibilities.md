<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1105
layer_key: vendors_and_external_responsibilities
document_id: spec.org.vendors
document_type: layer_specification
module_scope: b11
status: accepted
spec_version: 2.0.0
title: Vendors and External Responsibilities Layer Specification
<!-- AISMM:META_END -->

# 1105 — Vendors and External Responsibilities

## 1. Purpose

Model vendor relationships, external responsibilities, and support contracts — connecting external parties to AISMM entities.

## 2. Identifiable Entities

```text
vendor.*
supplier.*
external_responsibility.*
support_contract.*
dependency_owner.*
```

## 3. Required Structure

```yaml
vendor:
  id: vendor.<name>
  name: string
  type: cloud_provider | saas | professional_services | open_source_steward | hardware
  support_contracts: [support_contract entity_id]
  owned_external_systems: [entity_id]

support_contract:
  id: support_contract.<vendor>.<scope>
  vendor: vendor entity_id
  sla_tier: string
  valid_from: ISO date
  valid_to: ISO date or null
