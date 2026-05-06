# AISMM Strict Mode and Validation Model

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.strict-mode
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

Strict Mode turns AISMM from documentation into a **validated semantic model**.

Without enforcement, AISMM risks accumulating rich documentation with broken references, orphaned entities, and unverified traceability. Strict Mode defines the rules, levels, and tooling interface for making AISMM structurally sound.

---

## 1. Validation Levels

Every AISMM repository may declare a validation level:

```yaml
validation_level: advisory | recommended | strict
```

| Level | Behavior |
|-------|----------|
| `advisory` | Violations are logged but do not block. Suitable for early-stage repositories. |
| `recommended` | Violations produce warnings. CI may fail on critical violations. |
| `strict` | All violations block merges and CI pipelines. Suitable for production-grade repositories. |

---

## 2. Mandatory Validation Rules

### Identity Rules

| Rule ID | Rule | Severity |
|---------|------|----------|
| `RULE-ID-001` | Every global ID must be unique across the repository. | critical |
| `RULE-ID-002` | Every entity ID must conform to the AISMM ID strategy format: `b{N}.{layer_id}.{entity_type}.{slug}` where `layer_id` is 3+ digits (`^[0-9]{3,}$`). For bundles b10+, layer IDs are 4 digits (e.g. `1001`, `1104`, `1206`) and must not be truncated or parsed as 3-digit IDs. | error |
| `RULE-ID-003` | No entity may reference a non-existent ID. | critical |

### Product and Model Instance Identity Rules

| Rule ID | Rule | Severity |
|---------|------|----------|
| `RULE-PID-001` | Every AISMM block must declare `product_id`. | error |
| `RULE-PID-002` | Every AISMM block must declare `model_instance_id`. | error |
| `RULE-PID-003` | `product_id` must be a valid UUID. | error |
| `RULE-PID-004` | `model_instance_id` must be a valid UUID. | error |
| `RULE-PID-005` | Blocks with different `product_id` must not be merged into one model unless explicitly mapped by the registry. | critical |
| `RULE-PID-006` | Blocks with different `model_instance_id` must not be merged unless the registry declares the merge allowed. | critical |

### Registry and Completeness Rules

| Rule ID | Rule | Severity |
|---------|------|----------|
| `RULE-REG-001` | If `aismm.registry.json` exists, it must be used as the model discovery root. | critical |
| `RULE-REG-002` | If no registry exists, `completeness_status` must be reported as `unknown`. | warning |
| `RULE-REG-003` | If an expected required source is unavailable, `completeness_status` must be `partial` or `invalid`. | error |
| `RULE-REG-004` | AI context packages must include `model_completeness_status`. | warning |
| `RULE-REG-005` | A parser must report which bundles/layers were loaded and which were unavailable. | warning |

### Empty and Missing Layer Rules

| Rule ID | Rule | Severity |
|---------|------|----------|
| `RULE-LAYER-001` | A required layer with no real block and no empty layer block declared in registry is **missing** → validation error. | error |
| `RULE-LAYER-002` | A required layer with a valid empty layer block is **empty** → validation warning only. | warning |
| `RULE-LAYER-003` | A layer with `completion_status: not_applicable` must include a rationale field. | warning |
| `RULE-LAYER-004` | A layer with `completion_status: partial` must include `known_gaps`. | warning |
| `RULE-LAYER-005` | A layer with `completion_status: complete` must satisfy its layer completeness criteria. | warning |

### Cross-Layer Reference Rules

| Rule ID | Rule | Severity |
|---------|------|----------|
| `RULE-REF-001` | Every cross-layer reference must resolve to an existing entity. | critical |
| `RULE-REF-002` | Every trace link must declare a `relationship_type`. | error |
| `RULE-REF-003` | Every high-criticality trace link must declare `confidence` and `provenance`. | error |

### Requirement Traceability Rules

| Rule ID | Rule | Severity |
|---------|------|----------|
| `RULE-REQ-001` | Every requirement must link to at least one behavior or domain rule. | warning |
| `RULE-REQ-002` | Every critical requirement must link to at least one test or validation check. | error |
| `RULE-REQ-003` | Every requirement that changes must trigger re-evaluation of linked tests. | warning |

### Release and Work Item Rules

| Rule ID | Rule | Severity |
|---------|------|----------|
| `RULE-REL-001` | Every release must link to included work items. | error |
| `RULE-REL-002` | Every work item should link to a value source, requirement, or risk. | warning |
| `RULE-REL-003` | Every high-risk change must link to a mitigation or approval record. | error |

