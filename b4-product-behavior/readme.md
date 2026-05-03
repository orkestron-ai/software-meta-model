# Bundle 4 — Product Behavior (Requirements, Domain Rules, Access & Quality)

## Overview

Bundle 4 defines **what the system must do, how it behaves, who is allowed to act, and how well it must perform**.

It is the **behavioral contract of the product**.

It transforms:

- strategy → requirements  
- requirements → domain rules  
- rules → allowed actions  
- actions → constrained by quality  

---

## Layers Overview

### 401 — Requirements

Defines **what must happen**:

- functional requirements  
- business requirements  
- constraints  
- acceptance criteria  
- scenarios  

---

### 402 — Domain Model and Business Rules

Defines **how behavior works internally**:

- domain entities  
- business rules  
- invariants  
- state machines  
- lifecycles  
- decision logic  

---

### 403 — Access Rights

Defines **who is allowed to act**:

- actors and roles  
- permissions  
- access policies (RBAC / ABAC)  
- conditions and scopes  

---

### 404 — NFR and Quality Attributes

Defines **how well the system must behave**:

- performance  
- availability  
- scalability  
- reliability  
- SLA / SLO  
- metrics and thresholds  

---

## Role of Bundle 4

Bundle 4 provides a full behavioral model:

```text
Requirements → Rules → Access → Quality
```

More precisely:

```text
What → How → Who → How well
```

It is used as:

- product behavior contract  
- source of truth for correctness  
- input for system design (b2)  
- input for implementation (b3)  
- base for testing and validation  

---

## Relationship with Other Bundles

### Depends on Bundle 0 (Product Core)

- aligns with value streams  
- supports product definition  
- constrained by economics  

---

### Depends on Bundle 1 (Business Dynamics)

- derived from strategy  
- validates hypotheses  
- implemented via processes  

---

### Drives Bundle 2 and 3

```text
Requirement → Process → Component → Code → Runtime
```

---

## Key Principles

### 1. Behavior is Explicit

All system behavior must be defined as:

- requirements  
- rules  
- scenarios  

---

### 2. Rules Before Implementation

```text
Code must implement rules, not define them
```

---

### 3. Access is Separate from Logic

```text
Allowed behavior ≠ possible behavior
```

---

### 4. Quality is Measurable

All quality must be:

- defined  
- measurable  
- testable  

---

### 5. Traceability

Every behavior must link:

```text
Requirement → Rule → Process → Component → Code
```

---

### 6. Domain-Centric Design

Domain model is:

- source of truth for behavior  
- independent from implementation  

---

## Typical Use Cases

- defining product behavior  
- validating requirements  
- aligning business and engineering  
- designing rules and policies  
- preparing test scenarios  
- enabling AI-driven development  

---

## Schema

Machine-readable schema:

```text
b4.schema.json
```

Defines:

- requirements model (401)  
- domain model (402)  
- access model (403)  
- quality model (404)  

---

## Summary

Bundle 4 defines **what the system must do, how it behaves, who can act, and with what quality**.

It is the **behavioral foundation of the entire product model**.
