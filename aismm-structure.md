# AISMM Structure Specification

## Overview

AISMM (AI-driven Software Meta-Model) is a **block-based, layer-aware, bundle-organized, machine-readable meta-model embedded in human-readable project materials**.

AISMM is designed for enterprise-grade, AI-native and agent-assisted software development operating in large-context environments.

The core principle:

> AISMM is not file-based.  
> AISMM is **block-based** and extracted from arbitrary files.

Files, folders, repositories and preferred formats are containers and representations. The real model is formed by AISMM blocks, their metadata, entities, references and the graph built from them.

---

## 1. Core Concept

### 1.1 File as Container

Any file (`.md`, `.txt`, `.json`, `.yaml`, `.bpmn`, `.xml`, `.fig`, etc.) may be treated as a **container of AISMM blocks or AISMM-compatible representations**.

A Markdown file may contain one or many AISMM blocks. A structured file such as JSON, BPMN, OpenAPI, AsyncAPI, CSDL or VSS JSON may act as a preferred representation for a layer or a part of a layer.

AISMM therefore does not depend on one fixed file layout. The same product model may be distributed across:

- one repository
- multiple repositories
- module-local `.smm` folders
- root-level `.smm` folders
- external schema or diagram files
- generated indexes and graph artifacts

---

### 1.2 AISMM and AIPLMM

AISMM operates inside a higher-level architectural context governed by **AIPLMM (AI Product Landscape Meta-Model)**.

The distinction is:

```text
AISMM  → describes one product/system and its internal software meta-model
AIPLMM → describes a landscape of products, systems, shared capabilities and cross-product dependencies
```

AIPLMM may describe:

- relationships across multiple products
- shared domains and cross-cutting capabilities
- end-to-end business processes spanning several AISMMs
- shared test strategies
- shared runtime and compliance contexts
- product landscape dependencies
- common architecture or security constraints

AISMM remains the product-level source of structured context. AIPLMM connects multiple AISMMs into a product landscape graph.

---

### 1.3 AISMM Block

The fundamental unit of AISMM is a **block**.

```text
<!-- AISMM:BEGIN -->
...metadata...
<!-- AISMM:META_END -->

...human-readable or machine-readable content...

<!-- AISMM:END -->
```

A block may describe:

- a layer specification
- a domain entity
- a rule
- a constraint
- an assumption
- a reference to an external structured model
- a prompt or execution instruction
- an extracted knowledge unit
- a traceability relation

---

## 2. Block Structure

### 2.1 Block Markers

| Marker | Description |
|---|---|
| `AISMM:BEGIN` | Start of AISMM block |
| `AISMM:META_END` | End of metadata section |
| `AISMM:END` | End of AISMM block |

---

### 2.2 Canonical Form

```text
<!-- AISMM:BEGIN -->
type: <block_type>
layer_id: <3-digit>
layer_key: <string>
document_id: <unique_id>
document_type: <type>
module_scope: <scope>
status: <status>
spec_version: <version>
title: <optional>
references:
  - <refs>
<!-- AISMM:META_END -->

# Human-readable content

...

<!-- AISMM:END -->
```

---

### 2.3 Metadata Format

The metadata section SHOULD be YAML-compatible.

It must be easy to parse without interpreting the human-readable body.

Recommended metadata properties:

```yaml
type: layer_specification
layer_id: "401"
layer_key: requirements
document_id: spec.requirements
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Requirements Layer Specification
references:
  - b4.401.requirement.user_login
```

---

## 3. Required Metadata Fields

| Field | Description |
|---|---|
| `type` | Block type |
| `layer_id` | 3-digit layer identifier |
| `layer_key` | Stable layer key |
| `document_id` | Globally unique document/block identifier |
| `document_type` | Logical type of document |
| `module_scope` | `root` or `module:<module_id>` |
| `status` | Lifecycle state |
| `spec_version` | Version of the specification/block format or layer specification |

---

## 4. Optional Metadata Fields

Optional fields MAY include:

- `title`
- `references`
- `tags`
- `owner`
- `priority`
- `version`
- `aliases`
- `previous_ids`
- `replaced_by`
- `supersedes`
- `confidence`
- `source`
- `security_classification`

---

## 5. Block Types

### 5.1 Core Types