### Knowledge and Context Rules

| Rule ID | Rule | Severity |
|---------|------|----------|
| `RULE-CTX-001` | Every context package must reference its source retrieval units. | warning |
| `RULE-CTX-002` | Restricted content must not appear in unauthorized context packages. | critical |
| `RULE-CTX-003` | Blocks with `authorship_class: imported_untrusted` must not be treated as authoritative. | critical |

### Schema Rules

| Rule ID | Rule | Severity |
|---------|------|----------|
| `RULE-SCH-001` | Every block must include a valid `aismm_meta` header. | critical |
| `RULE-SCH-002` | Every block must declare a `spec_version`. | error |
| `RULE-SCH-003` | Extension fields must follow the `x-<org>.<domain>.<entity>` namespace pattern. | warning |

---

## 3. Validation Artifact

When validation runs, it may produce a machine-readable artifact:

```text
aismm.validation.json
```

This file should not be committed to the repository but may be published as a CI artifact.

### Example

```json
{
  "validation_run_id": "validation.2026_05_05.main",
  "aismm_version": "2.0.0",
  "model_instance_id": "5f7f6f89-8db2-4a5c-9a67-1c59d29fd001",
  "product_id": "1c936e9b-c9d8-4871-8d56-0b7e0b8b61fb",
  "validation_level": "strict",
  "status": "failed",
  "model_completeness_status": "partial",
  "loaded_sources": ["source.primary", "source.infra"],
  "missing_sources": [],
  "restricted_sources": ["source.security"],
  "declared_empty_layers": ["b10", "b11", "b12"],
  "summary": {
    "total_checks": 142,
    "passed": 138,
    "warnings": 2,
    "errors": 1,
    "critical": 1
  },
  "violations": [
    {
      "rule_id": "RULE-REF-001",
      "severity": "critical",
      "entity_id": "b4.401.requirement.user_login",
      "message": "Cross-layer reference target 'b2.201.component.missing_service' does not exist.",
      "file": "b4-product-behavior/401-requirements.md",
      "line": 47
    },
    {
      "rule_id": "RULE-REQ-002",
      "severity": "error",
      "entity_id": "b4.401.requirement.payment_confirmation",
      "message": "Critical requirement has no linked test or validation check.",
      "file": "b4-product-behavior/401-requirements.md",
      "line": 83
    }
  ]
}
```

### Violation Severity Meanings

| Severity | Meaning |
|----------|---------|
| `critical` | Structural integrity broken. Must fix before merge in `strict` mode. |
| `error` | Rule violation. Blocks CI in `recommended` and `strict` modes. |
| `warning` | Recommended practice missing. Logged in all modes. |
| `info` | Informational. Does not block anything. |

---

## 4. CI Integration

### Basic CI Step (GitHub Actions Example)

```yaml
- name: Validate AISMM
  run: |
    npx aismm-validate \
      --root . \
      --level strict \
      --output aismm.validation.json

- name: Upload validation report
  uses: actions/upload-artifact@v3
  with:
    name: aismm-validation
    path: aismm.validation.json
```

### Gating Strategy

For `strict` mode repositories:

- All `critical` violations → fail CI, block merge
- All `error` violations → fail CI, block merge
- All `warning` violations → logged, do not block

For `recommended` mode:

- All `critical` violations → fail CI
- Errors and warnings → logged

### Branch Protection Integration

Repositories at L3+ conformance should add AISMM validation as a required status check on protected branches.

---

## 5. Rule Customization

Teams may extend or override validation rules using a local configuration:

```yaml
# aismm-validation-config.yaml
validation_level: strict
overrides:
  - rule_id: RULE-REQ-001
    severity: error   # upgrade from warning
  - rule_id: RULE-CTX-001
    enabled: false    # disable for early-stage project
custom_rules: []
```

Custom rules must not relax `critical` severity on security-related rules (`RULE-CTX-002`, `RULE-CTX-003`).

---

## Summary

| Concept | Definition |
|---------|-----------|
| Validation Level | `advisory` / `recommended` / `strict` |
| Validation Artifact | `aismm.validation.json` (CI output, not committed) |
| Rule IDs | Namespaced: `RULE-ID-*`, `RULE-REF-*`, `RULE-REQ-*`, `RULE-REL-*`, `RULE-CTX-*`, `RULE-SCH-*` |
| CI Integration | Status check on protected branches at L3+ conformance |

Strict Mode makes AISMM a governed model rather than rich documentation.
