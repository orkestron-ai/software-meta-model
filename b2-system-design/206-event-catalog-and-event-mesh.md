<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 206
layer_key: event_catalog_and_event_mesh
document_id: spec.b2.event_catalog_and_event_mesh
document_type: layer_specification
module_scope: b2
status: accepted
spec_version: 2.0.0
title: Event Catalog and Event Mesh Layer Specification
<!-- AISMM:META_END -->

# 206 — Event Catalog and Event Mesh

## 1. Purpose

Model event-driven architecture with a first-class event catalog, producers, consumers, and contracts.

---

## 2. Layer Role in AISMM

This layer extends Bundle B2 with event catalog and event mesh modeling capabilities, enabling AI agents and humans to reason over this domain within the full AISMM graph.

---

## 3. Main Output

Structured event catalog and event mesh entities with stable IDs, typed relationships, and cross-layer links.

---

## 4. Identifiable Entities

```text
domain_event.*
integration_event.*
event_schema.*
event_topic.*
event_producer.*
event_consumer.*
event_contract.*
event_version.*
event_mesh.*
event_retention_policy.*
```

---

## 5. Required Structure

Each entity must include:

- `id` following the AISMM ID strategy: `event.<product>.<name>`
- `name` or `title`
- Status field where applicable
- Cross-layer links to related entities

---

## 6. Cross-Layer Links

Entities in this layer link to other AISMM bundles as appropriate. See the bundle README for the full cross-layer link map.

---

## 7. Summary

Layer 206 (Event Catalog and Event Mesh) extends the AISMM model with event catalog and event mesh, enabling full product knowledge coverage for AI-native systems.
