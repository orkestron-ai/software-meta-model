# Product Core — Definition, Value & Economics (Bundle 0)

## Overview

Bundle 0 represents the **core semantic and control foundation of the product** within AISMM.

It defines:

- what the product is
- where its boundaries are
- how value is created and transformed
- who perceives value and why
- how the business is structured
- how value is executed under normal and failure conditions
- how value translates into economics and constraints

This bundle is the **entry point for understanding, analyzing, and controlling the product**.

---

## Included Layers

- **001 — Product Definition & Context**  
  Defines product boundaries, scope, ownership, and context

- **002 — Aeilus Value Streams**  
  Defines value chains, segments, flows, converters, and key results

- **003 — Stakeholders and Motivation**  
  Defines stakeholders, goals, expectations, success criteria, and change drivers

- **004 — Business Architecture**  
  Defines domains, capabilities, functions, services, and ownership

- **005 — Critical Path**  
  Defines minimal execution path, including failure handling, degraded and recovery routes

- **006 — Product Economics Model**  
  Defines revenue, costs, drivers, constraints, guardrails, and economic impact

---

## Purpose

This bundle provides:

### 1. Product Definition
- product identity
- boundaries and scope
- ownership and context

### 2. Value System
- value chains and segments
- value / anti-value flows
- converters and participants
- key results

### 3. Motivation & Change
- stakeholders and roles
- expectations and success criteria
- change drivers

### 4. Business Structure
- domains and capabilities
- business functions
- business services
- ownership rules

### 5. Execution Model
- minimal value delivery path
- dependencies and bottlenecks
- failure handling
- degraded and recovery routes
- continuity guarantees

### 6. Economic Model
- revenue and cost structures
- drivers
- unit economics
- constraints and guardrails
- impact assessment

---

## Bundle Role in AISMM

Bundle 0 acts as:

Definition → Value → Structure → Execution → Economics → Control

It is the **foundation for all other bundles**, including:

- business dynamics (b1)
- system design (b2)
- implementation (b3)
- behavior (b4)

---

## Schema Definition

The structure of this bundle is defined in:

b0.schema.json

This schema:

- aggregates all layers of Bundle 0
- defines all entities and relationships
- enables machine-readable product model
- supports validation and automation

---

## Integration into AISMM

Bundle 0 is included in the global model:

{
  "bundles": {
    "b0": {
      "$ref": "b0.schema.json"
    }
  }
}

---

## Key Principles

### 1. Stable Identifiers

All entities must have stable IDs:

value.execution_success  
capability.execute_workflow  
business_service.process_request  

---

### 2. Semantic First

This bundle defines **meaning**, not implementation.

It must NOT contain:

- UI
- API
- code
- database schemas

---

### 3. Cross-Layer Connectivity

All entities should be linkable:

Value → Capability → Service → Step → Event → Revenue

---

### 4. Explicit Boundaries

Product boundaries must be clearly defined:

- inside product scope
- external systems
- shared platforms
- manual activities

---

### 5. Control Through Economics

Economics layer defines:

constraints + guardrails → decision boundaries

---

## Notes

- Bundle 0 is **relatively stable**
- Changes indicate **strategic product evolution**
- All other bundles depend on it
- It should be versioned carefully

---

## Summary

Bundle 0 defines the product as a **value-driven, structured, executable, and economically constrained system**.
