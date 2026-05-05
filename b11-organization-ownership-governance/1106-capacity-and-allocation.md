<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1106
layer_key: capacity_and_allocation
document_id: spec.org.capacity
document_type: layer_specification
module_scope: b11
status: accepted
spec_version: 2.0.0
title: Capacity and Allocation Layer Specification
<!-- AISMM:META_END -->

# 1106 — Capacity and Allocation

## 1. Purpose

Track team capacity, allocation, load, reservations, and forecasts to support delivery planning and risk management.

## 2. Identifiable Entities

```text
capacity.*
allocation.*
team_load.*
reservation.*
forecast.*
```

## 3. Required Structure

```yaml
capacity:
  id: capacity.<team>.<period>
  team: team entity_id
  period: string  # e.g. 2026-Q2
  total_capacity: float  # person-weeks
  available_capacity: float

allocation:
  id: allocation.<team>.<initiative>.<period>
  team: team entity_id
  initiative: entity_id  # b8 work item, project, or stream
  allocated_capacity: float
  period: string
