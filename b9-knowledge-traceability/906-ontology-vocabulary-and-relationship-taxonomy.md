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

## 1A. Product Model Representation

In a product repository, this layer is represented by the directory `aismm/b9-knowledge-traceability/906-ontology-vocabulary-and-relationship-taxonomy/`.

The questions in this specification must be answered through one or more artifact files inside that directory, not through a single layer file.

Each new semantic unit for this layer must create a new artifact file whose identity is preserved after creation.

Artifact naming, file IDs and preferred artifact kinds for layer `906` are governed by [`../aismm-layer-artifact-naming.md`](../aismm-layer-artifact-naming.md).

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

## 10A. v3 Relationship Additions

AISMM v3 adds the following canonical typed relationships (used by the new layers `b0.007`, `b1.104`, `b8.810`, `b9.907` and the validation/standards governance):

| Relationship | Domain → Range | Meaning |
|--------------|----------------|---------|
| `automates` | subject_domain → bounded_context/component | a real-world domain is automated by a system surface |
| `realizes_domain` | b4.402.rule → b0.007.domain_rule | an implemented rule realizes a real-world domain rule |
| `registered_as` | b0.007.domain_term → b9.906.term | a domain term is registered as a canonical term |
| `evaluates` | retrospective/outcome → work_item/release/hypothesis | an evaluative record assesses another record |
| `assesses_outcome_of` | b1.104.outcome → hypothesis/goal/initiative | an outcome record assesses the result of a bet |
| `derived_from` | record → source/ingestion_run | a record was derived from a source or ingestion run |
| `ingested_from` | ingestion_run → ingestion_binding | a run executed a binding |
| `attested_by` | record → attestation | a record is attested by an owner attestation |
| `validates` | owner → record | a source owner validates a derived record |
| `disputes` | owner → record | a source owner disputes a derived record |
| `inherits_from` | policy → standard | a product policy inherits a baseline standard |
| `paired_with` | b1.104.outcome ↔ b8.810.retrospective | value verdict paired with process retrospective for one initiative |

These relationships are projection edges in `902` like all others; `derived_from`/`validates`/`disputes` additionally drive the owner-validation gate (`aismm-owner-validation-and-attestation.md`).

---

## 11. Summary

Layer 906 is the semantic backbone of the AISMM knowledge graph. By defining typed relationships and a shared vocabulary, it enables precise impact analysis, conflict detection, and reliable AI agent reasoning over the full product model.
