<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 104
layer_key: outcomes_and_hypothesis_validation
document_id: spec.outcomes.hypothesis.validation
document_type: layer_specification
module_scope: root
status: stable
spec_version: 3.0.0
title: Outcomes and Hypothesis Validation Layer Specification
kind_class: descriptive
<!-- AISMM:META_END -->

# 104 — Outcomes and Hypothesis Validation

## 1. Purpose of the Layer

The **Outcomes and Hypothesis Validation** layer closes the loop between **what we intended** and **what actually happened** at the level of value and business goals.

It is the **"was it worth it?"** layer. Where `b1.101` states a hypothesis and an expected outcome, `b1.104` records the **measured outcome and the verdict**, and what decision follows.

It answers:

- Did the hypothesis validate, refute, or stay inconclusive?
- What outcome did we actually observe versus the one we expected?
- How much of the intended value was realized?
- What decision follows — persevere, pivot, scale, or kill?
- What did we learn about the *bet itself* (not the delivery process)?

> V3 critique note: the analyst asked for one "retrospective" layer. We deliberately split it into two, because mixing outcome evaluation with process evaluation produces an unactionable layer. `b1.104` evaluates the **outcome of the bet** (value/goal). `b8.810` evaluates the **quality of the process/approach** that delivered it. They link to each other but answer different questions and have different owners.

---

## 1A. Product Model Representation

In a product repository, this layer is represented by the directory `aismm/b1-business-dynamics/104-outcomes-and-hypothesis-validation/`.

The questions in this specification must be answered through one or more record files inside that directory, not through a single layer file.

Each new semantic unit for this layer must create a new record file whose identity is preserved after creation.

Record naming, file IDs and preferred record kinds for layer `104` are governed by [`../aismm-layer-artifact-naming.md`](../aismm-layer-artifact-naming.md).

Preferred record kinds: `outcome`, `result`, `decision`.

---

## 2. Layer Role in AISMM

This layer connects:

Hypothesis / Goal → Measured Outcome → Verdict → Next Decision

It is the **feedback edge from execution back to strategy**. It makes value realization a first-class, traceable fact rather than tribal memory.

Other layers use it to:

- update strategy (`b1.102`) and hypotheses (`b1.101`) with evidence
- feed value-stream reality back into `b0.002`
- justify or retire requirements (`b4.401`) and initiatives
- supply the value half of the feedback-loop model (`aismm-feedback-loops.md`, loop 7)

---

## 3. Main Output

- outcome records with expected-vs-actual comparison
- validation verdicts (`validated | refuted | inconclusive`)
- value-realized assessments
- follow-on decisions (persevere/pivot/scale/kill)
- links to the originating hypothesis, goal, requirements, releases and metrics

---

## 4. Core Concepts

### 4.1 Outcome Record
A measured result tied to an intended outcome, with the measurement window and source metric.

### 4.2 Expected vs Actual
The intended outcome (from `b1.101.outcome.*`) compared with the observed value (from `b6.602` metrics, `b10` data, or `b0.002` value signals).

### 4.3 Validation Verdict
`validated`, `refuted`, `inconclusive`, or `superseded`. Must cite evidence (`b9.903`).

### 4.4 Value Realized
How much of the intended business value materialized, qualitatively and (where possible) quantitatively. Links to `b0.002` / `b0.006`.

### 4.5 Follow-on Decision
`persevere`, `pivot`, `scale`, `kill`, `re-test`. This decision is itself recorded and linked into `b8.805` decision log.

### 4.6 Attribution Confidence
How confident we are that the observed outcome was caused by the initiative (not external factors). Reuses `b9.903` confidence semantics.

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Outcome | outcome.* |
| Verdict | verdict.* |
| Value realization | value_realization.* |
| Follow-on decision | follow_on_decision.* |

---

## 6. Required Structure

### 6.1 Outcome
- id, evaluates (hypothesis/goal/initiative ref)
- expected outcome ref, observed value, measurement window, source metric ref
- attribution_confidence

### 6.2 Verdict
- id, value: validated|refuted|inconclusive|superseded
- evidence refs (`b9.903`)

### 6.3 Value Realization
- id, intended value ref (`b0.002`/`b0.006`), realized assessment

### 6.4 Follow-on Decision
- id, decision: persevere|pivot|scale|kill|re-test
- rationale, linked decision record (`b8.805`)

---

## 7. Preferred Representation

- Markdown narrative + structured YAML block per outcome
- Metric references resolved against `b6.602` / `b10.1008`
- Optional small expected-vs-actual table

---

## 8. Relationships

outcome → evaluates → b1.101.hypothesis | b0.003.goal
outcome → measured_by → b6.602.metric
outcome → produces → verdict
verdict → supported_by → b9.903.evidence
outcome → assesses → value_realization → of → b0.002.value
outcome → leads_to → follow_on_decision → recorded_in → b8.805.decision
outcome → informs → b1.102.strategy

(`evaluates` / `assesses_outcome_of` are added to the canonical taxonomy in `b9.906`.)

---

## 9. Cross-Layer Links

- `b1.101` — hypotheses being validated
- `b0.002` / `b0.003` / `b0.006` — value, goals, economics realized
- `b6.602` / `b10.1008` — metric sources for measurement
- `b8.804` — releases that delivered the change being evaluated
- `b8.810` — process retrospective for the same initiative (paired view)
- `b8.807` — feedback loop that may have triggered the bet

---

## 10. Boundaries

Does NOT include:

- the hypothesis statement itself (that is `b1.101`)
- delivery/process quality (that is `b8.810`)
- raw metric definitions (that is `b6.602`)
- the decision log container (that is `b8.805` — this layer links to it)

---

## 11. Minimal Valid Content

- at least one outcome linked to a hypothesis or goal
- expected-vs-actual comparison
- a verdict with at least one evidence reference
- a follow-on decision

---

## 12. Completeness Criteria

- every closed hypothesis in `b1.101` has a matching outcome record or an explicit "not yet measured" gap
- verdicts cite evidence and attribution confidence
- value realization links back to `b0`
- follow-on decisions are mirrored in `b8.805`

---

## 13. Summary

This layer answers **"did the bet pay off?"** It turns expected outcomes into measured verdicts with value realization and a next decision, providing the value-side closed loop that strategy and hypotheses depend on.

<!-- AISMM:END -->
