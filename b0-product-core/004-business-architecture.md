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
- business functions
- business services
- business entities and objects
- responsibility zones
- relationships between business elements

It answers:

- How is the product structured as a business system?
- What capabilities and services does the product provide?
- Which domains and functions exist within the product?
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
- map processes to capabilities and services
- connect value streams to business elements
- define ownership and responsibility
- ground requirements, controls, and APIs

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
- business functions
- business services
- business entities / objects
- ownership zones
- relationships between them

---

## 4. Core Concepts

### 4.1 Business Domain

A business domain is a logical area of responsibility within the product.

It groups related capabilities, services, and entities.

---

### 4.2 Business Capability

A capability defines what the product can do at a business level.

Independent of implementation.

---

### 4.3 Business Function

A business function represents a type of activity performed in the business.

Examples:

- registration
- processing
- validation
- calculation
- reporting
- notification

Functions are **stable categories of work**, not sequences.

---

### 4.4 Business Service

A business service is a concrete business-level service provided by the product.

Examples:

```text
service.create_account
service.verify_identity
service.calculate_invoice
```

Services:

- expose capabilities
- are used by processes
- are consumed by stakeholders or systems

---

### 4.5 Business Entity

A conceptual object in business logic.

Not a database table.

---

### 4.6 Business Role

A role that interacts with business services and capabilities.

---

### 4.7 Ownership Rule

Defines responsibility and ownership boundaries.

---

### 4.8 Business Relationship

Defines relationships:

- dependency
- composition
- usage
- ownership

---

## 5. Identifiable Entities

| Entity Type | Identifier Prefix |
|-------------|------------------|
| Domain | `domain.*` |
| Capability | `capability.*` |
| Function | `business_function.*` |
| Service | `business_service.*` |
| Entity | `entity.*` |
| Role | `business_role.*` |
| Ownership | `ownership.*` |
| Relationship | `relationship.*` |

---

## 6. Required Content Structure

### 6.1 Domains

- identifier
- description
- capabilities
- services
- ownership

---

### 6.2 Capabilities

- identifier
- description
- domain
- related entities
- supported functions

---

### 6.3 Business Functions

- identifier
- description
- domain
- related capabilities

---

### 6.4 Business Services

- identifier
- description
- provided capability
- inputs/outputs (conceptual)
- consumers

---

### 6.5 Business Entities

- identifier
- description
- role in business logic

---

### 6.6 Roles

- identifier
- description
- related services

---

### 6.7 Ownership

- ownership zones
- responsibilities
- shared ownership rules

---

### 6.8 Relationships

Define:

```text
domain → contains → capability
domain → contains → service
capability → supported_by → function
service → exposes → capability
service → uses → entity
```

---

## 7. Preferred Representation

Recommended:

- Markdown for structure
- diagrams (C4 / capability maps)
- structured JSON/YAML

---

## 8. Relationships Inside the Layer

```text
domain → contains → capability
domain → contains → service
capability → supported_by → business_function
service → exposes → capability
entity → used_by → service
role → uses → service
ownership → governs → domain
```

---

## 9. Relationships With Other AISMM Layers

### Value Streams

```text
value_segment → supported_by → business_service
value_segment → enabled_by → capability
```

---

### Stakeholders

```text
stakeholder → interacts_with → business_service
stakeholder → performs → business_role
```

---

### Processes

```text
process → uses → business_service
process_step → performs → business_function
```

---

### System Design

```text
component → implements → business_service
component → implements → capability
data_model → represents → entity
```

---

### Product Behavior

```text
requirement → targets → capability
control → implements → service
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
- service_definition
- function_definition

---

## 12. Minimal Valid Content

Must define:

- at least one domain
- at least one capability
- at least one service

---

## 13. Completeness Criteria

A mature model includes:

- full domain map
- capability map
- service catalog
- entity map
- ownership structure

---

## 14. Example Identifiers

```text
domain.workflow
capability.execute_workflow
business_function.processing
business_service.execute_task
entity.task
business_role.operator
```

---

## 15. Summary

This layer defines **the business structure of the product**.

It provides a stable map of domains, capabilities, functions, and services that:

- supports architecture
- grounds processes
- connects value to implementation

<!-- AISMM:END -->
