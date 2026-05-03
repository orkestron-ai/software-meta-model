<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 201
layer_key: applications_and_system_architecture
document_id: spec.applications.system.architecture
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.1.0
title: Applications and System Architecture Layer Specification
<!-- AISMM:META_END -->

# 201 — Applications and System Architecture

## 1. Purpose of the Layer

The **Applications and System Architecture** layer defines how the product is decomposed into systems, applications, and components — and how responsibilities from business layers are mapped into executable system structure.

It describes:

- system boundaries
- applications and services
- internal components
- responsibility distribution
- execution ownership of business logic
- mapping of business services and process steps to components

It answers:

- Where does the product logic live?
- How is the system decomposed?
- Which components execute which responsibilities?
- How do business services map to system structure?
- How are systems structured and connected?

---

## 2. Layer Role in AISMM

This layer connects:

Business Architecture + Processes → System Structure

It transforms:

- capabilities → components  
- services → implementations  
- process steps → execution units  

Other layers rely on it to:

- define system boundaries
- assign ownership of logic
- prepare API design (203)
- prepare data ownership (202)
- support implementation (b3)

This layer does NOT define:

- API contracts (see 203)
- data schemas (see 202)
- infrastructure (see 301)
- UI
- code

---

## 3. Main Output of the Layer

- systems
- applications
- components
- modules
- deployment units
- responsibility mappings
- execution ownership

---

## 4. Core Concepts

### 4.1 System
Bounded unit of software.

---

### 4.2 Application
Deployable unit inside system.

---

### 4.3 Component / Service
Execution unit with clear responsibility.

---

### 4.4 Module
Logical grouping.

---

### 4.5 Deployment Unit
Runtime artifact.

---

### 4.6 Responsibility Mapping (NEW)
Defines which component executes:

- capability
- service
- process step
- critical step

---

### 4.7 Execution Ownership (NEW)
Defines which component is the **source of truth executor**.

---

### 4.8 Boundary Type (NEW)
Defines:

- internal
- external
- shared
- infrastructure
- third-party

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| System | system.* |
| Application | application.* |
| Component | component.* |
| Service | service.* |
| Module | module.* |
| Deployment | deployment_unit.* |

---

## 6. Required Structure

### 6.1 Systems
- id
- boundary
- owned capabilities

---

### 6.2 Applications
- system
- responsibilities

---

### 6.3 Components
- application
- responsibility
- execution ownership
- implements capabilities/services

---

### 6.4 Modules
- grouping

---

### 6.5 Deployment Units
- runtime structure

---

### 6.6 Responsibility Mapping
- component → capability
- component → service
- component → process step
- component → critical step

---

## 7. Preferred Representation

- Markdown
- C4 diagrams
- JSON/YAML

---

## 8. Relationships

system → contains → application  
application → contains → component  
component → grouped_in → module  
component → deployed_as → deployment_unit  

---

## 9. Cross-Layer Links

Business Architecture:
component → implements → business_service.*

Processes:
component → executes → step.*

Critical Path:
component → executes → critical_step.*

Value:
component → contributes_to → value.*

---

## 10. Boundaries

Does NOT include:

- API contracts
- data schemas
- infra configs

---

## 11. Minimal Content

- 1 system
- 1 component

---

## 12. Completeness

- full decomposition
- mapping to business/services/processes
- ownership clarity

---

## 13. Summary

Defines how business logic becomes executable system structure.

<!-- AISMM:END -->
