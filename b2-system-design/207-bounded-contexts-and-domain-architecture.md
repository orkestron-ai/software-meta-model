<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 207
layer_key: bounded_contexts_and_domain_architecture
document_id: spec.b2.bounded_contexts_and_domain_architecture
document_type: layer_specification
module_scope: b2
status: accepted
spec_version: 2.0.0
title: Bounded Contexts and Domain Architecture Layer Specification
<!-- AISMM:META_END -->

# 207 — Bounded Contexts and Domain Architecture

## 1. Purpose

Model strategic DDD: bounded contexts, subdomains, context maps, and anti-corruption layers.

---

## 2. Layer Role in AISMM

This layer extends Bundle B2 with bounded contexts and domain architecture modeling capabilities, enabling AI agents and humans to reason over this domain within the full AISMM graph.

---

## 3. Main Output

Structured bounded contexts and domain architecture entities with stable IDs, typed relationships, and cross-layer links.

---

## 4. Identifiable Entities

```text
bounded_context.*
subdomain.*
context_map.*
upstream_context.*
downstream_context.*
anti_corruption_layer.*
shared_kernel.*
domain_service.*
```

---

## 5. Required Structure

Each entity must include:

- `id` following the AISMM ID strategy: `bounded.<product>.<name>`
- `name` or `title`
- Status field where applicable
- Cross-layer links to related entities

---

## 6. Cross-Layer Links

Entities in this layer link to other AISMM bundles as appropriate. See the bundle README for the full cross-layer link map.

---

## 7. Summary

Layer 207 (Bounded Contexts and Domain Architecture) extends the AISMM model with bounded contexts and domain architecture, enabling full product knowledge coverage for AI-native systems.
