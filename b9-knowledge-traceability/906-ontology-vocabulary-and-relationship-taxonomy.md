<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 906
layer_key: ontology_vocabulary_and_relationship_taxonomy
document_id: spec.knowledge.ontology
document_type: layer_specification
module_scope: b9
status: accepted
spec_version: 2.0.0
title: Ontology, Vocabulary and Relationship Taxonomy Layer Specification
<!-- AISMM:META_END -->

# 906 — Ontology, Vocabulary and Relationship Taxonomy

## 1. Purpose

Define the **common vocabulary, ontology concepts, relationship types, and graph semantics** used across AISMM.

Without a shared vocabulary, relationship types become inconsistent, impact analysis breaks down, and AI agents cannot reliably reason over trace links.

---

## 2. Layer Role in AISMM

This layer is the **semantic foundation** of the traceability graph. It defines what relationships mean — not just that A links to B, but what kind of link it is and what that implies.

---

## 3. Main Output

A canonical relationship taxonomy, ontology terms, edge semantics, and a vocabulary registry.

---

## 4. Core Concepts

**ontology_term** — A named concept in the AISMM vocabulary with a stable ID and definition.

**relationship_type** — A canonical typed relationship between AISMM entities with defined semantics.

**edge_semantic** — A declaration of what a relationship type implies (direction, transitivity, invertibility).

**vocabulary_entry** — A term in the AISMM vocabulary with synonyms and deprecation status.

**synonym** — An alternative name for an ontology term or relationship type.

**deprecated_term** — A term that has been superseded and should no longer be used.

---

## 5. Identifiable Entities

```text
ontology_term.*
relationship_type.*
edge_semantic.*
vocabulary_entry.*
synonym.*
deprecated_term.*
```

---

## 6. Canonical Relationship Types

### Structural

| Type | Meaning |
|------|---------|
| `implements` | Entity A implements the intent of Entity B |
| `realizes` | Entity A realizes the value or outcome of Entity B |
| `depends_on` | Entity A requires Entity B to function |
| `derives_from` | Entity A is derived from or based on Entity B |
| `constrains` | Entity A constrains or limits Entity B |
| `requires` | Entity A explicitly requires Entity B |

### Behavioral

| Type | Meaning |
|------|---------|
| `motivates` | Entity A motivates or justifies Entity B |
| `refines` | Entity A makes Entity B more specific |
| `triggers` | Entity A activates or causes Entity B |
| `produces` | Entity A produces Entity B as output |
| `consumes` | Entity A consumes Entity B as input |
| `affects` | Entity A affects Entity B |
| `is_affected_by` | Entity A is affected by Entity B (inverse of `affects`) |

### Validation

| Type | Meaning |
|------|---------|
| `validates` | Entity A validates or verifies Entity B |
| `evidences` | Entity A provides evidence for Entity B |
| `refutes` | Entity A contradicts or disproves Entity B |
| `observes` | Entity A observes or monitors Entity B |

### Causal

| Type | Meaning |
|------|---------|
| `supersedes` | Entity A replaces Entity B |
| `conflicts_with` | Entity A conflicts with Entity B |

### Ownership

| Type | Meaning |
|------|---------|
| `owns` | Entity A is owned by Entity B (team → component) |
| `governs` | Entity A governs Entity B (policy → component) |

### Risk

| Type | Meaning |
|------|---------|
| `mitigates` | Entity A reduces the risk of Entity B |

### Temporal

| Type | Meaning |
|------|---------|
| `valid_during` | Entity A is valid during the period defined by Entity B |
| `preceded_by` | Entity A was preceded by Entity B |

---

## 7. Relationship Categories

```text
structural     — composition, dependency, implementation
behavioral     — causation, production, consumption
causal         — conflict, supersession
validation     — evidence, verification, observation
ownership      — governance, stewardship
risk           — mitigation, exposure
knowledge      — derivation, refinement
temporal       — validity windows, succession
security       — access, restriction
runtime        — deployment, execution
```

---

## 8. Required Edge Properties

Every trace link in the traceability graph (layer 902) should declare:

| Property | Required | Description |
|----------|----------|-------------|
| `relation_type` | yes | One of the canonical relationship types above |
| `direction` | recommended | `directed` or `bidirectional` |
| `weight` | optional | Numeric weight for prioritized traversal (0.0–1.0) |
| `criticality` | recommended | `critical`, `high`, `medium`, `low` |
| `confidence` | recommended | Confidence score (0.0–1.0) |
| `provenance` | recommended | Source entity for this link |
| `valid_from` | optional | Temporal validity start |
| `valid_to` | optional | Temporal validity end |
| `rationale` | optional | Human-readable justification |

---

## 9. Cross-Layer Links

Layer 906 is referenced by:

- `902` — Traceability Graph (uses relationship types)
- `903` — Source Provenance (uses ontology terms for provenance types)
- `904` — Context Coverage (uses vocabulary for gap classification)
- `905` — Context Retrieval and RAG (uses ontology terms for retrieval metadata)

---

## 10. Boundaries

- This layer defines vocabulary and types; it does not store actual trace links (that is 902).
- Domain-specific ontologies may extend this vocabulary using the `x-<org>.<domain>.<term>` convention.

---

## 11. Summary

Layer 906 is the semantic backbone of the AISMM knowledge graph. By defining typed relationships and a shared vocabulary, it enables precise impact analysis, conflict detection, and reliable AI agent reasoning over the full product model.
