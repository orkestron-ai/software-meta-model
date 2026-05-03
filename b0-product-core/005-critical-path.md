<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 005
layer_key: critical_path
document_id: spec.critical.path
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.1.0
title: Critical Path Layer Specification
<!-- AISMM:META_END -->

# 005 — Critical Path

## 1. Purpose of the Layer

The **Critical Path** layer defines the minimal sequence of steps required to deliver the core value of the product **including how this path behaves under failure conditions**.

It describes:

- the shortest path from input to value outcome
- key steps required to produce value
- dependencies between steps
- bottlenecks and constraints
- failure handling behavior
- degraded and recovery routes
- continuity guarantees

It answers:

- What is the core flow that produces value?
- What steps are essential?
- What is the minimal working path?
- Where are the bottlenecks?
- What cannot be skipped?
- What happens when something fails?
- How does the system continue to deliver value under degradation?

---

## 2. Layer Role in AISMM

This layer connects:

```text
Value → Execution (minimal and resilient form)
```

It is the **bridge between static structure and dynamic processes**, enriched with resilience semantics.

Other layers rely on it to:

- identify key execution paths
- prioritize optimization
- understand system dependencies
- define minimal viable execution
- reason about failure and recovery
- derive processes and workflows

This layer does NOT define:

- full processes
- detailed branching logic
- UI flows
- implementation details

It defines the **essential and resilient path only**.

---

## 3. Main Output of the Layer

The output is a structured description of:

- critical steps
- their order
- dependencies
- inputs and outputs
- value produced at each step
- failure handling mechanisms
- degraded and recovery routes
- continuity contracts

---

## 4. Core Concepts

### 4.1 Critical Step

A critical step is an essential unit of execution.

It:

- cannot be removed without breaking value delivery
- contributes directly to value creation

---

### 4.2 Sequence

Defines the order of execution.

---

### 4.3 Dependency

Defines that one step depends on another.

---

### 4.4 Input / Output

Each step:

- consumes input
- produces output

---

### 4.5 Bottleneck

A step that limits performance or value delivery.

---

### 4.6 Value Transition

Each step should map to a value transformation.

---

### 4.7 Failure Handling

Defines what happens when a step fails.

Includes:

- retry
- fallback
- escalation
- compensation

---

### 4.8 Degraded Route

A degraded route is an alternative path where value is delivered with reduced quality.

---

### 4.9 Recovery Route

A recovery route restores normal execution after failure or degradation.

---

### 4.10 Continuity Contract

Defines guarantees of value delivery under constraints.

Examples:

- partial completion allowed
- delayed completion allowed
- eventual consistency
- best-effort execution

---

## 5. Identifiable Entities

| Entity Type | Identifier Prefix |
|-------------|------------------|
| Step | `step.*` |
| Dependency | `dependency.*` |
| Input | `input.*` |
| Output | `output.*` |
| Bottleneck | `bottleneck.*` |
| Failure Handling | `failure.*` |
| Degraded Route | `degraded_route.*` |
| Recovery Route | `recovery_route.*` |
| Continuity Contract | `continuity.*` |

---

## 6. Required Content Structure

### 6.1 Critical Steps

- identifier
- description
- role in value creation

---

### 6.2 Sequence Definition

- ordered list of steps

---

### 6.3 Dependencies

- step dependencies

---

### 6.4 Inputs and Outputs

- consumed inputs
- produced outputs

---

### 6.5 Value Mapping

- value transformation per step

---

### 6.6 Bottlenecks

- known constraints

---

### 6.7 Failure Handling

- what happens on failure
- retry / fallback / escalation

---

### 6.8 Degraded Routes

- alternative execution paths
- reduced value scenarios

---

### 6.9 Recovery Routes

- how system returns to normal

---

### 6.10 Continuity Contracts

- guarantees of value delivery

---

## 7. Preferred Representation

Recommended:

- Markdown for clarity
- simple flow diagrams
- sequence diagrams
- structured JSON/YAML

This is NOT BPMN.

---

## 8. Relationships Inside the Layer

```text
step → depends_on → step
step → consumes → input
step → produces → output
step → creates → value
step → fails → failure
failure → triggers → degraded_route
degraded_route → leads_to → recovery_route
step → governed_by → continuity
```

---

## 9. Relationships With Other AISMM Layers

### Value Streams

```text
step → realizes → value_segment
```

---

### Business Architecture

```text
step → uses → business_service.*
step → performs → business_function.*
```

---

### Processes

```text
process_step → refines → step
```

---

### System Design

```text
component → executes → step
```

---

### Product Behavior

```text
control → implements → step
event → triggers → step
```

---

### Product Economics

```text
step → generates → cost
step → protects → revenue
failure → increases → cost
```

---

## 10. Layer Boundaries

Must not include:

- full processes
- UI flows
- API definitions
- implementation details

---

## 11. Recommended Block Types

- layer_document
- step_definition
- failure_definition
- route_definition

---

## 12. Minimal Valid Content

Must define:

- at least one step
- at least one dependency

---

## 13. Completeness Criteria

A mature model includes:

- full critical path
- dependencies
- value mapping
- bottlenecks
- failure handling
- degraded and recovery routes
- continuity contracts

---

## 14. Example Identifiers

```text
step.create_request
step.process_request
step.return_result
failure.timeout
degraded_route.manual_fallback
recovery_route.retry_flow
continuity.eventual_completion
```

---

## 15. Summary

This layer defines the **minimal and resilient path of value delivery**.

It ensures that the product:

- delivers value in normal conditions
- continues operating under failure
- provides guarantees of continuity

<!-- AISMM:END -->
