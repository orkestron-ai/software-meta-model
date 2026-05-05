<!-- AISMM:BEGIN -->
type: global_specification
document_id: spec.global.id.strategy
document_type: global_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: AISMM Unified ID Strategy
<!-- AISMM:META_END -->

# AISMM Unified ID Strategy

## 1. Purpose

This document defines a **unified identifier strategy** for AISMM entities across all bundles and layers.

It ensures that every identifiable entity can be:

- referenced globally
- traced across layers
- linked in schemas
- used by humans and AI agents
- safely moved between files without losing identity

---

## 2. Core Principle

```text
Identity must not depend on file location.
```

File paths may change, documents may be split or merged, and preferred representations may differ.

The entity ID must remain stable.

---

## 3. Global ID Format

Recommended canonical format:

```text
b{bundle}.{layer}.{entity_type}.{slug}
```

Example:

```text
b4.401.requirement.user_login
b2.201.component.auth_service
b8.801.work_item.fix_login_timeout
b9.902.trace_link.requirement_to_component
```

---

## 4. ID Parts

### 4.1 Bundle

The bundle where the entity is primarily defined.

Examples:

```text
b0
b1
b2
...
b9
```

---

### 4.2 Layer

Layer identifier. Supports 3-digit format for bundles b0–b9 and 4-digit format for bundles b10 and above.

Format rule:

```text
b0–b9:   3-digit layer ID, e.g. 001, 401, 902
b10+:    4-digit layer ID, e.g. 1001, 1104, 1206
```

Recommended regex:

```regex
^[0-9]{3,}$
```

Examples:

```text
001
102
203
806
902
1001
1104
1206
```

> **Note:** Do not parse only the first 3 digits of a layer ID. `1001`, `1101`, and `1201` are distinct 4-digit IDs — they must not be interpreted as `100`, `110`, or `120`.

---

### 4.3 Entity Type

Stable semantic type of the entity.

Examples:

```text
requirement
component
data_entity
work_item
decision
trace_link
retention_score
```

---

### 4.4 Slug

Human-readable stable name.

Rules:

- lowercase
- latin letters, digits and underscores only
- must start with a letter
- should be short but meaningful
- should not include status, date, or version unless identity truly depends on it

Good:

```text
user_login
payment_authorization
billing_service
release_v1_4_0
```

Bad:

```text
new_task
final_version
temp_fix
task_2026_05_01
```

---

## 5. JSON Schema Pattern

Recommended ID pattern (supports 3-digit and 4-digit layer IDs):

```json
{
  "type": "string",
  "pattern": "^b[0-9]+\\.[0-9]{3,}\\.[a-z][a-z0-9_]*\\.[a-z][a-z0-9_]*$"
}
```

This pattern allows:

- 3-digit layer IDs for bundles b0–b9: `b4.401.requirement.user_login`
- 4-digit layer IDs for bundles b10+: `b10.1004.model_version.fraud_detector_v3`

Examples of valid global IDs:

```text
b4.401.requirement.user_login
b9.902.trace_link.req_to_component
b10.1001.data_product.customer_events
b10.1004.model_version.fraud_detector_v3
b11.1102.ownership.auth_service_team
b12.1203.token_budget.rag_context_budget
```

For local files or backward compatibility, existing shorter IDs may remain temporarily, but cross-layer references SHOULD use global IDs.

---

## 6. Local ID vs Global ID

### 6.1 Local ID

A local ID may be used inside one file or layer:

```text
requirement.user_login
component.auth_service
```

### 6.2 Global ID

A global ID must be used for cross-layer, cross-bundle, or long-lived references:

```text
b4.401.requirement.user_login
b2.201.component.auth_service
```

### 6.3 Rule

```text
Local IDs are allowed for authoring convenience.
Global IDs are required for traceability.
```

---

## 7. Reference Rules

Any field that references entities across layers SHOULD use global IDs.

Examples:

```json
{
  "implements_requirements": [
    "b4.401.requirement.user_login"
  ],
  "implemented_in_components": [
    "b2.201.component.auth_service"
  ]
}
```

---

## 8. Entity Type Naming Rules

Entity type MUST be singular.

Good:

```text
requirement
component
work_item
trace_link
```

Bad:

```text
requirements
components
work_items
trace_links
```

Entity type SHOULD match schema definition names where possible.

---

## 9. Stability Rules

An ID SHOULD NOT change when:

- title changes
- description changes
- file moves
- representation format changes
- entity receives more details
- entity status changes

An ID MAY change only when:

- the entity is split into multiple distinct entities
- the entity meaning changes fundamentally
- the original entity was incorrectly identified

When an ID changes, an alias or migration record SHOULD be preserved.

---

## 10. Versioning

Version should not be embedded in the ID unless the version is part of the entity identity.

Good:

```text
b8.804.version.v1_4_0
b8.804.release.release_2026_05_01
```

Avoid:

```text
b4.401.requirement.user_login_v2
```

Instead use:

```json
{
  "id": "b4.401.requirement.user_login",
  "version": "2.0"
}
```

---

## 11. Aliases and Migration

When entities are renamed or moved, preserve aliases:

```json
{
  "id": "b2.201.component.auth_service",
  "aliases": [
    "component.authentication",
    "b2.201.service.auth"
  ]
}
```

Recommended alias fields:

```text
aliases
previous_ids
replaced_by
supersedes
```

---

## 12. Cross-Bundle Examples

### Requirement to Component

```text
b4.401.requirement.user_login
  → implemented_by →
b2.201.component.auth_service
```

### Component to Code

```text
b2.201.component.auth_service
  → implemented_in →
b3.302.module.auth
```

### Work Item to Requirement

```text
b8.801.work_item.fix_login_timeout
  → affects →
b4.401.requirement.user_login
```

### Trace Link

```text
b9.902.trace_link.user_login_requirement_to_auth_component
```

---

## 13. Reserved Prefixes

Recommended reserved top-level prefixes:

| Prefix | Meaning |
|--------|---------|
| b0 | Product Core |
| b1 | Business Dynamics |
| b2 | System Design |
| b3 | Implementation |
| b4 | Product Behavior |
| b5 | User Interaction |
| b6 | Runtime Operations |
| b7 | Quality, Risk & Compliance |
| b8 | Change Execution |
| b9 | Knowledge Traceability |
| b10 | Reserved for Agent / Orchestration extensions |

---

## 14. ID Governance

AISMM SHOULD maintain an ID registry or generated index.

Recommended file:

```text
/.smm/id-registry.json
```

The registry may contain:

```json
{
  "id": "b4.401.requirement.user_login",
  "entity_type": "requirement",
  "title": "User Login",
  "layer": "401",
  "bundle": "b4",
  "defined_in": "b4-product-behavior/401-requirements.md",
  "status": "active",
  "aliases": []
}
```

---

## 15. Validation Rules

A valid AISMM repository SHOULD enforce:

- unique global IDs
- no broken references
- no duplicate active IDs
- no references to archived IDs unless explicitly allowed
- aliases resolve to active IDs or archived records
- entity type in ID matches actual schema entity type

---

## 16. Summary

The unified ID strategy makes AISMM a stable, traceable, machine-readable model.

```text
Files organize knowledge.
IDs define identity.
Trace links define reasoning.
```

<!-- AISMM:END -->
