<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 707
layer_key: privacy_rights_and_dpia
document_id: spec.b7.privacy_rights_and_dpia
document_type: layer_specification
module_scope: b7
status: accepted
spec_version: 2.0.0
title: Privacy Rights and DPIA Layer Specification
<!-- AISMM:META_END -->

# 707 — Privacy Rights and DPIA

## 1. Purpose

Model DPIA records, data subject rights, consent scopes, and deletion requests.

---

## 2. Layer Role in AISMM

This layer extends Bundle B7 with privacy rights and dpia modeling capabilities, enabling AI agents and humans to reason over this domain within the full AISMM graph.

---

## 3. Main Output

Structured privacy rights and dpia entities with stable IDs, typed relationships, and cross-layer links.

---

## 4. Identifiable Entities

```text
dpia.*
data_subject_request.*
consent_scope.*
privacy_right.*
retention_exception.*
deletion_request.*
```

---

## 5. Required Structure

Each entity must include:

- `id` following the AISMM ID strategy: `privacy.<product>.<name>`
- `name` or `title`
- Status field where applicable
- Cross-layer links to related entities

---

## 6. Cross-Layer Links

Entities in this layer link to other AISMM bundles as appropriate. See the bundle README for the full cross-layer link map.

---

## 7. Summary

Layer 707 (Privacy Rights and DPIA) extends the AISMM model with privacy rights and dpia, enabling full product knowledge coverage for AI-native systems.
