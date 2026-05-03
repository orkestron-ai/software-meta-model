<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 101
layer_key: business_hypotheses
document_id: spec.business.hypotheses
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Business Hypotheses Layer Specification
<!-- AISMM:META_END -->

# 101 — Business Hypotheses

## 1. Purpose of the Layer

The **Business Hypotheses** layer defines assumptions about the product, market, users, value, behavior, and economics that must be validated or invalidated.

This layer captures uncertain beliefs that may lead to product changes.

It answers:

- What do we believe to be true?
- Why do we believe it?
- What value, behavior, or economic outcome do we expect?
- How will the hypothesis be validated?
- What evidence supports or contradicts it?
- What decisions should be made based on validation?

This layer is the main entry point for product discovery and business-driven change.

---

## 2. Layer Role in AISMM

This layer connects:

```text
Uncertainty → Evidence → Product Change
```

It translates business uncertainty into structured validation work.

Other layers rely on it to:

- justify changes in requirements
- explain roadmap decisions
- connect experiments with value and economics
- document why the product evolves
- distinguish validated knowledge from assumptions

This layer does NOT define:

- final requirements
- implementation tasks
- full business processes
- technical architecture
- UI or API behavior

It defines **testable business assumptions** and their validation logic.

---

## 3. Main Output of the Layer

The output is a structured set of business hypotheses, including:

- hypothesis statements
- assumptions
- expected outcomes
- validation criteria
- evidence
- decisions
- links to affected value, economics, stakeholders, strategy, and product behavior

---

## 4. Core Concepts

### 4.1 Hypothesis

A hypothesis is a testable assumption about the product or its context.

Examples:

```text
hypothesis.enterprise_clients_need_agent_auditability
hypothesis.usage_based_pricing_increases_revenue
hypothesis.workflow_templates_reduce_time_to_value
```

A good hypothesis must be:

- explicit
- testable
- connected to expected value or economic effect
- falsifiable
- linked to evidence or validation plan

---

### 4.2 Assumption

An assumption is an underlying belief that supports a hypothesis.

Examples:

```text
assumption.enterprise_clients_have_compliance_needs
assumption.users_understand_workflow_templates
```

Assumptions may be reused across multiple hypotheses.

---

### 4.3 Expected Outcome

An expected outcome describes what should happen if the hypothesis is true.

Outcomes may be:

- behavioral
- economic
- operational
- value-related
- adoption-related

Examples:

```text
outcome.increased_activation_rate
outcome.lower_support_load
outcome.higher_usage_revenue
```

---

### 4.4 Validation Metric

A validation metric defines how the hypothesis will be evaluated.

Examples:

```text
metric.activation_rate
metric.workflow_completion_rate
metric.revenue_per_active_account
metric.support_ticket_reduction
```

---

### 4.5 Experiment

An experiment is a planned activity used to validate or invalidate a hypothesis.

Examples:

```text
experiment.pricing_ab_test
experiment.prototype_user_test
experiment.workflow_template_pilot
```

Experiments may involve product changes, interviews, analytics, prototypes, or controlled releases.

---

### 4.6 Evidence

Evidence is information used to support or reject a hypothesis.

Evidence may come from:

- user interviews
- telemetry
- experiments
- sales feedback
- support tickets
- market research
- financial results
- qualitative observations

Examples:

```text
evidence.interview_batch_2026_01
evidence.activation_metric_q1
```

---

### 4.7 Decision

A decision defines what should happen based on hypothesis validation.

Examples:

```text
decision.continue_initiative
decision.stop_experiment
decision.expand_feature_scope
decision.change_pricing_model
```

---

## 5. Identifiable Entities

