# AISMM Versioning and Conformance

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.versioning-and-conformance
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
  authors: ["AISMM Governance"]
```

---

## Purpose

This document defines the versioning model, layer lifecycle, migration policy, extension namespace, and conformance levels for AISMM itself.

It enables safe, predictable evolution of the meta-model over time and provides clear compatibility rules for consumers, tooling, and AI agents.

---

## 1. AISMM Versioning

AISMM uses **semantic versioning**:

```text
MAJOR.MINOR.PATCH
```

Current version: **AISMM 2.0.0**

### Version Change Rules

| Increment | When to use |
|-----------|-------------|
| **MAJOR** | New bundles with required fields, breaking schema changes, new mandatory metadata fields, removal or renaming of existing bundles/layers |
| **MINOR** | New optional layers, new optional fields, new preferred representations, new bundles without breaking changes |
| **PATCH** | Wording fixes, example corrections, non-breaking schema clarifications, documentation improvements |

### Version Declaration

Every AISMM-compliant repository should declare the AISMM version it targets in a root-level file or README:

```yaml
aismm_version: "2.0.0"
```

### Version Compatibility

- Consumers targeting AISMM `2.x.x` must accept all `2.MINOR.x` documents.
- Breaking changes only occur on MAJOR increments.
- Tooling must ignore unknown optional fields to remain forward-compatible.

---

## 2. Layer Lifecycle Status

Every layer specification carries a `status` field. The canonical status values are:

| Status | Meaning |
|--------|---------|
| `draft` | Work in progress. Not for production use. May change without notice. |
| `proposed` | Ready for community review. Breaking changes still possible. |
| `accepted` | Reviewed and accepted. Minor changes allowed. Suitable for use. |
| `stable` | Mature and stable. Changes follow full migration policy. |
| `deprecated` | Still usable but no longer recommended. Migration guidance exists. |
| `superseded` | Replaced by a newer layer. Reference to successor is provided. |
| `archived` | Removed from active use. Kept for historical record only. |
| `withdrawn` | Was proposed or accepted but retracted. Not to be used. |

### Allowed Transitions

```text
draft       → proposed | withdrawn
proposed    → accepted | draft | withdrawn
accepted    → stable | deprecated | withdrawn
stable      → deprecated
deprecated  → superseded | archived
superseded  → archived
```

Transitions not listed above require explicit governance approval and a migration record.

---

## 3. Migration Policy

When AISMM evolves between versions, affected parties must be able to understand what changed and how to migrate.

### Change Categories

| Change Type | Description |
|-------------|-------------|
| `layer_renamed` | A layer has a new canonical name |
| `layer_split` | A layer has been divided into two or more layers |
| `layer_merged` | Two or more layers have been combined into one |
| `layer_extension` | New fields or entities added to an existing layer |
| `field_renamed` | A metadata field name has changed |
| `entity_type_moved` | An entity type has moved from one layer to another |
| `bundle_added` | A new bundle has been added to AISMM |
| `bundle_deprecated` | An existing bundle is deprecated |
| `schema_breaking` | Schema change that breaks existing documents |
| `schema_additive` | Schema change that is backward-compatible |

### Migration Record Format

All MAJOR and significant MINOR version changes must produce a migration record in `migrations/`.

```json
{
  "migration_id": "migration.aismm_v1_to_v2.b9_relationship_taxonomy",
  "from_version": "1.0.0",
  "to_version": "2.0.0",
  "change_type": "layer_extension",
  "affected_layers": ["902"],
  "description": "Added relationship taxonomy and typed edge classification to the traceability graph layer.",
  "migration_steps": [
    {
      "step": 1,
      "action": "Add `relationship_type` field to all traceability graph edges.",
      "required": true
    },
    {
      "step": 2,
      "action": "Set `relationship_type` to `generic` for existing edges if type is unknown.",
      "required": false
    }
  ],
  "backward_compatible": true,
  "notes": "Existing documents without `relationship_type` remain valid at L2. L3+ requires typed edges."
}
```

### Migration Record Location

```text
migrations/
  migration.aismm_v1_to_v2.<description>.json
  migration.aismm_v2_to_v3.<description>.json
