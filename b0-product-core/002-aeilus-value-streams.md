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

The **Aeilus Value Streams** layer defines how value is created, transformed, transferred, accumulated, degraded, lost, or destroyed within and around the product.

This layer describes the product from a **value perspective**, independent of implementation, architecture, UI, APIs, or internal processes.

It answers:

- Where does value originate?
- Through which value chains and value stream segments does value move?
- What are the entry and exit points of value flows?
- Where is value increased, decreased, accumulated, or lost?
- Which converters transform value or anti-value?
- Where does anti-value appear?
- Which participants produce, consume, or evaluate value?
- What key results indicate that value has been delivered?
- How do different participants perceive value in different contexts?

---

## 2. Layer Role in AISMM

This layer provides the **semantic backbone of the product in terms of value**.

Other AISMM layers rely on it to:

- understand why the product exists
- align decisions with value creation
- connect actions and outcomes to value
- evaluate effectiveness and efficiency
- connect product behavior with economic impact
- identify where anti-value appears and how it propagates
- prioritize changes based on value impact

This layer does NOT define:

- detailed business processes
- technical implementation
- UI behavior
- API structure
- database structure
- internal system architecture
- financial formulas

It defines **value logic only**.

---

## 3. Main Output of the Layer

The output of this layer is a **value system model**, consisting of:

- value chains
- value stream segments
- value flows
- value entry points and exit points
- converters
- participants
- value and anti-value elements
- key results
- measurable or observable value effects
- mappings to other AISMM layers

---

## 4. Core Concepts

### 4.1 Value

Value is any positive outcome perceived by a participant.

Value may be:

- functional (task completion)
- economic (revenue, savings, cost avoidance)
- experiential (UX quality, satisfaction, convenience)
- informational (insight, knowledge, clarity)
- strategic (market position, optionality, differentiation)
- operational (speed, reliability, manageability)
- risk-reducing (lower uncertainty, fewer failures, better control)

Value must be described in terms of **who perceives it** and **why it matters**.

---

### 4.2 Anti-Value

Anti-value is any negative outcome, friction, loss, waste, or harmful effect.

Examples:

- delays
- errors
- rework
- manual effort
- friction
- unnecessary cost
- risk exposure
- quality degradation
- lost opportunity
- uncertainty
- operational overload

Anti-value may be:

- consumed by converters
- produced by converters
- accumulated inside a system or process
- transferred to another participant
- transformed into future risks, costs, or technical debt

---

### 4.3 Value Element

A value element is a measurable or identifiable unit of value or anti-value.

Examples:

```text
value.execution_completed
value.customer_satisfaction
value.time_saved
antivalue.delay
antivalue.failed_execution
antivalue.manual_rework
```

Value elements should be stable enough to be referenced by strategy, processes, product economics, metrics, and requirements.

---

### 4.4 Value Chain

A value chain is a high-level sequence of value creation across the product or organization.

It describes how value moves from initial demand, need, or trigger to final outcome.

Examples:

```text
value_chain.customer_onboarding
value_chain.agent_task_execution
value_chain.order_fulfillment
```

A value chain may contain multiple value stream segments.

---

### 4.5 Value Stream Segment

A value stream segment is a meaningful part of a value chain.

It may represent:

- one stage of value creation
- one handoff between participants
- one transformation area
- one major business function
- one product module contribution
- one process area

Examples:

```text
value_segment.registration
value_segment.identity_verification
value_segment.workflow_execution
```

Segments help connect value streams with business architecture, critical path, processes, modules, and economics.

---

### 4.6 Entry Point

An entry point defines where value flow starts.

Examples:

```text
entry_point.user_need
entry_point.customer_request
entry_point.business_signal
entry_point.external_event
```

Entry points may be connected to stakeholders, channels, triggers, hypotheses, or processes.

---

### 4.7 Exit Point

An exit point defines where value flow produces a meaningful outcome.

Examples:

```text
exit_point.account_created
exit_point.task_completed
exit_point.report_delivered
exit_point.payment_confirmed
```

Exit points should be connected to key results, stakeholder outcomes, metrics, and economics.

---

### 4.8 Key Result

A key result is an observable or measurable outcome that indicates value delivery.

Examples:

```text
key_result.registration_completed
key_result.time_to_value_reduced
key_result.manual_work_reduced
```