| Entity Type       | Identifier Prefix | Description                           |
| :---------------- | :---------------- | :------------------------------------ |
| Hypothesis        | `hypothesis.*`    | Testable business assumption          |
| Assumption        | `assumption.*`    | Supporting belief or condition        |
| Expected Outcome  | `outcome.*`       | Expected result if hypothesis is true |
| Experiment        | `experiment.*`    | Validation activity                   |
| Validation Metric | `metric.*`        | Measurement used for validation       |
| Evidence          | `evidence.*`      | Proof or signal                       |
| Decision          | `decision.*`      | Decision based on validation          |
| Risk              | `risk.*`          | Risk connected to hypothesis          |
| Segment           | `segment.*`       | Affected user/customer segment        |

Identifiers must be stable and reusable by other layers.

---

## 6. Required Content Structure

---

### 6.1 Hypothesis Statement

Each hypothesis must include:

- identifier
- short name
- clear statement
- rationale
- affected product area
- expected impact

Recommended structure:

```text
Hypothesis ID:
Name:
Statement:
Rationale:
Affected Scope:
Expected Impact:
```

---

### 6.2 Assumptions

Define assumptions that support the hypothesis.

Each assumption should include:

- identifier
- statement
- confidence level
- related hypothesis
- known uncertainty

---

### 6.3 Expected Outcomes

Define what is expected if the hypothesis is valid.

Each expected outcome should include:

- identifier
- description
- outcome type
- related value element
- related economic effect if applicable

---

### 6.4 Validation Plan

Define how the hypothesis will be checked.

Should include:

- validation method
- experiment or analysis approach
- required evidence
- success threshold
- failure threshold
- timeframe if relevant

---

### 6.5 Metrics

Define metrics used for validation.

Each metric should include:

- identifier
- definition
- measurement source
- interpretation rule

---

### 6.6 Evidence

Define collected or expected evidence.

Each evidence item should include:

- identifier
- source
- date or period
- summary
- reliability level
- relationship to hypothesis

---

### 6.7 Decision Logic

Define what decisions follow from validation.

Should include:

- continue
- stop
- pivot
- scale
- reduce scope
- create requirement
- update strategy
- update economics

---

### 6.8 Status

Each hypothesis should have a lifecycle status.

Recommended statuses:

```text
draft
ready_for_validation
validating
validated
invalidated
inconclusive
archived
```

---

## 7. Preferred Representation

The semantic content of this layer is independent of any specific representation format.

This layer defines **testable business assumptions and validation logic**, not how they must be stored or visualized.

Due to the narrative and decision-oriented nature of this layer, the following representations are considered most suitable:

- Markdown (`.md`) for hypothesis descriptions, rationale, and decision records
- Structured formats (JSON / YAML) for machine-readable hypothesis catalogs
- Tables for hypothesis registers, statuses, metrics, and decisions
- Simple diagrams for mapping hypotheses to value, stakeholders, and roadmap items

Implementations:

- SHOULD use Markdown for human-readable hypothesis documentation
- SHOULD use structured formats when automated validation, reporting, or dashboards are required
- MAY use tables to maintain hypothesis registers
- MAY use diagrams to show relationships and validation flow
- MUST NOT encode semantics in format-specific constructs

The correctness of this layer is determined by the completeness, traceability, and validation logic of its hypotheses, not by the chosen representation.

---

## 8. Relationships Inside the Layer

This layer should define relationships such as:

```text
hypothesis → based_on → assumption
hypothesis → expects → outcome
hypothesis → validated_by → experiment
hypothesis → measured_by → metric
hypothesis → supported_by → evidence
hypothesis → contradicted_by → evidence
hypothesis → leads_to → decision
decision → changes_status_of → hypothesis
```

---

## 9. Relationships With Other AISMM Layers

### Product Definition & Context

Hypotheses must be scoped to product areas, modules, channels, customer segments, or external context entities.

```text
hypothesis.* → affects → product.*
hypothesis.* → affects → module.*
hypothesis.* → relates_to → channel.*
hypothesis.* → relates_to → external.*
```

---

### Value Streams

Hypotheses may predict changes in value or anti-value.

```text
hypothesis.* → expects_change_in → value.*
hypothesis.* → expects_reduction_in → antivalue.*
```

---

### Stakeholders and Motivation

Hypotheses should be connected to stakeholder goals, motivations, or pain points.

