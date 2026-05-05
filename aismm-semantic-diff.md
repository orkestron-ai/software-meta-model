# AISMM Semantic Diff Specification

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.semantic-diff
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

Standard text diffs show what text changed. Semantic diff explains **how AISMM changed as a model** — what entities were added, removed, or restructured; which relationships weakened; which trace links changed type; which confidence levels dropped.

This enables meaningful pull request reviews and AI-assisted change impact assessment.

---

## 1. Semantic Change Types

| Change Type | Description |
|-------------|-------------|
| `entity_added` | A new identifiable entity was introduced |
| `entity_removed` | An existing entity was deleted |
| `entity_changed` | An entity's fields or content changed |
| `entity_moved` | An entity moved to a different layer or bundle |
| `entity_renamed` | An entity's ID or canonical name changed |
| `reference_added` | A new reference from one entity to another was added |
| `reference_removed` | An existing reference was removed |
| `trace_link_added` | A new trace link was added to the traceability graph |
| `trace_link_removed` | An existing trace link was removed |
| `trace_link_type_changed` | The `relationship_type` of a trace link changed |
| `trace_link_weight_changed` | The weight or criticality of a trace link changed |
| `confidence_changed` | The confidence score of a knowledge item changed |
| `provenance_changed` | The source provenance of an entity changed |
| `coverage_changed` | The coverage metric of a context package changed |
| `schema_changed` | A bundle schema was modified |
| `layer_added` | A new layer specification was introduced |
| `layer_deprecated` | An existing layer was marked deprecated |
| `bundle_added` | A new bundle was introduced |

---

## 2. Semantic Diff Artifact

When a pull request is processed, a semantic diff artifact may be generated:

```text
aismm.semantic_diff.json
```

### Example

```json
{
  "diff_id": "semantic_diff.pr_42",
  "base_ref": "main",
  "head_ref": "feature/new-payment-api",
  "generated_at": "2026-05-05T10:00:00Z",
  "summary": {
    "entities_added": 3,
    "entities_removed": 0,
    "entities_changed": 5,
    "trace_links_added": 8,
    "trace_links_removed": 1,
    "confidence_changes": 2,
    "schema_changes": 1
  },
  "changes": [
    {
      "change_type": "entity_added",
      "entity_id": "b2.203.api_operation.post_payment_confirm",
      "layer": "203",
      "bundle": "b2",
      "file": "b2-system-design/203-api-and-interfaces.md",
      "description": "New API operation added for payment confirmation"
    },
    {
      "change_type": "trace_link_removed",
      "source_entity": "b4.401.requirement.legacy_checkout",
      "target_entity": "b3.302.module.checkout_v1",
      "relationship_type": "implements",
      "criticality": "high",
      "description": "High-criticality trace link removed — verify that requirement is superseded",
      "requires_review": true
    },
    {
      "change_type": "confidence_changed",
      "entity_id": "b9.903.source.payment_spec_v2",
      "from_confidence": 0.92,
      "to_confidence": 0.71,
      "description": "Source confidence reduced — may affect downstream retrievals"
    }
  ]
}
```

---

## 3. PR Review Rules

The following rules guide semantic diff-based review:

| Rule | Trigger | Required Action |
|------|---------|-----------------|
| Security layer change | Any change in b7.703 or b7.705–707 | Security owner review required |
| Economics model change | Any change in b0.006 or b12 | Product/value owner review required |
| Architecture decision change | Any change in b8.805 with `decision_type: architecture` | Architect review required |
| High-criticality trace link removed | `trace_link_removed` with `criticality: high` | Explicit approval required |
| Requirement without tests | `entity_added` in b4.401 with no linked test | Warning — DoD not satisfied |
| Entity deleted without successor | `entity_removed` with no `superseded_by` | Warning — orphan risk |
| Confidence decreased significantly | `confidence_changed` with delta > 0.2 | Review provenance update |
| Schema breaking change | `schema_changed` with `backward_compatible: false` | Migration record required |

---

## 4. Semantic Diff in Git Workflow

### Pull Request Template Addition

Repositories using AISMM should add to their PR template:

```markdown
## Semantic Impact
<!-- Attach aismm.semantic_diff.json or summarize below -->
- Entities added:
- Entities removed:
- Trace links affected:
- Confidence changes:
- Schema changes:
```

### Automated Comment Example

A CI bot may post the semantic diff summary as a PR comment:

```
AISMM Semantic Diff — PR #42
━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ 3 entities added
✅ 8 trace links added
⚠️  1 high-criticality trace link removed (requires review)
⚠️  2 confidence values decreased
ℹ️  1 schema change (backward compatible)

Security layer touched → security owner review required.
```

---

## Summary

| Concept | Definition |
|---------|-----------|
| Semantic diff | Change description at the model level, not the text level |
| Artifact | `aismm.semantic_diff.json` (CI output) |
| Change types | 18 canonical types covering entities, links, confidence, schema |
| PR review rules | Ownership-based gate rules triggered by change type |

Semantic diff makes AISMM pull requests reviewable as model changes, not just text changes.
