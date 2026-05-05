# Bundle 2 — System Design (Systems, Data, Interfaces & Integrations)

## Overview

Bundle 2 defines **how the product is realized as a working system in practice**.

It transforms:

- business architecture → system structure  
- processes → execution components  
- business entities → data models  
- interactions → API contracts  
- contracts → real integrations  

It answers:

- How is the system decomposed?
- Who executes business logic?
- Where is data owned?
- How do components interact?
- How do systems communicate in reality?

---

## Layers Overview

### 201 — Applications and System Architecture
Defines **execution structure**:

- systems and boundaries  
- applications and components  
- modules and deployment units  
- responsibility mapping  
- execution ownership (source of truth executors)  

---

### 202 — Data and Information Architecture
Defines **system state and data model**:

- data domains  
- entities and attributes  
- relationships  
- ownership (source of truth)  
- data flows  
- lifecycle and states  
- consistency rules  

---

### 203 — API and Interfaces
Defines **interaction contracts**:

- APIs and operations  
- requests and responses  
- commands and events  
- contracts and versioning  
- interaction patterns (sync / async)  

---

### 204 — Integrations
Defines **real runtime communication**:

- integrations between systems  
- external systems  
- protocols and channels  
- messages and events  
- reliability and delivery guarantees  
- integration topology  

---

## Role of Bundle 2

Bundle 2 provides a coherent execution model:

Systems → Data → Contracts → Communication

More precisely:

Structure → State → Interaction → Runtime

It is used as:

- architecture foundation  
- bridge between business and implementation  
- source of truth for system design  
- base for APIs and integrations  
- input for implementation (b3)  

---

## Relationship with Other Bundles

### Depends on Bundle 0 (Product Core)
- implements business architecture (004)  
- supports value streams (002)  
- constrained by economics (006)  

### Depends on Bundle 1 (Business Dynamics)
- executes processes (103)  
- supports initiatives (102)  
- evolves based on hypotheses (101)  

---

## Key Principles

### 1. Execution Ownership

Every responsibility must have a clear executor:

component → source_of_truth_executor → logic/data

---

### 2. Separation of Concerns

- 201 → structure  
- 202 → data  
- 203 → contracts  
- 204 → runtime communication  

---

### 3. Data as System State

Data is:

- owned  
- versioned  
- lifecycle-driven  
- consistent (with defined guarantees)  

---

### 4. Contracts Before Integration

- API defines interaction  
- Integration defines execution  

API (203) ≠ Integration (204)

---

### 5. Distributed System Reality

System must model:

- async communication  
- retries and failures  
- consistency trade-offs  
- topology  

---

### 6. Traceability

Everything must link back:

Process → Component → API → Integration → Data

---

## Data Responsibility Boundary

```text
b2 = WHAT the data model is (structure, schema, entities, relationships)
b3 = HOW it is implemented, packaged and deployed (migrations, artifacts, code)
b10 = HOW data assets evolve, flow, are measured, used for AI/ML and governed over time
```

Examples:

```text
data_entity.customer                    → defined in b2.202
database_migration.add_customer_status  → implemented in b3.304 / b8.809
schema_version.customer_v2             → governed in b10.1002
data_quality_rule.customer_status_not_null → governed in b10.1008
```

---

## Summary

Bundle 2 defines how the product becomes a **real, executable, distributed system**.

It connects structure, data, contracts, and runtime communication into a single coherent architecture model.