Key results may be mapped to product goals, KPIs, metrics, economic drivers, or critical path outcomes.

---

### 4.9 Value Flow

A value flow represents movement of value or anti-value between converters, participants, or contextual entities.

It describes:

- source
- target
- direction
- value elements transferred
- anti-value elements transferred
- transformation logic
- accumulation or loss
- entry or exit point when applicable

---

### 4.10 Converter

A converter is an abstract entity that transforms value or anti-value.

A converter:

- consumes value and/or anti-value
- produces value and/or anti-value
- may accumulate value or anti-value internally
- may represent a product module, process, actor, system, function, business service, or external participant

Examples:

```text
converter.workflow_engine
converter.support_operator
converter.payment_gateway
```

Converter is a semantic role, not necessarily a technical component.

---

### 4.11 Value Participant

A value participant is any entity that:

- contributes to value creation
- consumes value
- evaluates value
- receives anti-value
- influences value interpretation

Participants may be:

- external users
- customers
- internal teams
- systems
- partners
- regulators
- conceptual actors

---

### 4.12 Value Context

A value context defines the scope in which value is evaluated.

Different participants may perceive the same value flow differently.

Examples:

```text
context.customer_onboarding
context.internal_operations
context.partner_integration
```

---

### 4.13 Value Metric

A value metric measures value, anti-value, throughput, efficiency, quality, or perceived outcome.

Examples:

```text
metric.time_to_value
metric.success_rate
metric.manual_effort
metric.failure_rate
```

Metrics should be linked to observability, KPIs, economics, hypotheses, or process metrics when applicable.

---

## 5. Identifiable Entities

This layer should define stable identifiers for the following entity types.

| Entity Type | Identifier Prefix | Description |
|------------|------------------|-------------|
| Value | `value.*` | Positive value element |
| Anti-Value | `antivalue.*` | Negative value, friction, loss, or waste |
| Value Chain | `value_chain.*` | High-level chain of value creation |
| Value Stream Segment | `value_segment.*` | Segment of a value chain |
| Entry Point | `entry_point.*` | Point where value flow starts |
| Exit Point | `exit_point.*` | Point where value flow produces an outcome |
| Key Result | `key_result.*` | Observable result of value delivery |
| Converter | `converter.*` | Entity that transforms value or anti-value |
| Flow | `flow.*` | Directed transfer of value or anti-value |
| Participant | `participant.*` | Entity producing, consuming, or evaluating value |
| Context | `context.*` | Scope of value interpretation |
| Metric | `metric.*` | Measurement of value or anti-value |

Identifiers must be stable and reused by other AISMM layers.

---

## 6. Required Content Structure

A value streams layer should contain the following sections.

---

### 6.1 Value Chains

Define high-level chains of value creation.

Each value chain should include:

- identifier
- name
- description
- participants
- entry point
- exit point
- expected key results
- related product modules or business services

---

### 6.2 Value Stream Segments

Define meaningful segments within value chains.

Each segment should include:

- identifier
- name
- parent value chain
- source and target
- participating converters
- value elements
- anti-value elements
- key result or expected outcome
- related business capability or service if known

---

### 6.3 Entry and Exit Points

Define where value flows begin and where meaningful outcomes appear.

Each entry or exit point should include:

- identifier
- description
- related participant or channel
- related trigger, process, or critical step if known
- related key result if applicable

---

### 6.4 Value Elements

Define:

- types of value
- types of anti-value
- their meaning
- who perceives them
- why they matter
- how they can be measured or observed

---

### 6.5 Converters

Define entities that transform value.

For each converter:

- identifier
- description
- converter type
- consumed value or anti-value
- produced value or anti-value
- role in value transformation
- related module, process, function, business service, system, or actor

---

### 6.6 Value Flows

Define how value moves.

Each flow should include:

- identifier
- source
- destination
- transferred value elements
- transferred anti-value elements
- direction
- transformation meaning
- related value stream segment

---

### 6.7 Participants

Define entities that:

- produce value
- consume value
- evaluate value
- receive anti-value
- influence value interpretation

---

### 6.8 Value Transformations

Describe:

- how value changes
- where it increases
- where it decreases
- where it is accumulated
- where it is lost
- where anti-value is consumed or produced

---

### 6.9 Anti-Value Sources

Define:

- where anti-value originates
- how it propagates
- which participants are affected
- which converters produce or consume it
- which risks, costs, or technical debt may be linked to it

---

### 6.10 Key Results

Define observable or measurable outcomes that confirm value delivery.

Each key result should include:

- identifier
- description
- related value chain or segment
- related stakeholder outcome
- related KPI or metric if available
- related economic effect if available

---

### 6.11 Metrics

Define measurable aspects:

- value metrics
- anti-value metrics
- efficiency indicators
- throughput indicators
- perception metrics
- economic proxies

---

## 7. Preferred Representation

The semantic content of this layer is independent of any specific representation format.

This layer defines **how value exists, flows, transforms, accumulates, and degrades**, not how it must be visualized or stored.

However, due to the inherently **graph-based nature of value systems**, the following representations are considered most suitable:

- Graph-based representations (directed graphs) for modeling value flows and transformations
- Structured formats (JSON / YAML) for machine-readable value system definitions
- Visual diagrams for human understanding of value flows and transformations
- Markdown (`.md`) for descriptive explanations and contextual clarification

Typical graph concepts that representations should support:

- nodes (value elements, converters, participants, entry points, exit points)
- edges (value flows)
- directionality
- multiple value types (value / anti-value)
- value chains and value stream segments
- transformations, accumulation, loss, and degradation

---

### Reference Representation (VSS)

A commonly used and highly suitable representation for this layer is the **Value Stream Schema (VSS)**.

The schema is defined in:

```text
vss.schema.json
```

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

### Representation Guidelines

Implementations:

- SHOULD use graph-capable representations for clarity and completeness
- SHOULD prefer structured formats when machine processing is required
- MAY use VSS (`vss.schema.json`) as a primary working format
- MAY use alternative formats if they preserve semantic meaning
- MUST NOT encode semantics in representation-specific details

The correctness of this layer is determined by the consistency and completeness of the **value system model**, not by the chosen format.

---

## 8. Relationships Inside the Layer

This layer should define relationships such as:

```text
value_chain → contains → value_segment
value_chain → starts_at → entry_point
value_chain → ends_at → exit_point
value_segment → contains → flow
value_segment → produces → key_result
converter → consumes → value
converter → consumes → anti-value
converter → produces → value
converter → produces → anti-value
converter → accumulates → value
converter → accumulates → anti-value
flow → transfers → value
flow → transfers → anti-value
participant → evaluates → value
participant → affected_by → anti-value
participant → affected_by → flow
value → measured_by → metric
anti-value → measured_by → metric
key_result → measured_by → metric
```

These relationships may be written in human-readable text, structured tables, graph format, or extracted into a dedicated traceability graph.

---

## 9. Relationships With Other AISMM Layers

### 9.1 Product Definition & Context

Product modules, channels, external systems, context domains, and manual activities may participate in the value model.

```text
converter.* → module.*
participant.* → external.*
entry_point.* → channel.*
value_segment.* → context.*
manual_activity.* → converter.*
```

---

### 9.2 Stakeholders and Motivation

Stakeholders evaluate value and anti-value differently depending on their expectations and motivations.

```text
participant.* → stakeholder.*
value.* → stakeholder perception
antivalue.* → stakeholder pain
key_result.* → stakeholder success criterion
```

---

### 9.3 Business Architecture

Value stream segments should be grounded in business functions, business services, capabilities, and business domains.

```text
value_segment.* → supported_by → business_service.*
value_segment.* → enabled_by → capability.*
value_segment.* → related_to → business_function.*
converter.* → business_capability.*
flow.* → supported_by → business_service.*
```

---

### 9.4 Critical Path

Critical path steps represent minimal execution required to move value through key segments.

```text
critical_step.* → supports → value_segment.*
critical_step.* → produces → key_result.*
critical_chain.* → realizes → value_chain.*
```

---

### 9.5 Business Hypotheses

Hypotheses may predict changes in value, anti-value, or key results.

```text
hypothesis.* → expects_change_in → value.*
hypothesis.* → expects_reduction_in → antivalue.*
hypothesis.* → validates → key_result.*
```

---

### 9.6 Strategy and Product Management

Strategies, objectives, KPIs, and initiatives should be connected to value chains and key results.