| Type | Description |
|---|---|
| `layer_specification` | Specification of what a layer must describe |
| `layer_document` | Main document block for a layer |
| `entity_definition` | Entity description |
| `rule_definition` | Rule description |
| `constraint_definition` | Constraint description |
| `assumption_definition` | Assumption description |
| `schema_reference` | Reference to JSON/YAML/BPMN/OpenAPI/etc. |
| `knowledge_unit` | Extracted compact knowledge item |
| `trace_link` | Explicit relation between AISMM entities |
| `prompt` | Execution prompt or reusable instruction |

---

### 5.2 Specialized Types

Specialized types MAY include:

- `value_model`
- `economics_model`
- `event_definition`
- `control_definition`
- `api_contract`
- `data_contract`
- `decision_record`
- `risk_record`
- `test_case`
- `release_record`
- `retention_policy`
- `retrieval_unit`

---

## 6. Layer Identification

Layer identifiers use a 3-digit numeric format:

```text
001–999
```

Layer IDs are stable and are used for:

- document organization
- schema references
- traceability
- context retrieval
- validation

Examples:

```text
001 — Product Definition and Context
201 — Applications and System Architecture
806 — Knowledge Retention and History Compaction
905 — Context Retrieval and RAG
```

---

## 7. Bundle Identification

AISMM layers are grouped into bundles.

Canonical bundle range:

```text
b0–b12
```

Current AISMM bundles:

```text
b0  — Product Core
b1  — Business Dynamics
b2  — System Design
b3  — Implementation
b4  — Product Behavior
b5  — User Interaction
b6  — Runtime Operations
b7  — Quality, Risk & Compliance
b8  — Change Execution
b9  — Knowledge Traceability
b10 — Data and AI Lifecycle
b11 — Organization, Ownership and Governance
b12 — FinOps and Technical Economics
```

---

## 8. Current Layer Map

### b0 — Product Core

```text
001 — Product Definition and Context
002 — Æilus Value Streams
003 — Stakeholders and Motivation
004 — Business Architecture
005 — Critical Path
006 — Economics Model
```

### b1 — Business Dynamics

```text
101 — Business Hypotheses
102 — Strategy and Product Management
103 — Processes
```

### b2 — System Design

```text
201 — Applications and System Architecture
202 — Data and Information Architecture
203 — API and Interfaces
204 — Integrations
205 — External Systems and Ecosystem Surface
206 — Event Catalog and Event Mesh
207 — Bounded Contexts and Domain Architecture
```

### b3 — Implementation

```text
301 — Technology Architecture
302 — Code and Implementation
303 — Build, Deployment and Runtime Artifacts
304 — Dependency Inventory, SBOM and Reproducibility
305 — Configuration, Feature Flags and Environment Variants
```

### b4 — Product Behavior

```text
401 — Requirements
402 — Domain Model and Business Rules
403 — Access Rights
404 — NFR and Quality Attributes
405 — State Machines and Lifecycles
406 — Behavioral Contracts and Invariants
407 — Error Taxonomy and Failure Behavior
```

### b5 — User Interaction

```text
501 — User Scenarios and UX Logic
502 — Interface Structure and Navigation
503 — Screens, Forms and UI States
504 — Design System and UI Components
505 — Accessibility, Localization and Notifications
```

### b6 — Runtime Operations

```text
601 — Runtime Environment and Topology
602 — Observability and Monitoring
603 — Incident Management and Response
604 — SLA, SLO and Operational Governance
605 — Capacity, Scaling and Performance Engineering
606 — Disaster Recovery, Backup and Continuity
607 — Operational Readiness and Drills
```

### b7 — Quality, Risk & Compliance

```text
701 — Quality Assurance and Testing
702 — Risk Management
703 — Security and Privacy
704 — Compliance and Audit
705 — Threat Modeling and Attack Surface
706 — Vulnerability Management and Disclosure
707 — Privacy Rights and DPIA
```

### b8 — Change Execution

```text
801 — Work Items and Change Requests
802 — Planning and Delivery Flow
803 — Testing and Acceptance Execution
804 — Release, Version and Rollout Management
805 — Change History and Decision Log
806 — Knowledge Retention and History Compaction
807 — Feedback Loops and Learning Cycles
808 — CI/CD Pipeline and Automation
809 — Migration, Backfill and Long-Running Refactors
```

### b9 — Knowledge Traceability

```text
901 — Knowledge Index and Navigation
902 — Traceability Graph
903 — Source Provenance and Confidence
904 — Context Coverage and Consistency
905 — Context Retrieval and RAG
906 — Ontology, Vocabulary and Relationship Taxonomy
```

### b10 — Data and AI Lifecycle

