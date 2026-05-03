<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 202
layer_key: data_and_information_architecture
document_id: spec.data.information.architecture
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.1.0
title: Data and Information Architecture Layer Specification
<!-- AISMM:META_END -->

# 202 — Data and Information Architecture

## 1. Purpose of the Layer

The **Data and Information Architecture** layer defines how information is structured, owned, transformed, and flows across the system.

It describes:

- data domains
- entities and attributes
- relationships between entities
- ownership and source of truth
- data flows between components
- lifecycle and state transitions of data
- data consistency and synchronization

It answers:

- What data exists in the product?
- How is it structured and related?
- Who owns the data (source of truth)?
- How does data move across the system?
- How does data evolve over time?
- How does data support value creation and execution?

---

## 2. Layer Role in AISMM

This layer connects:

Business Meaning → Data → System Execution

It transforms:

- business entities → data entities  
- process execution → data lifecycle  
- system components → data ownership  

Other layers rely on it to:

- define input/output structures (APIs)
- define ownership for system components (201)
- support process execution (103)
- enable analytics and economics (006)

This layer does NOT define:

- database engines
- storage infrastructure
- API endpoints
- UI representation

---

## 3. Main Output of the Layer

- data domains
- entities and attributes
- relationships
- ownership model
- data flows
- lifecycle definitions
- data consistency rules

---

## 4. Core Concepts

### 4.1 Data Domain
Logical grouping of related data.

---

### 4.2 Entity
Core data object representing business concept.

---

### 4.3 Attribute
Property of an entity.

---

### 4.4 Relationship
Connection between entities.

---

### 4.5 Data Flow
Movement of data across components or processes.

---

### 4.6 Ownership (Source of Truth) (ENHANCED)
Defines:

- which component owns the data
- where data is authoritative
- who can modify it

---

### 4.7 Data Lifecycle (NEW)
Defines states and transitions:

- created
- updated
- consumed
- archived
- deleted

---

### 4.8 Data State (NEW)
Represents current condition of entity.

---

### 4.9 Data Consistency (NEW)
Defines:

- strong consistency
- eventual consistency
- synchronization rules

---

### 4.10 Data Contract (NEW)
Defines expected structure exchanged between components.

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Data Domain | data_domain.* |
| Entity | entity.* |
| Attribute | attribute.* |
| Relationship | relationship.* |
| Data Flow | data_flow.* |
| Dataset | dataset.* |
| Data State | data_state.* |

---

## 6. Required Structure

### 6.1 Data Domains
- id
- description
- entities

---

### 6.2 Entities
- id
- domain
- attributes
- states (optional)
- lifecycle

---

### 6.3 Attributes
- name
- type
- meaning
- constraints

---

### 6.4 Relationships
- type
- cardinality

---

### 6.5 Ownership
- component owner
- source of truth
- access rules

---

### 6.6 Data Flows
- source component
- target component
- entities transferred

---

### 6.7 Lifecycle
- states
- transitions
- triggers

---

### 6.8 Consistency Rules
- consistency type
- synchronization rules

---

## 7. Preferred Representation

- Markdown
- ER diagrams
- JSON/YAML

---

## 8. Relationships

entity → belongs_to → data_domain  
entity → has → attribute  
entity → relates_to → entity  
data_flow → transfers → entity  
component → owns → entity  

---

## 9. Cross-Layer Links

Business Architecture:
entity → represents → business_entity.*

Processes:
process → reads/writes → entity

System Architecture:
component → owns → entity  
component → reads/writes → entity  

Value:
entity → carries → value.*

Economics:
entity → used_for → metric.*

---

## 10. Boundaries

Does NOT include:

- DB technology
- storage config
- API endpoints

---

## 11. Minimal Content

- 1 entity
- 1 domain

---

## 12. Completeness

- full entity map
- ownership defined
- lifecycle defined
- flows defined

---

## 13. Summary

Defines how data becomes a controlled, consistent, and owned system resource.

<!-- AISMM:END -->
