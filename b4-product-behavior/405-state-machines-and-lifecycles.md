<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 405
layer_key: state_machines_and_lifecycles
document_id: spec.b4.state_machines_and_lifecycles
document_type: layer_specification
module_scope: b4
status: accepted
spec_version: 2.0.0
title: State Machines and Lifecycles Layer Specification
<!-- AISMM:META_END -->

# 405 — State Machines and Lifecycles

## 1. Purpose

Model explicit state machines and entity lifecycles for safe AI-assisted behavior modification.

---

## 2. Layer Role in AISMM

This layer extends Bundle B4 with state machines and lifecycles modeling capabilities, enabling AI agents and humans to reason over this domain within the full AISMM graph.

---

## 3. Main Output

Structured state machines and lifecycles entities with stable IDs, typed relationships, and cross-layer links.

---

## 4. Identifiable Entities

```text
state_machine.*
state.*
state_transition.*
lifecycle.*
transition_guard.*
```

---

## 5. Required Structure

Each entity must include:

- `id` following the AISMM ID strategy: `state.<product>.<name>`
- `name` or `title`
- Status field where applicable
- Cross-layer links to related entities

---

## 6. Cross-Layer Links

Entities in this layer link to other AISMM bundles as appropriate. See the bundle README for the full cross-layer link map.

---

## 7. Summary

Layer 405 (State Machines and Lifecycles) extends the AISMM model with state machines and lifecycles, enabling full product knowledge coverage for AI-native systems.