```text
1001 — Data Products and Contracts
1002 — Data Lineage and Schema Evolution
1003 — Feature and Embedding Lifecycle
1004 — Model Registry and Versioning
1005 — Training, Evaluation and Experimentation
1006 — Drift, Monitoring and Retraining
1007 — Labeling, Annotation and Ground Truth
1008 — Data Quality and Observability
```

### b11 — Organization, Ownership and Governance

```text
1101 — Team Topology and RACI
1102 — Ownership Graph
1103 — Decision Rights and Governance
1104 — Skills and Capability Matrix
1105 — Vendors and External Responsibilities
1106 — Capacity and Allocation
```

### b12 — FinOps and Technical Economics

```text
1201 — Cost Allocation and Showback
1202 — Capacity and Commitment Management
1203 — Token and Inference Economics
1204 — Cost of Quality and Incident
1205 — Unit Economics by Service, Tenant and Request
1206 — Carbon and Sustainability Accounting
```

---

## 9. Module Scope

AISMM supports both root-level and module-level meta-models.

```text
root
module:<module_id>
```

Examples:

```text
root
module:billing
module:auth
module:mobile.ios
```

A root AISMM describes the product as a whole.

A module AISMM describes a bounded part of the product and may override, extend or specialize root-level context.

---

## 10. Document Identity

Every AISMM block MUST have a stable `document_id`.

Document identity must be:

- globally unique within the repository or product AISMM
- stable across file moves
- stable across formatting changes
- independent of folder structure

Examples:

```text
spec.product.definition.context
spec.value.streams
product.definition.context
value.streams.main
critical.path.primary
```

`document_id` identifies the block/document. Entity IDs identify specific modeled objects inside or across blocks.

---

## 11. Unified Entity ID Strategy

AISMM SHOULD use a global entity ID format for cross-layer and cross-bundle references:

```text
b{bundle}.{layer}.{entity_type}.{slug}
```

Examples:

```text
b4.401.requirement.user_login
b2.201.component.auth_service
b8.801.work_item.fix_login_timeout
b9.902.trace_link.requirement_to_component
```

### 11.1 ID Rules

- IDs must be stable.
- IDs must not depend on file location.
- IDs should not change when titles or descriptions change.
- IDs should use singular entity type names.
- IDs should use lowercase slugs with underscores.
- Global IDs SHOULD be used for all long-lived and cross-layer references.

### 11.2 Local IDs

Local IDs are allowed for authoring convenience:

```text
requirement.user_login
component.auth_service
```

But cross-layer references SHOULD use global IDs:

```text
b4.401.requirement.user_login
b2.201.component.auth_service
```

---

## 12. Referencing

### 12.1 Entity References

References point to entities defined in AISMM blocks.

Preferred global format:

```text
b{bundle}.{layer}.{entity_type}.{slug}
```

Example:

```yaml
references:
  - b4.401.requirement.user_login
  - b2.201.component.auth_service
  - b8.801.work_item.fix_login_timeout
```

Legacy/local references MAY be used temporarily:

```text
001-entity.product_scope
105-event.execution_completed
310-control.assign_task
```

---

### 12.2 External Files

External files are referenced using `file:`.

```yaml
references:
  - file:prefs/vss/vss.schema.json
  - file:prefs/pnl/pnl.schema.json
  - file:prefs/bpmn/process.bpmn
```

---

### 12.3 External Repositories

For layers isolated in separate repositories, references must include repository identifiers.

```yaml
references:
  - repo:secure-core-layer/b7.703.security_control.secret_rotation
  - repo:finance-models/b0.006.economics_model.product_pnl
```

---

### 12.4 URLs

External web resources may be referenced using `url:`.

```yaml
references:
  - url:https://example.com/specification
```

---

## 13. Supported File Formats

| Format | Use |
|---|---|
| `.md` | default human-readable specification |
| `.json` | schema/data/registries |
| `.yaml` / `.yml` | configs and structured metadata |
| `.bpmn` | business processes |
| `.xml` | structured models |
| `.fig` | Figma/UI models |
| `.svg` | diagrams |
| `.png` | exported diagrams |
| `.openapi.yaml` | REST API contracts |
| `.asyncapi.yaml` | event/API contracts |
| `.graphql` | GraphQL schemas |

---

## 14. Preferred Representation

AISMM distinguishes between:

```text
semantic layer content
preferred representation
storage format
```

A layer specification describes **what must be represented**.

A preferred representation describes **which format is most convenient for humans and tools**.

Examples:

```text
VSS JSON is preferred for value streams because it can be visualized as one large scheme.
BPMN is preferred for business processes.
OpenAPI is preferred for REST APIs.
AsyncAPI is preferred for event-based APIs.
CSDL / JSON Schema may be preferred for data models.
```

Preferred representation is defined inside each layer specification, not in a separate universal `aismm-layers-prefs.md` file.

---

## 15. Folder Structure

AISMM does NOT depend on folder structure, but it supports a recommended repository layout.

### 15.1 Root Structure

```text
repo/
  README.md
  aismm-structure.md
  aismm-security.md
  aismm-unified-id-strategy.md

  b0-product-core/
    README.md
    b0.schema.json
    001-product-definition-context.md
    002-aeilus-value-streams.md
    ...

  b1-business-dynamics/
    README.md
    b1.schema.json
    ...

  b9-knowledge-traceability/
    README.md
    b9.schema.json
    901-knowledge-index-and-navigation.md
    ...
```

---

### 15.2 Module Structure

```text
modules/
  billing/
    .smm/
      README.md
      b0-product-core/
      b2-system-design/
      b4-product-behavior/
      b8-change-execution/
```

---

### 15.3 Hybrid Structure

```text
repo/
  README.md
  docs/
  src/
  .smm/
  b0-product-core/
  b1-business-dynamics/
```

---

### 15.4 Distributed Structure

Layers can be isolated into separate repositories for security, scale or ownership reasons.

```text
main-repo/
  b0-product-core/
    002-aeilus-value-streams.md

secure-repo/
  b7-quality-risk-compliance/
    703-security-and-privacy.md
```

Distributed structures require explicit `repo:` references.

---

## 16. Security and Restricted Layers

AISMM supports restricted, encrypted or isolated layers.

Reasons:

- confidential product economics
- security architecture
- personal data
- compliance evidence
- credentials or sensitive operational context

AISMM parsers SHOULD support:

1. discovering distributed sources
2. authenticating access
3. decrypting restricted layers
4. preserving access boundaries
5. excluding inaccessible content from unauthorized context packages

Restricted data must never be silently inserted into AI context.

---

## 17. Granularity Rules

AISMM supports several granularity modes:

### 17.1 Single File

All blocks are kept in one file.

Useful for:

- prototypes
- small products
- examples

---

### 17.2 Layer Files

Each layer has a dedicated file.

Useful for:

- standard AISMM repositories
- medium-size products
- version control clarity

---

### 17.3 Multi-Document Layer

One layer may be represented by multiple files.

Useful for:

- large process catalogs
- large API surfaces
- large requirement sets
- distributed teams

---

### 17.4 Modular AISMM

Each module may have its own `.smm` submodel.

Useful for:

- microservices
- monorepos
- product platforms
- teams with separate ownership

---

## 18. Entity Identification

Each block may define entities using stable identifiers.

Examples:

```text
entity.*
event.*
control.*
value.*
metric.*
requirement.*
component.*
work_item.*
decision.*
trace_link.*
retrieval_unit.*
```

Entity IDs SHOULD follow the unified ID strategy for long-lived references.

---

## 19. Parsing Rules

Any context extraction tool implementing AISMM must:

1. Fetch all configured sources.
2. Pull external repositories if distributed AISMM is used.
3. Authenticate and decrypt restricted layers if access is granted.
4. Scan supported files.
5. Extract AISMM blocks.
6. Parse metadata.
7. Validate required fields.
8. Extract entities and references.
9. Resolve local and global references.
10. Build a graph model.
11. Attach provenance and confidence where available.
12. Build or update knowledge indexes.
13. Prepare retrieval units for RAG if required.
14. Produce a global AISMM graph artifact.

---

## 20. Validation Rules

### 20.1 Mandatory Validation

- `AISMM:BEGIN` present
- `AISMM:META_END` present
- `AISMM:END` present
- required metadata fields exist
- valid `layer_id`
- valid `layer_key`
- unique `document_id`
- valid `module_scope`

---

### 20.2 Recommended Validation

- valid references
- no broken global IDs
- consistent module scope
- schema compliance
- no duplicate active entity IDs
- layer content follows its specification
- preferred representation files exist when referenced
- security classification is respected
- provenance exists for critical knowledge
- confidence is assigned where needed

---

## 21. Layer Content Rules

Each layer specification defines:

- purpose of the layer
- role in AISMM
- main output
- core concepts
- identifiable entities
- required structure
- preferred representation
- relationships
- cross-layer links
- boundaries
- minimal content
- completeness criteria

