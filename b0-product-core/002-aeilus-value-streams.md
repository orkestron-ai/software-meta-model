<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 002
layer_key: aeilus_value_streams
document_id: spec.aeilus.value.streams
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Aeilus Value Streams Layer Specification
<!-- AISMM:META_END -->

# 002 — Aeilus Value Streams

## 1. Purpose of the Layer

The **Aeilus Value Streams** layer defines how value is created, transformed, transferred, accumulated, and lost within and around the product.

This layer describes the system **from a value perspective**, independent of implementation, architecture, or processes.

It answers:

- Where does value originate?
- How does value flow through the system?
- Where is value increased, decreased, or destroyed?
- Which elements transform value?
- Where does anti-value appear?
- How do different participants perceive value?

---

## 2. Layer Role in AISMM

This layer provides the **semantic backbone of the product in terms of value**.

Other layers rely on it to:

- understand why the system exists
- align decisions with value creation
- connect actions and outcomes to value
- evaluate effectiveness and efficiency
- map economic and operational impact

This layer does NOT define:

- technical implementation
- UI behavior
- API structure
- internal system architecture

It defines **value logic only**.

---

## 3. Main Output of the Layer

The output of this layer is a **value system model**, consisting of:

- value transformations
- value flows
- participants affecting or perceiving value
- value and anti-value elements
- measurable or observable value effects

---

## 4. Core Concepts

### 4.1 Value

Value is any positive outcome perceived by a participant.

Value may be:

- functional (task completion)
- economic (revenue, savings)
- experiential (UX quality)
- informational (insight, knowledge)
- strategic (market position)

---

### 4.2 Anti-Value

Anti-value is any negative outcome:

- delays
- errors
- friction
- cost without benefit
- risk realization

Anti-value is consumed by the system and may also be produced.

---

### 4.3 Value Element

A value element is a measurable or identifiable unit of value or anti-value. There are many Value Element types.

Examples:

```text
value.execution_completed
value.customer_satisfaction
antivalue.delay
antivalue.failed_execution
```

---

### 4.4 Value Flow

A value flow represents movement of value or anti-value between participants.

It describes:

- direction
- transformation
- accumulation
- distribution

---

### 4.5 Converter

A converter is an abstract entity that transforms value. Converter is a role for participants.

A converter:

- consumes value and/or anti-value
- produces value and/or anti-value
- may accumulate value internally

Converters may represent:

- product modules
- processes
- actors
- systems
- functions

---

### 4.6 Value Participant

A value participant is any entity that:

- contributes to value creation
- consumes value
- evaluates value

Participants may be:

- internal
- external
- human
- system-level
- conceptual

---

### 4.7 Value Context

A value context defines the scope in which value is evaluated.

Different participants may perceive value differently within different contexts.

---

## 5. Identifiable Entities

| Entity Type | Identifier Prefix |
| ----------- | ----------------- |
| Value       | `value.*`         |
| Anti-Value  | `antivalue.*`     |
| Converter   | `converter.*`     |
| Flow        | `flow.*`          |
| Participant | `participant.*`   |
| Context     | `context.*`       |
| Metric      | `metric.*`        |

---

## 6. Required Content Structure

---

### 6.1 Value Elements

Define:

- types of value
- types of anti-value
- their meaning
- their relevance

---

### 6.2 Converters

Define entities that transform value.

For each converter:

- identifier
- description
- role in value transformation

---

### 6.3 Value Flows

Define how value moves:

- source
- destination
- type of value
- direction

---

### 6.4 Participants

Define entities that:

- produce value
- consume value
- evaluate value

---

### 6.5 Value Transformations

Describe:

- how value changes
- where it increases
- where it decreases
- where it is lost

---

### 6.6 Anti-Value Sources

Define:

- where anti-value originates
- how it propagates

---

### 6.7 Metrics

Define measurable aspects:

- value metrics
- anti-value metrics
- efficiency indicators

---

## 7. Preferred Representation

The semantic content of this layer is independent of any specific representation format.

This layer defines **how value exists, flows, and transforms**, not how it must be visualized or stored.

However, due to the inherently **graph-based nature of value systems**, the following representations are considered most suitable:

- Graph-based representations (directed graphs) for modeling value flows and transformations  
- Structured formats (JSON / YAML) for machine-readable value system definitions  
- Visual diagrams for human understanding of value flows and transformations  
- Markdown (`.md`) for descriptive explanations and contextual clarification  

Typical graph concepts that representations should support:

- nodes (value elements, converters, participants)  
- edges (value flows)  
- directionality  
- multiple value types (value / anti-value)  
- transformations and accumulation  

---

### Reference Representation (VSS)

A commonly used and highly suitable representation for this layer is the **Value Stream Schema (VSS)**.

The schema is defined in:

vss.schema.json

VSS provides:

- a structured graph model of value systems  
- explicit representation of converters, flows, and value elements  
- compatibility with visual editors and tools  
- the ability to view and manipulate the entire value system as a single coherent model  

VSS is considered:

- a **recommended representation** for this layer  
- a **reference implementation of the value model**  
- a **convenient format for both humans and tools**  

However:

- VSS is **not mandatory**  
- the semantic model of this layer MUST NOT depend on VSS-specific constructs  
- alternative representations MAY be used if they preserve the same semantics  

---

## Representation Guidelines

Implementations:

- SHOULD use graph-capable representations for clarity and completeness  
- SHOULD prefer structured formats when machine processing is required  
- MAY use VSS (`vss.schema.json`) as a primary working format  
- MAY use alternative formats if they preserve semantic meaning  
- MUST NOT encode semantics in representation-specific details  

The correctness of this layer is determined by the consistency and completeness of the **value system model**, not by the chosen format.

---

## 8. Relationships Inside the Layer

```text
converter → consumes → value
converter → produces → value
converter → produces → anti-value
flow → transfers → value
participant → evaluates → value
participant → affected_by → flow
value → measured_by → metric
```

---

## 9. Relationships With Other AISMM Layers

### Product Definition & Context

```text
converter → module.*
participant → external.*
```

---

### Stakeholders

```text
participant → stakeholder.*
value → stakeholder perception
```

---

### Business Architecture

```text
converter → business capability
```

---

### Critical Path

```text
critical_step → flow
```

---

### Product Behavior

```text
control → value transformation
event → value change
```

---

### P&L Model

```text
value → revenue
antivalue → cost
```

---

### Observability

```text
metric → monitoring
```

---

## 10. Layer Boundaries

This layer must not include:

- UI details
- API definitions
- database structures
- code logic
- detailed process steps

---

## 11. Recommended Block Types

- layer_document
- value_definition
- flow_definition
- converter_definition
- metric_definition

---

## 12. Minimal Valid Content

Must define:

- at least one value element
- at least two converters
- at least one flow

---

## 13. Completeness Criteria

A mature model includes:

- full value graph
- anti-value mapping
- participants
- metrics
- cross-layer links

---

## 14. Example Identifiers

```text
value.execution_success
antivalue.execution_failure
converter.workflow_engine
flow.execution_flow
participant.customer
metric.success_rate
```

---

## 15. Summary

This layer defines **how value exists and moves in the system**.

It is independent of implementation and provides the foundation for:

- product decisions
- optimization
- economic modeling
- system evolution

<!-- AISMM:END -->
