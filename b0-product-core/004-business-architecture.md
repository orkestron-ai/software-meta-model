<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 004
layer_key: business_architecture
document_id: spec.business.architecture
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Business Architecture Layer Specification
<!-- AISMM:META_END -->

# 004 — Business Architecture

## 1. Purpose of the Layer

The **Business Architecture** layer defines how the product is structured from a business perspective.

It describes:

- business domains
- business capabilities
- business-level entities
- responsibility zones
- relationships between business elements

It answers:

- How is the product structured as a business system?
- What capabilities does the product provide?
- Which domains exist within the product?
- How are responsibilities distributed?
- What are the stable structural elements of the business?

---

## 2. Layer Role in AISMM

This layer provides the **static business structure** of the product.

It connects:

```text
Product Definition → Structure of Business Logic
```

Other layers rely on it to:

- define system architecture boundaries
- map processes to capabilities
- connect value streams to business elements
- define ownership and responsibility
- ground requirements and controls

This layer does NOT define:

- execution order (processes)
- implementation details
- UI or UX
- runtime behavior

It defines **what exists**, not **how it runs**.

---

## 3. Main Output of the Layer

The output is a structured model of:

- business domains
- business capabilities
- core business entities
- ownership zones
- relationships between them

---

## 4. Core Concepts

### 4.1 Business Domain

A business domain is a logical area of responsibility within the product.

A domain groups related capabilities and entities.

Examples:

```text
domain.workflow_management
domain.billing
domain.user_management
```

Domains should be:

- stable
- meaningful
- non-overlapping (as much as possible)

---

### 4.2 Business Capability

A capability defines what the product can do at a business level.

It is independent of implementation.

Examples:

```text
capability.execute_workflow
capability.assign_task
capability.calculate_billing
```

Capabilities should be:

- atomic enough to reason about
- reusable across processes
- mapped to domains

---

### 4.3 Business Entity

A business entity is a core concept used in the business logic.

It is not a database table, but a conceptual object.

Examples:

```text
entity.workflow
entity.task
entity.invoice
entity.subscription
```

Entities:

- participate in capabilities
- are used in processes
- are referenced across layers

---

### 4.4 Ownership Zone

Defines responsibility for domains and capabilities.

Examples:

```text
ownership.product_team
ownership.billing_team
ownership.external_provider
```

---

### 4.5 Business Relationship

Defines relationships between business elements.

Examples:

- dependency
- composition
- usage
- responsibility

---

## 5. Identifiable Entities

| Entity Type | Identifier Prefix |
|------------|------------------|
| Domain | domain.* |
| Capability | capability.* |
| Entity | entity.* |
| Ownership | ownership.* |
| Relationship | relationship.* |

---

## 6. Required Content Structure

---

### 6.1 Business Domains

For each domain:

- identifier
- name
- description
- owned capabilities
- related entities
- ownership

---

### 6.2 Business Capabilities

For each capability:

- identifier
- name
- description
- domain
- inputs / outputs (conceptual)
- related entities

---

### 6.3 Business Entities

For each entity:

- identifier
- name
- description
- role in business logic
- related capabilities

---

### 6.4 Ownership

Define:

- who owns domains
- who owns capabilities
- shared responsibilities

---

### 6.5 Relationships

Define relationships such as:

- domain → contains → capability
- capability → uses → entity
- domain → depends_on → domain
- capability → depends_on → capability

---

## 7. Preferred Representation

The semantic content of this layer is independent of any specific representation format.

This layer defines **who the stakeholders are, what they want, and how they perceive value**, not how this information must be structured or stored.

Due to the narrative and conceptual nature of this layer, the following representations are considered most suitable:

- Markdown (`.md`) for structured descriptions of stakeholders, goals, motivations, and relationships  
- Structured formats (JSON / YAML) for machine-readable stakeholder definitions and mappings  
- Tables or simple diagrams for visualizing relationships, dependencies, and conflicts  

These representations are **recommendations, not requirements**.

Implementations:

- SHOULD use Markdown as the primary format for clarity and human understanding  
- MAY use structured formats when machine processing or integration is required  
- MAY use visual representations to improve comprehension of relationships  
- MUST NOT encode semantics in format-specific constructs  

The correctness of this layer is determined by the completeness and consistency of its **semantic content**, not by the chosen representation format.

---


## 8. Relationships Inside the Layer

```text
domain → contains → capability
capability → uses → entity
entity → participates_in → capability
domain → owned_by → ownership
capability → owned_by → ownership
```

---

## 9. Relationships With Other AISMM Layers

### Product Definition & Context

```text
domain → maps_to → module.*
```

---

### Value Streams

```text
capability → implements → value transformation
domain → groups → converters
```

---

### Stakeholders

```text
stakeholder → interacts_with → capability
```

---

### Critical Path

```text
critical_step → uses → capability
```

---

### Product Behavior

```text
control → implements → capability
requirement → targets → capability
```

---

### System Design

```text
component → implements → capability
service → maps_to → domain
data_model → represents → entity
```

---

### P&L Model

```text
capability → cost_driver
capability → revenue_driver
```

---

## 10. Layer Boundaries

This layer must not include:

- process flows
- UI details
- API definitions
- database schemas
- execution logic

---

## 11. Recommended Block Types

- layer_document
- domain_definition
- capability_definition
- entity_definition
- ownership_definition

---

## 12. Minimal Valid Content

Must define:

- at least one domain
- at least one capability
- at least one entity

---

## 13. Completeness Criteria

A mature model includes:

- full domain map
- capability map
- entity map
- ownership structure
- dependency graph

---

## 14. Example Identifiers

```text
domain.workflow
domain.billing

capability.execute_workflow
capability.calculate_charge

entity.workflow
entity.task
entity.invoice

ownership.product_team
ownership.finance_team
```

---

## 15. Summary

This layer defines **the business structure of the product**.

It provides a stable map of domains, capabilities, and entities that:

- supports architecture
- grounds processes
- connects value to implementation

<!-- AISMM:END -->
