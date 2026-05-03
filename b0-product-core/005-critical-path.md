<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 005
layer_key: critical_path
document_id: spec.critical.path
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Critical Path Layer Specification
<!-- AISMM:META_END -->

# 005 — Critical Path

## 1. Purpose of the Layer

The **Critical Path** layer defines the minimal sequence of steps required to deliver core value of the product.

It describes:

- the shortest path from input to value outcome
- key steps required to produce value
- dependencies between steps
- bottlenecks and constraints

It answers:

- What is the core flow that produces value?
- What steps are essential?
- What is the minimal working path?
- Where are the bottlenecks?
- What cannot be skipped?

---

## 2. Layer Role in AISMM

This layer connects:

```text
Value → Execution (minimal form)
```

It is the **bridge between static structure and dynamic processes**.

Other layers rely on it to:

- identify key execution paths
- prioritize optimization
- understand system dependencies
- derive processes and workflows

This layer does NOT define:

- full processes
- alternative paths
- UI flows
- detailed system behavior

It defines the **essential path only**.

---

## 3. Main Output of the Layer

The output is a structured description of:

- critical steps
- their order
- dependencies
- inputs and outputs
- value produced at each step

---

## 4. Core Concepts

### 4.1 Critical Step

A critical step is an essential unit of execution.

It:

- cannot be removed without breaking value delivery
- contributes directly to value creation

Example:

```text
step.create_task
step.execute_workflow
step.generate_result
```

---

### 4.2 Sequence

Defines the order of execution.

Critical path is inherently sequential (even if system is parallel internally).

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

## 5. Identifiable Entities

| Entity Type | Identifier Prefix |
| :---------- | :---------------- |
| Step        | `step.*`          |
| Dependency  | `dependency.*`    |
| Input       | `input.*`         |
| Output      | `output.*`        |
| Bottleneck  | `bottleneck.*`    |

---

## 6. Required Content Structure

---

### 6.1 Critical Steps

Define all essential steps.

For each step:

- identifier
- description
- role in value creation

---

### 6.2 Sequence Definition

Define order of steps.

---

### 6.3 Dependencies

Define dependencies between steps.

---

### 6.4 Inputs and Outputs

Define:

- what each step consumes
- what each step produces

---

### 6.5 Value Mapping

Each step should map to value transformation.

---

### 6.6 Bottlenecks

Define known bottlenecks or constraints.

---

## 7. Preferred Representation

The semantic content of this layer is independent of any specific representation format.

This layer defines **the minimal execution path for value delivery**, not how it must be visualized or stored.

Due to its semi-structural and sequential nature, the following representations are considered most suitable:

- Markdown (`.md`) for describing steps, sequences, and dependencies  
- Structured formats (JSON / YAML) for machine-readable path definitions  
- Simple diagrams (flow diagrams, sequence diagrams) for visualization  

This layer is intentionally simpler than full process modeling and SHOULD NOT be represented as full BPMN.

Implementations:

- SHOULD use simple and clear representations of sequence and dependencies  
- SHOULD avoid overcomplicating the model  
- MAY use diagrams to improve understanding  
- MUST NOT encode semantics in format-specific constructs  

The correctness of this layer is determined by the completeness and clarity of the **critical path definition**, not by the chosen representation.

---

## 8. Relationships Inside the Layer

```text
step → depends_on → step
step → consumes → input
step → produces → output
step → creates → value
```

---

## 9. Relationships With Other AISMM Layers

### Value Streams

```text
step → maps_to → value transformation
```

---

### Stakeholders

```text
step → impacts → stakeholder.*
```

---

### Business Architecture

```text
step → uses → capability.*
```

---

### Product Behavior

```text
control → implements → step
event → represents → step execution
```

---

### System Design

```text
component → executes → step
```

---

### P&L Model

```text
step → generates → cost
step → contributes_to → revenue
```

---

## 10. Layer Boundaries

This layer must not include:

- full business processes
- UI flows
- API definitions
- implementation details
- branching logic (except minimal)

---

## 11. Recommended Block Types

- layer_document
- step_definition
- dependency_definition

---

## 12. Minimal Valid Content

Must define:

- at least one step
- at least one dependency

---

## 13. Completeness Criteria

A mature model includes:

- full minimal value path
- step dependencies
- value mapping
- bottlenecks

---

## 14. Example Identifiers

```text
step.create_request
step.process_request
step.return_result
dependency.request_to_processing
bottleneck.processing_delay
```

---

## 15. Summary

This layer defines the **minimal path of value delivery**.

It provides a simplified execution model that connects value to system behavior and prepares the foundation for full process modeling.

<!-- AISMM:END -->
