# AISMM Empty Layer Standard

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.empty-layer-standard
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

A **missing layer** and an **empty layer** are semantically different in AISMM.

```text
missing layer = unknown whether the layer exists; no file, no declaration
empty layer   = layer exists, is expected, but has no modeled entities yet
```

Without an explicit empty layer block, tools and AI agents cannot distinguish between "this layer has not been started" and "this layer does not exist in this model."

The Empty Layer Standard defines how to represent layers that are expected but not yet populated.

---

## 1. Completion Status Values

Every layer document may carry a `completion_status` field:

| Status | Meaning |
|--------|---------|
| `not_started` | Layer exists as a file but no structure has been defined yet |
| `empty` | Layer has defined expected structure but no entities have been added |
| `partial` | Some entities exist but the layer is not complete |
| `complete` | Layer satisfies its completeness criteria |
| `not_applicable` | This layer is intentionally excluded from this model (requires rationale) |
| `deferred` | Layer is planned but explicitly deferred to a later phase |

---

## 2. Empty Layer Metadata Requirements

Every empty layer block must include the following fields in its AISMM metadata block:

```yaml
type: layer_document
model_instance_id: <uuid>
product_id: <uuid>
layer_id: "<layer_id>"
layer_key: <string>
document_id: <bundle>.<layer_id>.layer.<layer_key>
document_type: layer_document
module_scope: root
status: stable
spec_version: 2.0.0
completion_status: empty
```

The `model_instance_id` and `product_id` bind the empty block to the correct product model, preventing accidental merging of empty stubs from different products.

---

## 3. Empty Layer Content Pattern

```markdown
<!-- AISMM:BEGIN -->
type: layer_document
model_instance_id: 00000000-0000-0000-0000-000000000000
product_id: 00000000-0000-0000-0000-000000000000
layer_id: "401"
layer_key: requirements
document_id: b4.401.layer.requirements
document_type: layer_document
module_scope: root
status: stable
spec_version: 2.0.0
completion_status: empty
<!-- AISMM:META_END -->

# 401 — Requirements

## Completion Status

```text
empty
```

## Expected Structure

- requirements: []
- acceptance_criteria: []
- requirement_groups: []
- requirement_relationships: []

## Known Gaps

- Requirements have not been modeled yet.
- No functional requirements have been captured.
- Acceptance criteria are undefined.

<!-- AISMM:END -->
```

---

## 4. Rules

### Presence Rule

Every expected layer in `aismm.registry.json` must be satisfied by either:

1. At least one real AISMM block for that layer, **or**
2. An explicit empty layer block with `completion_status: empty`, **or**
3. A declaration in `declared_empty_layers` in the registry

### `not_applicable` Rule

A `not_applicable` layer must include a `rationale` explaining why it is excluded:

```yaml
completion_status: not_applicable
rationale: "This product has no user-facing UI; b5 UX layers do not apply."
```

### `partial` Rule

A `partial` layer must include at least one `known_gap` entry explaining what is missing.

### Strict Mode Behavior

| State | Strict Mode Response |
|-------|---------------------|
| Required layer missing, no empty block | **error** |
| Required layer empty with valid empty block | warning |
| `not_applicable` without rationale | warning |
| `partial` without known gaps | warning |
| `complete` but completeness criteria not met | error |

---

## 5. Validation Artifact Extension

The `aismm.validation.json` artifact should include layer completion status:

```json
{
  "validation_run_id": "validation.2026_05_05.main",
  "model_completeness_status": "partial",
  "loaded_sources": ["src.main_repo"],
  "missing_sources": [],
  "restricted_sources": ["src.security_restricted"],
  "declared_empty_layers": ["1004", "1005"],
  "layer_completion_summary": {
    "complete": 45,
    "partial": 12,
    "empty": 8,
    "not_started": 3,
    "not_applicable": 2,
    "deferred": 1,
    "missing": 0
  }
}
```

---

## 6. Example Empty Layer File

See [`examples/empty-layer.example.md`](./examples/empty-layer.example.md) for a complete template.

---

## 7. Cross-References

- [`aismm-model-registry.md`](./aismm-model-registry.md) — registry declares expected and empty layers
- [`aismm-strict-mode.md`](./aismm-strict-mode.md) — strict mode rules for missing vs empty
- [`aismm-consistency-checks.md`](./aismm-consistency-checks.md) — consistency check for empty layers

---

## Summary

| Concept | Definition |
|---------|-----------|
| Empty layer | Layer file exists with `completion_status: empty` and no entities |
| Missing layer | No layer file and not declared empty in registry |
| `completion_status` | Lifecycle status of a layer's population |
| Strict mode | Distinguishes missing (error) from empty (warning) |

Empty layers make incompleteness **visible and intentional** rather than ambiguous.
