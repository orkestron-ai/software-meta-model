<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 402
layer_key: domain_model_and_business_rules
document_id: spec.domain.model.rules
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Domain Model and Business Rules Layer Specification
<!-- AISMM:META_END -->

# 402 — Domain Model and Business Rules

## 1. Purpose of the Layer

The **Domain Model and Business Rules** layer defines the **core logic of the product domain** — what entities exist, how they behave, and which rules must always hold.

It describes:

- domain entities and attributes  
- relationships  
- invariants  
- state machines and lifecycles  
- business rules and policies  
- decision logic  

It answers:

- What are the core business objects?
- How do they behave over time?
- What rules must never be violated?
- How are decisions made?

---

## 2. Layer Role in AISMM

This layer connects:

Requirements → Domain Logic → System Behavior

It transforms:

- requirements → enforceable rules  
- entities → behavior  
- processes → state transitions  

Other layers rely on it to:

- implement correct behavior (b2, b3)  
- validate business logic  
- define constraints on data (202)  
- drive process execution (103)  

---

## 3. Main Output

- domain entities  
- attributes and relationships  
- business rules  
- invariants  
- state machines  
- decision models  

---

## 4. Core Concepts

### 4.1 Domain Entity

Represents a core business object.

---

### 4.2 Attribute

Property of a domain entity.

---

### 4.3 Relationship

Connection between domain entities.

---

### 4.4 Business Rule

Defines allowed or forbidden behavior.

---

### 4.5 Invariant (CRITICAL)

Rule that must ALWAYS hold true.

---

### 4.6 State

Represents condition of an entity.

---

### 4.7 State Transition

Defines allowed change of state.

---

### 4.8 Lifecycle

Full state model of an entity.

---

### 4.9 Domain Event (NEW)

Represents something that happened in domain.

---

### 4.10 Decision Model (NEW)

Defines structured decision logic:

- rules  
- conditions  
- outcomes  

---

### 4.11 Policy (NEW)

High-level rule governing behavior.

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Domain Entity | domain_entity.* |
| Attribute | domain_attribute.* |
| Relationship | domain_relationship.* |
| Business Rule | business_rule.* |
| Invariant | invariant.* |
| State | state.* |
| Transition | transition.* |
| Event | domain_event.* |
| Decision | decision_model.* |

---

## 6. Required Structure

### 6.1 Domain Entities
- id  
- attributes  
- relationships  

---

### 6.2 Attributes
- name  
- type  
- constraints  

---

### 6.3 Relationships
- type  
- cardinality  

---

### 6.4 Business Rules
- condition  
- outcome  

---

### 6.5 Invariants
- rule  
- validation  

---

### 6.6 Lifecycle
- states  
- transitions  

---

### 6.7 Decision Models
- inputs  
- rules  
- outputs  

---

## 7. Preferred Representation

- Markdown  
- JSON Schema / CSDL  
- decision tables  
- state diagrams  

---

## 8. Relationships

entity → has → attribute  
entity → relates_to → entity  
entity → follows → lifecycle  
rule → applies_to → entity  
transition → moves → state  

---

## 9. Cross-Layer Links

Requirements:
rule → enforces → requirement.*

Data:
entity → maps_to → entity.*

Processes:
transition → triggered_by → process_step.*

System:
rule → implemented_in → component.*

---

## 10. Boundaries

Does NOT include:

- UI  
- API  
- infrastructure  

---

## 11. Minimal Content

- 1 entity  
- 1 rule  

---

## 12. Completeness

- full domain model  
- rules defined  
- lifecycle defined  
- invariants defined  

---

## 13. Summary

Defines **core business logic and rules governing system behavior**.

<!-- AISMM:END -->
