<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 101
layer_key: business_hypotheses
document_id: spec.business.hypotheses
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.1.0
title: Business Hypotheses Layer Specification
<!-- AISMM:META_END -->

# 101 — Business Hypotheses

## 1. Purpose of the Layer

The **Business Hypotheses** layer defines structured, testable assumptions about the product, market, users, value, behavior, and economics.

It captures uncertainty and turns it into **validated knowledge and decisions**.

It answers:

- What do we believe and why?
- What outcome do we expect?
- How will we validate it?
- What evidence supports or contradicts it?
- What decision follows from validation?

---

## 2. Layer Role in AISMM

This layer connects:

Uncertainty → Evidence → Decision → Change

It is the **entry point of product evolution**.

Other layers use it to:

- generate requirements
- prioritize initiatives
- justify changes
- connect value, behavior, and economics to decisions

---

## 3. Main Output

- hypotheses
- assumptions
- expected outcomes
- validation plans
- metrics
- evidence
- decisions
- links to value, stakeholders, and economics

---

## 4. Core Concepts

### 4.1 Hypothesis

A testable statement about expected change.

Must be:
- explicit
- falsifiable
- measurable
- linked to value or economics

---

### 4.2 Assumption

A belief supporting a hypothesis.

---

### 4.3 Expected Outcome

Result if hypothesis is true.

---

### 4.4 Validation Metric

Defines measurement of outcome.

---

### 4.5 Experiment

Activity to validate hypothesis.

---

### 4.6 Evidence

Data supporting or contradicting hypothesis.

---

### 4.7 Decision

Action based on validation.

---

### 4.8 Business Signal (NEW)

A trigger indicating need for hypothesis:

- market change
- KPI deviation
- stakeholder feedback
- operational issue

---

### 4.9 Decision Horizon (NEW)

Timeframe for decision:

- short-term
- mid-term
- strategic

---

### 4.10 Impact Scope (NEW)

Defines what is affected:

- value streams
- modules
- capabilities
- economics

---

## 5. Identifiable Entities

| Entity | Prefix |
|-------|--------|
| Hypothesis | hypothesis.* |
| Assumption | assumption.* |
| Outcome | outcome.* |
| Experiment | experiment.* |
| Metric | metric.* |
| Evidence | evidence.* |
| Decision | decision.* |
| Signal | signal.* |

---

## 6. Required Structure

### 6.1 Hypothesis

- id
- statement
- rationale
- business_signal
- decision_horizon
- impact_scope

---

### 6.2 Assumptions

- id
- statement
- confidence

---

### 6.3 Expected Outcomes

- id
- description
- related value/economics

---

### 6.4 Validation Plan

- method
- metrics
- thresholds
- timeframe

---

### 6.5 Metrics

- definition
- source
- interpretation

---

### 6.6 Evidence

- source
- reliability
- summary

---

### 6.7 Decision Logic

- continue / stop / pivot / scale

---

### 6.8 Status

- draft
- validating
- validated
- invalidated
- archived

---

## 7. Preferred Representation

- Markdown for narrative
- JSON/YAML for structured data

---

## 8. Relationships

hypothesis → based_on → assumption  
hypothesis → expects → outcome  
hypothesis → validated_by → experiment  
hypothesis → measured_by → metric  
hypothesis → supported_by → evidence  
hypothesis → leads_to → decision  
hypothesis → triggered_by → signal  

---

## 9. Cross-Layer Links

Value:
hypothesis → affects → value.*

Stakeholders:
hypothesis → addresses → stakeholder.*

Economics:
hypothesis → affects → revenue/cost

Behavior:
hypothesis → leads_to → requirement.*

Strategy:
hypothesis → informs → initiative.*

---

## 10. Boundaries

Does NOT include:

- final requirements
- implementation
- UI
- full processes

---

## 11. Minimal Valid Content

- at least one hypothesis
- expected outcome
- validation method
- status

---

## 12. Completeness Criteria

- hypotheses + assumptions
- validation plans
- evidence
- decisions
- links to value/economics/stakeholders

---

## 13. Summary

This layer defines **why the product changes and how uncertainty becomes decisions**.

<!-- AISMM:END -->