```text
strategy.* → supports → value_chain.*
objective.* → targets → key_result.*
kpi.* → measures → key_result.*
initiative.* → improves → value_segment.*
```

---

### 9.7 Processes

Processes may implement or operationalize value stream segments.

```text
process.* → operationalizes → value_segment.*
process_step.* → transforms → value.*
process_metric.* → measures → key_result.*
```

---

### 9.8 Product Behavior

Controls, requirements, events, and business rules may produce or constrain value transformations.

```text
requirement.* → supports → value.*
control.* → performs → value_transformation
event.* → signals → value_change
business_rule.* → constrains → converter.*
```

---

### 9.9 Product Economics

Value and anti-value may map to revenue, cost, economic drivers, or economic impact assessment.

```text
value.* → contributes_to → revenue.*
antivalue.* → contributes_to → cost.*
key_result.* → affects → revenue_driver.*
antivalue.* → affects → cost_driver.*
```

---

### 9.10 Observability

Metrics and signals may measure value delivery, anti-value, and key results.

```text
metric.* → observed_by → observability_metric.*
key_result.* → measured_by → metric.*
value.* → measured_by → metric.*
antivalue.* → measured_by → metric.*
```

---

### 9.11 Risks and Technical Debt

Anti-value may be linked to risks, constraints, operational problems, or technical debt.

```text
antivalue.* → caused_by → risk.*
antivalue.* → caused_by → debt.*
risk.* → threatens → key_result.*
debt.* → increases → antivalue.*
```

---

## 10. Layer Boundaries

This layer must not include:

- detailed process steps
- UI details
- API definitions
- database structures
- implementation details
- code logic
- financial formulas
- operational runbooks

This layer may reference those elements only to explain value relationships.

---

## 11. Recommended Block Types

The layer may contain the following AISMM block types:

- `layer_document`
- `value_chain_definition`
- `value_segment_definition`
- `value_definition`
- `anti_value_definition`
- `entry_point_definition`
- `exit_point_definition`
- `key_result_definition`
- `flow_definition`
- `converter_definition`
- `participant_definition`
- `metric_definition`
- `schema_reference`

---

## 12. Minimal Valid Content

A minimal valid Aeilus Value Streams layer must define:

- at least one `value.*` element
- at least one `value_chain.*`
- at least one `value_segment.*`
- at least two converters
- at least one flow
- at least one participant or explicit statement that participants are not yet modeled

---

## 13. Recommended Completeness Criteria

A mature model includes:

- value chains
- value stream segments
- entry points and exit points
- full value graph
- anti-value mapping
- participants
- converters
- key results
- metrics
- cross-layer links to product context, stakeholders, business architecture, critical path, economics, and observability

---

## 14. Example Identifiers

```text
value.execution_success
value.time_saved
antivalue.execution_failure
antivalue.manual_rework
value_chain.customer_onboarding
value_segment.identity_verification
entry_point.user_request
exit_point.account_created
key_result.registration_completed
converter.workflow_engine
flow.execution_flow
participant.customer
metric.success_rate
```

---

## 15. Example AISMM Block

```md
<!-- AISMM:BEGIN -->
type: value_chain_definition
layer_id: 002
layer_key: aeilus_value_streams
document_id: value_chain.customer_onboarding
document_type: value_chain_definition
module_scope: root
status: draft
spec_version: 1.0.0
title: Customer Onboarding Value Chain
<!-- AISMM:META_END -->

# Customer Onboarding Value Chain

## Purpose

Describe how customer onboarding creates value for users and the business.

## Entry Point

- entry_point.user_need

## Segments

- value_segment.registration
- value_segment.identity_verification
- value_segment.first_success

## Exit Point

- exit_point.first_successful_use

## Key Results

- key_result.registration_completed
- key_result.time_to_value_reduced

## Participants

- participant.customer
- participant.product_team

## Value Elements

- value.account_created
- value.time_saved

## Anti-Value Elements

- antivalue.registration_delay
- antivalue.manual_rework

<!-- AISMM:END -->
```

---

## 16. Summary

The Aeilus Value Streams layer defines **how value exists, moves, transforms, accumulates, and degrades** in and around the product.

It is independent of implementation and provides the foundation for:

- product decisions
- value-based prioritization
- optimization
- economic modeling
- system evolution
- impact analysis
- observability of value delivery

<!-- AISMM:END -->