```text
hypothesis.* → addresses → stakeholder.*
hypothesis.* → supports → goal.*
hypothesis.* → reduces → stakeholder anti-value
```

---

### Business Architecture

Hypotheses may affect business domains, capabilities, or business entities.

```text
hypothesis.* → affects → capability.*
hypothesis.* → affects → domain.*
```

---

### Critical Path

Hypotheses may target bottlenecks or steps in the critical path.

```text
hypothesis.* → improves → step.*
hypothesis.* → reduces → bottleneck.*
```

---

### Product Economics

Hypotheses may expect changes in revenue, costs, or drivers.

```text
hypothesis.* → expects → revenue.*
hypothesis.* → expects → cost.*
hypothesis.* → affects → revenue_driver.*
hypothesis.* → affects → cost_driver.*
```

---

### Strategy and Product Management

Validated hypotheses may become strategic objectives, initiatives, roadmap items, or product decisions.

```text
hypothesis.* → informs → strategy.*
hypothesis.* → leads_to → initiative.*
hypothesis.* → leads_to → roadmap_item.*
decision.* → updates → product_strategy
```

---

### Requirements

Validated hypotheses may produce requirements.

```text
hypothesis.* → leads_to → requirement.*
```

---

### Quality and Testing

Experiments and validation plans may define tests or acceptance criteria.

```text
experiment.* → defines → test.*
metric.* → validates → requirement.*
```

---

### Observability

Validation metrics may require runtime measurement.

```text
metric.* → observed_by → observability_metric.*
evidence.* → derived_from → telemetry.*
```

---

## 10. Layer Boundaries

This layer must not include:

- final product requirements
- full implementation tasks
- detailed UI design
- detailed architecture
- complete process maps
- release execution plans
- accounting reports

This layer may reference those elements, but it should not replace them.

---

## 11. Recommended Block Types

The layer may contain the following AISMM block types:

- `layer_document`
- `hypothesis_definition`
- `assumption_definition`
- `experiment_definition`
- `metric_definition`
- `evidence_definition`
- `decision_definition`

---

## 12. Minimal Valid Content

A minimal valid Business Hypotheses layer must define:

- at least one `hypothesis.*`
- hypothesis statement
- expected outcome
- validation method or validation metric
- current hypothesis status

---

## 13. Recommended Completeness Criteria

A mature version of this layer should define:

- hypothesis statements
- assumptions
- expected outcomes
- validation metrics
- experiments
- evidence
- decision logic
- status tracking
- links to stakeholders, value, economics, and roadmap

---

## 14. Example Identifiers

```text
hypothesis.workflow_templates_reduce_time_to_value
assumption.users_repeat_similar_workflows
outcome.shorter_first_success
experiment.template_pilot
metric.time_to_first_value
evidence.pilot_results_may_2026
decision.expand_template_library
```

---

## 15. Example AISMM Block

```md
<!-- AISMM:BEGIN -->
type: hypothesis_definition
layer_id: 101
layer_key: business_hypotheses
document_id: hypothesis.workflow_templates_reduce_time_to_value
document_type: hypothesis_definition
module_scope: root
status: draft
spec_version: 1.0.0
title: Workflow Templates Reduce Time to Value
<!-- AISMM:META_END -->

# Workflow Templates Reduce Time to Value

## Hypothesis Statement

Enterprise users will reach their first successful workflow execution faster if they can start from predefined workflow templates.

## Rationale

Users may not understand how to design full workflows from scratch during onboarding.

## Expected Outcome

- outcome.shorter_first_success
- metric.time_to_first_value improves by at least 30%

## Validation Plan

Run a pilot with template-based onboarding and compare time to first successful workflow execution.

## Decision Logic

If time to first value improves by at least 30%, expand the template library and create related roadmap items.

<!-- AISMM:END -->
```

---

## 16. Summary

The Business Hypotheses layer defines the structured uncertainty that drives product evolution.

It ensures that product changes are connected to assumptions, validation, evidence, and decisions.

<!-- AISMM:END -->