```

---

## 4. Extension Namespace

AISMM allows safe extension without forking using a namespaced prefix convention.

### Extension Prefix Format

```text
x-<organization>.<domain>.<entity>
```

### Rules

- Extension fields must start with `x-`.
- Organization slug must be lowercase, alphanumeric, hyphens allowed.
- Domain identifies the product or context area.
- Entity identifies the specific extension.
- Extension fields are always optional.
- AISMM tooling must ignore unknown extension fields.

### Examples

```text
x-datsteam.gameton.reward_model
x-acme.payments.settlement_rule
x-myorg.iam.permission_scope
```

### Extension Registration

Extensions do not require central registration. However, organizations are encouraged to document their extension namespaces in their own repositories to support tooling and agent consumption.

---

## 5. Conformance Levels

AISMM defines five conformance levels. Each level builds on the previous one.

### L1 — Block Format

> The repository uses AISMM-compatible blocks.

**Minimum requirements:**

- All blocks use the `aismm_meta` header with at minimum: `document_type`, `spec_version`.
- Files are Markdown with embedded YAML metadata blocks.
- No naming convention violations.

**What it enables:** human readability, basic tooling compatibility.

---

### L2 — Typed Entities

> Entities are explicitly typed and identifiable.

**Minimum requirements:**

- All blocks satisfy L1.
- All significant entities carry a stable `id` following the AISMM ID strategy.
- Entity types are declared using AISMM-defined type values from the relevant layer schema.
- Layer assignment is explicit in each block.

**What it enables:** entity-level search, type-based filtering, schema validation.

---

### L3 — Traceability Graph

> Entities are connected across layers via typed relationships.

**Minimum requirements:**

- All blocks satisfy L2.
- Cross-layer links use the AISMM relationship format with source, target, and `relationship_type`.
- Bundle b9 (Knowledge Traceability) is present and populated.
- At minimum, requirements are traced to implementation, and implementation to tests.

**What it enables:** impact analysis, dependency traversal, AI-assisted change scoping.

---

### L4 — RAG-Ready Context

> Blocks are structured for effective retrieval-augmented generation.

**Minimum requirements:**

- All blocks satisfy L3.
- Each block has a `summary` field with a self-contained natural language description.
- Blocks are bounded to RAG-friendly chunk sizes (recommended ≤ 1500 tokens per block).
- Bundle b9 layer 905 (Context Retrieval and RAG) is populated.
- Retrieval metadata such as `context_tags` or `retrieval_hints` are present.

**What it enables:** semantic search, context assembly for AI agents, grounded code generation.

---

### L5 — Agent-Grade Governance

> The model supports safe, auditable AI agent operations.

**Minimum requirements:**

- All blocks satisfy L4.
- Every mutable block carries a `last_modified_by` field (human or agent ID).
- Change history is maintained in bundle b8.
- Agent-visible vs. agent-writable scopes are declared in block metadata.
- Source provenance is declared (bundle b9, layer 903).
- No orphaned entities: all entities are reachable from at least one other entity.

**What it enables:** safe agent writes, full audit trails, governed AI-assisted evolution of the product model.

---

## 6. Conformance Declaration

A repository may declare its conformance level in its root README or in a `aismm-manifest.yaml`:

```yaml
aismm_version: "2.0.0"
conformance_level: L3
bundles:
  - b0-product-core
  - b1-business-dynamics
  - b2-system-design
  - b3-implementation
  - b4-product-behavior
  - b5-user-interaction
  - b6-runtime-operations
  - b7-quality-risk-compliance
  - b8-change-execution
  - b9-knowledge-traceability
```

---

## Summary

| Topic | Definition |
|-------|-----------|
| Versioning | Semantic versioning (MAJOR.MINOR.PATCH) |
| Current AISMM version | 2.0.0 |
| Layer statuses | draft → proposed → accepted → stable → deprecated → superseded → archived |
| Migration records | JSON documents in `migrations/` directory |
| Extension namespace | `x-<org>.<domain>.<entity>` |
| Conformance levels | L1 Block Format → L2 Typed Entities → L3 Traceability Graph → L4 RAG-Ready → L5 Agent Governance |

AISMM versioning and conformance ensures that the meta-model can evolve safely, that consumers can declare their compatibility level, and that AI agents can operate with known governance guarantees.
