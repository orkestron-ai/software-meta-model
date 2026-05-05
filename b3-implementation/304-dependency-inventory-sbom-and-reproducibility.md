<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 304
layer_key: dependency_inventory_sbom_and_reproducibility
document_id: spec.b3.dependency_inventory_sbom_and_reproducibility
document_type: layer_specification
module_scope: b3
status: accepted
spec_version: 2.0.0
title: Dependency Inventory, SBOM and Reproducibility Layer Specification
<!-- AISMM:META_END -->

# 304 — Dependency Inventory, SBOM and Reproducibility

## 1. Purpose

Model the full dependency graph, SBOM, license inventory, and supply chain provenance for build reproducibility.

---

## 2. Layer Role in AISMM

This layer extends Bundle B3 with dependency inventory, sbom and reproducibility modeling capabilities, enabling AI agents and humans to reason over this domain within the full AISMM graph.

---

## 3. Main Output

Structured dependency inventory, sbom and reproducibility entities with stable IDs, typed relationships, and cross-layer links.

---

## 4. Identifiable Entities

```text
dependency.*
package.*
sbom.*
license_record.*
container_digest.*
lock_file.*
build_reproducibility.*
supply_chain_attestation.*
```

---

## 5. Required Structure

Each entity must include:

- `id` following the AISMM ID strategy: `dependency.<product>.<name>`
- `name` or `title`
- Status field where applicable
- Cross-layer links to related entities

---

## 6. Cross-Layer Links

Entities in this layer link to other AISMM bundles as appropriate. See the bundle README for the full cross-layer link map.

---

## 7. Summary

Layer 304 (Dependency Inventory, SBOM and Reproducibility) extends the AISMM model with dependency inventory, sbom and reproducibility, enabling full product knowledge coverage for AI-native systems.
