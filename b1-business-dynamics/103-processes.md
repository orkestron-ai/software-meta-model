<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 103
layer_key: processes
document_id: spec.processes
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.1.0
title: Processes Layer Specification
<!-- AISMM:META_END -->

# 103 — Processes

## 1. Purpose of the Layer

The **Processes** layer defines how the product operates through **real executable workflows**.

It describes:

- end-to-end business processes
- hierarchical decomposition of processes
- detailed execution flows
- roles, systems, and interactions
- branching logic and orchestration
- real operational complexity

It answers:

- How does the system actually work in reality?
- What are the full execution flows?
- How are decisions made during execution?
- How do processes interact and compose?
- How does execution scale and evolve?

---

## 2. Layer Role in AISMM

This layer connects:

Critical Path → Full Operational Execution

IMPORTANT:

Critical Path (005) ≠ Processes (103)

- Critical Path = minimal required execution
- Processes = full real-world execution

Processes include:

- optional steps
- parallel paths
- exceptions
- operational overhead
- orchestration logic

---

## 3. Main Output

- process network
- hierarchical processes
- execution flows
- orchestration logic
- role/system interaction model

---

## 4. Core Concepts

### 4.1 Process

A structured flow of execution producing an outcome.

---

### 4.2 Process Hierarchy

Processes are hierarchical:

- L0 — end-to-end flows
- L1 — domain processes
- L2+ — detailed execution

---

### 4.3 Process Step

Execution unit inside process.

---

### 4.4 Event

Triggers or affects execution.

---

### 4.5 Gateway

Decision or branching logic.

---

### 4.6 Role / Actor

Executor of steps.

---

### 4.7 Process Orchestration (NEW)

Defines coordination between:

- steps
- subprocesses
- systems

---

### 4.8 Exception Flow (NEW)

Defines behavior outside normal execution:

- errors
- retries
- escalations

---

### 4.9 Parallel Execution (NEW)

Defines concurrent flows.

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Process | process.* |
| Step | step.* |
| Event | event.* |
| Gateway | gateway.* |
| Role | role.* |

---

## 6. Required Structure

### 6.1 Processes

- id
- level
- parent
- related processes

---

### 6.2 Steps

- sequence
- executor
- input/output

---

### 6.3 Events

- triggers
- start/end

---

### 6.4 Gateways

- decision logic

---

### 6.5 Orchestration

- process coordination
- dependencies

---

### 6.6 Exception Flows

- failure paths
- recovery behavior

---

### 6.7 Parallel Flows

- concurrent execution

---

## 7. Preferred Representation

PRIMARY:

- BPMN 2.0 (.bpmn)

ALTERNATIVE:

- Markdown for overview
- JSON/YAML for machine representation

RULES:

- processes may be distributed across files
- hierarchy MUST be explicit
- IDs MUST be consistent
- relationships MUST NOT rely on file structure

---

## 8. Relationships

process → decomposes_into → process  
process → contains → step  
process → calls → process  
step → triggered_by → event  
step → executed_by → role  
gateway → controls → flow  

---

## 9. Cross-Layer Links

Strategy:
process → implements → initiative.*

Value:
process → transforms → value.*

Business Architecture:
process → uses → business_service.*

Critical Path:
process → extends → step.*

Behavior:
step → invokes → control.*

Economics:
process → generates → cost.*

---

## 10. Boundaries

Does NOT include:

- minimal path (005)
- implementation
- UI
- API

---

## 11. Minimal Valid Content

- one process
- one step

---

## 12. Completeness Criteria

- full hierarchy
- orchestration
- exception flows
- parallel execution
- cross-layer links

---

## 13. Summary

Defines **real execution complexity of the product**, beyond minimal path.

<!-- AISMM:END -->
