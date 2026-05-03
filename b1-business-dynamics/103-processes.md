<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 103
layer_key: processes
document_id: spec.processes
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Processes Layer Specification
<!-- AISMM:META_END -->

# 103 — Processes

## 1. Purpose of the Layer

The **Processes** layer defines how the business operates through executable flows of work.

It describes:

- business processes at different levels of abstraction
- process steps, events, and decision points
- roles and responsibilities within processes
- inputs, outputs, and artifacts
- hierarchy and decomposition of processes
- relationships between processes

It answers:

- How does the product operate in reality?
- What flows of work exist?
- Who does what and when?
- How do processes connect to each other?
- How do high-level processes decompose into detailed ones?

---

## 2. Layer Role in AISMM

This layer connects:

```text
Strategy → Execution Reality
```

It operationalizes initiatives and capabilities into **structured workflows**.

Other layers rely on it to:

- understand real execution flows
- connect requirements to operations
- define responsibilities and interactions
- map system behavior to business operations

This layer does NOT define:

- minimal value path (see Critical Path)
- low-level code logic
- UI structure
- detailed technical architecture

It defines **real operational processes**.

---

## 3. Main Output of the Layer

The output is a **network of interconnected processes**, including:

- high-level processes
- sub-processes
- detailed execution flows
- process hierarchy
- process relationships
- mapping to roles, systems, and capabilities

---

## 4. Core Concepts

### 4.1 Process

A process is a structured flow of work that produces an outcome.

Examples:

```text
process.user_onboarding
process.order_processing
process.workflow_execution
```

---

### 4.2 Sub-Process

A sub-process is a decomposition of a higher-level process.

Processes may form a hierarchy:

```text
process.onboarding
  → process.onboarding.registration
  → process.onboarding.activation
```

---

### 4.3 Process Step

A step is a unit of work within a process.

---

### 4.4 Event

An event triggers, starts, ends, or affects a process.

Examples:

- start event
- end event
- intermediate event
- external trigger

---

### 4.5 Gateway / Decision

Represents branching logic within a process.

---

### 4.6 Role / Actor

Defines who performs a step:

- user
- system
- external service
- internal team

---

### 4.7 Input / Output

Defines what enters and exits a process or step.

---

### 4.8 Process Hierarchy

Processes can be:

- top-level (end-to-end flows)
- mid-level (domain processes)
- detailed (step-level execution)

---

### 4.9 Process Relationship

Defines relationships between processes:

- parent → child
- process → calls → process
- process → depends_on → process

---

## 5. Identifiable Entities

| Entity Type  | Identifier Prefix  |
| :----------- | :----------------- |
| Process      | `process.*`        |
| Sub-Process  | `process.*`        |
| Step         | `step.*`           |
| Event        | `event.*`          |
| Gateway      | `gateway.*`        |
| Role         | `role.*`           |
| Input        | `input.*`          |
| Output       | `output.*`         |
| Process Link | `process_link.*`   |

---

## 6. Required Content Structure

---

### 6.1 Process Definitions

Each process should include:

- identifier
- name
- description
- level (high-level / mid-level / detailed)
- parent process (if any)
- related processes

---

### 6.2 Process Hierarchy

Define relationships:

- parent → child
- decomposition
- aggregation

---

### 6.3 Steps

Each process defines:

- steps
- sequence
- responsibilities

---

### 6.4 Events

Define:

- start conditions
- triggers
- end conditions

---

### 6.5 Decisions / Gateways

Define:

- branching conditions
- decision logic

---

### 6.6 Roles

Define who executes steps.

---

### 6.7 Inputs / Outputs

Define:

- what flows through the process
- artifacts produced

---

### 6.8 Process Links

Define how processes interact:

- call
- trigger
- dependency

---

## 7. Preferred Representation

The semantic content of this layer is independent of any specific representation format.

This layer defines **structured flows of work**, not how they must be stored.

Due to the process-oriented nature of this layer, the following representations are considered most suitable:

- BPMN (`.bpmn`) for detailed and formal process modeling  
- Markdown (`.md`) for high-level descriptions and documentation  
- Structured formats (JSON / YAML) for machine-readable process graphs  

Important considerations:

- There may be **many process files**
- Processes may be **distributed across modules**
- Processes may have **hierarchical decomposition**
- Processes must be **linked via identifiers**

Implementations:

- SHOULD use BPMN for detailed process modeling  
- SHOULD maintain hierarchical relationships between processes  
- SHOULD ensure consistent identifiers across files  
- MAY use Markdown for high-level overviews  
- MUST NOT depend solely on file structure to define relationships  

The correctness of this layer is determined by the consistency, completeness, and connectivity of the process network.

---

## 8. Relationships Inside the Layer

```text
process → contains → step
process → decomposes_into → process
process → calls → process
step → triggered_by → event
step → executed_by → role
step → produces → output
step → consumes → input
gateway → controls → step flow
```

---

## 9. Relationships With Other AISMM Layers

### Strategy and Product Management

```text
process → implements → initiative.*
```

---

### Business Hypotheses

```text
process → validates → hypothesis.*
```

---

### Critical Path

```text
process → includes → step.*
```

---

### Business Architecture

```text
process → uses → capability.*
process → uses → entity.*
```

---

### Product Behavior

```text
step → invokes → control.*
event → maps_to → event.*
```

---

### Value Streams

```text
process → transforms → value.*
```

---

### Stakeholders

```text
role → represents → stakeholder.*
```

---

### Economics

```text
process → generates → cost.*
process → contributes_to → revenue.*
```

---

## 10. Layer Boundaries

This layer must not include:

- UI structure
- API contracts
- database schemas
- implementation details
- product strategy definition

---

## 11. Recommended Block Types

- layer_document
- process_definition
- step_definition
- event_definition
- role_definition
- process_link_definition

---

## 12. Minimal Valid Content

Must define:

- at least one process
- at least one step
- at least one role or event

---

## 13. Completeness Criteria

A mature model includes:

- full process hierarchy
- linked processes
- clear roles
- defined inputs and outputs
- mapping to capabilities and strategy

---

## 14. Example Identifiers

```text
process.user_onboarding
process.workflow_execution
step.validate_input
event.user_signup
role.system
process_link.onboarding_to_activation
```

---

## 15. Summary

This layer defines **how the product operates through connected processes**.

It provides a structured and scalable representation of execution flows, enabling:

- operational clarity
- system mapping
- process optimization
- alignment with strategy and value

<!-- AISMM:END -->
