<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 907
layer_key: ingestion_bindings_and_extraction_recipes
document_id: spec.ingestion.bindings.extraction.recipes
document_type: layer_specification
module_scope: root
status: stable
spec_version: 3.0.0
title: Ingestion Bindings and Extraction Recipes Layer Specification
kind_class: descriptive
<!-- AISMM:META_END -->

# 907 — Ingestion Bindings and Extraction Recipes

## 1. Purpose of the Layer

The **Ingestion Bindings and Extraction Recipes** layer defines **how records in each layer are populated by agents from the systems that own the data**.

Most AISMM records are *derived*: an agent reads a source system (GitLab, Confluence, Jira, Grafana, a database, an MCP-exposed tool) and produces a model record. This layer makes that pipeline an explicit, governed, traceable object instead of an invisible side effect.

It answers:

- Which owning system is the source of truth for a given layer's records?
- Through which connector (MCP server / tool / API) is it reached?
- Which extraction recipe (prompt + mapping) transforms source data into records?
- On what trigger or schedule does ingestion run?
- Who owns the source, and what validation must the result pass before it is trusted?

> V3 design note (answering the analyst's question "do we need a layer with prompts, or where to put this?"): we **split** the concept. The *recipe/prompt* is **normative** (a method) and lives in `00-policies` as a versioned `recipe`/`prompt` record. The *binding* (which system → which connector → which recipe → which target layer → which owner) and the *ingestion run provenance* are **descriptive** and live here in `b9.907` + `b9.903`. A binding **references** a recipe; it does not inline it.

---

## 1A. Product Model Representation

In a product repository, this layer is represented by the directory `aismm/b9-knowledge-traceability/907-ingestion-bindings-and-extraction-recipes/`.

The questions in this specification must be answered through one or more record files inside that directory, not through a single layer file.

Each new semantic unit for this layer must create a new record file whose identity is preserved after creation.

Record naming, file IDs and preferred record kinds for layer `907` are governed by [`../aismm-layer-artifact-naming.md`](../aismm-layer-artifact-naming.md).

Preferred record kinds: `binding`, `ingest`, `recipe`.

---

## 2. Layer Role in AISMM

This layer connects:

Owning System → Connector (MCP) → Recipe (prompt + mapping) → Target Layer Records → Provenance → Validation

It is the **data acquisition contract** of the model. It is what lets agents repeatably and safely fill layer directories, and it is the anchor for source-owner validation (`aismm-owner-validation-and-attestation.md`).

Other layers use it to:

- attach run provenance to derived records (`b9.903`)
- route owner validation to the correct source owner (`b11.1102`)
- assemble safe context packages knowing each record's ingestion path (`b9.905`)
- bind systems described in `b2.204`/`b2.205` to the records they feed

---

## 3. Main Output

- ingestion binding records (system ↔ connector ↔ recipe ↔ target layer)
- recipe references (recipes themselves are normative in `00-policies`)
- field/mapping definitions (source field → record field)
- trigger/schedule definitions
- source-owner references and required validation policy
- ingestion run records (provenance of each actual run)

---

## 4. Core Concepts

### 4.1 Ingestion Binding
A first-class object stating: *records of layer X are populated from owning system S via connector C using recipe R, on trigger T, owned by O, validated by policy V.*

### 4.2 Owning System
The system that authoritatively holds the source data. References `b2.205` (external systems) or `b2.204` (integrations). The owning system, not AISMM, is the source of truth for the underlying fact.

### 4.3 Connector Reference
A pointer to the access mechanism: an MCP server/tool, an API, an export. Modeled like `agent_contract_reference` (see `aismm-agent-contract-references.md`) — AISMM references the connector, it does not define it.

### 4.4 Extraction Recipe Reference
A pointer to a versioned recipe/prompt in `00-policies` (`kind_class: normative`). The recipe specifies what to read, how to transform, what record kind to emit, and the target layer.

### 4.5 Field Mapping
The declared mapping from source fields to record fields, enabling deterministic extraction and later diffing.

### 4.6 Trigger / Schedule
What causes ingestion: manual, scheduled (cron), or event-driven (e.g. a webhook from the owning system).

### 4.7 Ingestion Run
A provenance record of one execution: binding ref, recipe version, connector, agent identity, timestamp, inputs digest, produced/updated record IDs, and resulting `validation_status`. Stored here and/or projected into `b9.903`.

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Ingestion binding | ingestion_binding.* |
| Connector reference | connector_reference.* |
| Recipe reference | recipe_reference.* |
| Field mapping | field_mapping.* |
| Ingestion run | ingestion_run.* |

---

## 6. Required Structure

### 6.1 Ingestion Binding
- id, target_layers
- owning_system (`b2.205`/`b2.204` ref)
- connector_reference (MCP/tool/API)
- recipe_reference (`00-policies` recipe id + version)
- trigger/schedule
- owner_of_source (`b11.1102` ref)
- validation_policy (ref into `aismm-owner-validation-and-attestation.md`)

### 6.2 Field Mapping
- source field → record field
- transform notes / normalization

### 6.3 Ingestion Run
- id, binding ref, recipe version, connector, agent identity
- run timestamp, source snapshot/digest
- produced_record_ids, updated_record_ids
- resulting validation_status

---

## 7. Preferred Representation

- Markdown narrative + structured YAML binding block
- Recipe content itself stored separately in `00-policies` (referenced, not inlined)
- Connector references as stable identifiers (no secrets; secret refs are redacted per `aismm-context-security.md`)

---

## 8. Relationships

ingestion_binding → reads_from → b2.205.external_system | b2.204.integration
ingestion_binding → via → connector_reference
ingestion_binding → uses → recipe_reference → defined_in → 00-policies.recipe
ingestion_binding → populates → <target layer>
ingestion_binding → owned_by → b11.1102.owner
ingestion_run → ingested_from → ingestion_binding
ingestion_run → produced → <record> (records carry derived_from → ingestion_run)
ingestion_run → recorded_in → b9.903.source

(`ingested_from` / `derived_from` are added to the canonical taxonomy in `b9.906`.)

---

## 9. Cross-Layer Links

- `b2.204` / `b2.205` — the systems and integrations being read
- `00-policies` — the normative recipes/prompts referenced by bindings
- `b9.903` — provenance of each ingestion run
- `b11.1102` — source ownership (validation routing)
- `b9.905` — retrieval uses ingestion path for trust decisions
- `aismm-owner-validation-and-attestation.md` — validation gate applied to ingested records
- `aismm-ingestion-and-source-binding.md` — governance spec for this layer

---

## 10. Boundaries

Does NOT include:

- the recipe/prompt text itself (normative; lives in `00-policies`)
- connector/agent contract definitions (external; referenced only)
- the structural description of the source system (that is `b2.205`)
- the trust/validation rules themselves (governance doc); this layer applies them

---

## 11. Minimal Valid Content

- at least one ingestion binding for any layer that is agent-populated
- owning system, connector, and recipe references resolved
- owner_of_source assigned
- a validation policy reference

---

## 12. Completeness Criteria

- every agent-populated layer has at least one ingestion binding
- every binding references a versioned recipe in `00-policies`
- every binding names an owning system and source owner
- ingestion runs produce provenance linked from derived records
- no binding routes secret material into context (redaction enforced)

---

## 13. Summary

This layer answers **"how is each layer filled from the systems that own the data, and is that pipeline governed?"** It binds owning systems and MCP connectors to versioned extraction recipes and target layers, and records each ingestion run as provenance — the foundation for both automated population and source-owner validation.

<!-- AISMM:END -->