Layer content must describe **semantic meaning first**. Preferred formats such as BPMN, VSS JSON or OpenAPI are representations, not the semantic model itself.

---

## 22. Layer Specifications

Each layer has its own specification.

Recommended location:

```text
/bundles/<bundle>/<layer>/spec.md
```

Repository-friendly location may be:

```text
/b0-product-core/001-product-definition-context.md
/b1-business-dynamics/103-processes.md
/b9-knowledge-traceability/905-context-retrieval-and-rag.md
```

A layer specification defines what must be represented, not only how it is stored.

---

## 23. Composition

AISMM composition:

```text
blocks → entities → layers → bundles → repository/module → product AISMM graph → product landscape graph
```

Generated artifacts may include:

```text
aismm.json
aismm.graph.json
aismm.index.json
aismm.traceability.json
aismm.health.json
```

---

## 24. Decomposition

AISMM decomposition:

```text
global graph → bundle views → layer views → blocks → source files
```

This allows the same AISMM to be used by:

- humans reading Markdown
- AI agents retrieving context
- validators checking consistency
- graph tools performing impact analysis
- CI/CD pipelines enforcing rules

---

## 25. AISMM and RAG

AISMM is designed to support RAG, but AISMM is not only a RAG store.

The RAG pipeline may use:

```text
blocks → retrieval units → embeddings → ranked context → context package
```

The retrieval layer should preserve:

- source references
- entity IDs
- trace links
- confidence
- missing context
- consistency warnings

RAG must not flatten AISMM into anonymous chunks. Retrieved context must remain connected to the semantic graph.

---

## 26. AISMM Graph

AISMM is ultimately a semantic graph.

```text
nodes = AISMM entities
edges = explicit references / trace links / derived relationships
paths = reasoning and impact analysis
```

The graph enables:

- impact analysis
- root cause analysis
- change analysis
- coverage analysis
- context assembly
- consistency checking
- agent reasoning

---

## 27. Versioning and Conformance

AISMM is versioned using **semantic versioning** (`MAJOR.MINOR.PATCH`).

Current version: **AISMM 2.0.0**

### Layer Lifecycle

Every layer specification carries a `status` field:

```text
draft → proposed → accepted → stable → deprecated → superseded → archived
```

Withdrawn layers exit the lifecycle at any point before `stable`.

### Conformance Levels

AISMM defines five staged conformance levels. Each level builds on the previous one.

| Level | Name | Key Requirement |
|-------|------|-----------------|
| **L1** | Block Format | `aismm_meta` headers present, Markdown-first format |
| **L2** | Typed Entities | Stable IDs, declared entity types, layer assignment |
| **L3** | Traceability Graph | Typed cross-layer links, b9 populated, basic traceability |
| **L4** | RAG-Ready Context | `summary` fields, bounded chunk sizes, retrieval metadata |
| **L5** | Agent-Grade Governance | Audit trail, agent scopes, source provenance, no orphans |

A repository declares its conformance level in its README or `aismm-manifest.yaml`.

### Extension Namespace

Safe extensions use the prefix:

```text
x-<organization>.<domain>.<entity>
```

Example: `x-datsteam.gameton.reward_model`

AISMM tooling must ignore unknown extension fields.

See [`aismm-versioning-and-conformance.md`](./aismm-versioning-and-conformance.md) for the complete specification including migration policy and migration record format.

---

## 28. Relationship to Agent Contracts

AISMM does not define agent contracts directly.

Agent capabilities, result contracts and execution interfaces should be defined in separate contract repositories.

AISMM provides the structured product context that agents may consume or update under controlled rules.

```text
AISMM = product/system knowledge
Agent contracts = how agents work with that knowledge
```

---

## 29. Core Principle

> AISMM is a semantic graph embedded in human-readable and machine-readable project artifacts.

Or shorter:

```text
Markdown is the interface.
Blocks are the unit.
Graph is the model.
Knowledge is the result.
```

---

## 30. Summary

AISMM provides:

- machine-readable blocks
- human-readable documentation
- flexible file structure
- strong typing via metadata
- stable global identifiers
- cross-layer graph linking
- preferred representations per layer
- secure multi-repo distribution
- encrypted or restricted layer support
- RAG-ready retrieval units
- provenance and confidence
- context coverage and consistency checks

AISMM is a **block-based, layer-aware, bundle-organized, graph-structured meta-model embedded in project artifacts**, designed for AI-native software development, full-context development and agent-assisted engineering.
