<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 810
layer_key: retrospectives_and_process_effectiveness
document_id: spec.retrospectives.process.effectiveness
document_type: layer_specification
module_scope: root
status: stable
spec_version: 3.0.0
title: Retrospectives and Process Effectiveness Layer Specification
kind_class: descriptive
<!-- AISMM:META_END -->

# 810 — Retrospectives and Process Effectiveness

## 1. Purpose of the Layer

The **Retrospectives and Process Effectiveness** layer evaluates **how well the way we worked served the goal** — the quality of the process, decisions, and approach used to deliver an initiative.

Where `b1.104` asks *"did the bet pay off?"* (value), `b8.810` asks *"was the path good?"* (process). It is the structured, traceable form of an engineering/product retrospective.

It answers:

- For a given initiative/cycle, what worked and what did not in *how we executed*?
- Were our decisions, estimates, and approach effective at reaching the goal?
- How effective was the process (flow, rework, delays, defects escaped)?
- What concrete improvement actions follow, and who owns them?
- Did previous improvement actions actually change anything?

> V3 critique note: this layer exists because the analyst's single "retrospective" idea conflated outcome and process. Keeping process retrospectives here (linked to delivery in `b8`) and outcome verdicts in `b1.104` prevents an unactionable mixed layer and gives each a correct owner.

---

## 1A. Product Model Representation

In a product repository, this layer is represented by the directory `aismm/b8-change-execution/810-retrospectives-and-process-effectiveness/`.

The questions in this specification must be answered through one or more record files inside that directory, not through a single layer file.

Each new semantic unit for this layer must create a new record file whose identity is preserved after creation.

Record naming, file IDs and preferred record kinds for layer `810` are governed by [`../aismm-layer-artifact-naming.md`](../aismm-layer-artifact-naming.md).

Preferred record kinds: `retro`, `lesson`, `improvement`.

---

## 2. Layer Role in AISMM

This layer connects:

Initiative / Cycle → Process Assessment → Lessons → Improvement Actions → (verified) Change in Practice

It is the **process-side closed loop**. It makes "how we work" an evolvable, measurable, traceable property of the product, not folklore lost after each sprint.

Other layers use it to:

- improve planning and delivery flow (`b8.802`)
- adjust ways-of-working policies in the product operating model (`00-policies`)
- inform decision rights / governance (`b11.1103`)
- feed the process half of the feedback-loop model (`aismm-feedback-loops.md`, loop 7)

---

## 3. Main Output

- retrospective records per initiative/cycle
- effectiveness assessment of process, decisions, and approach
- lessons learned (positive and negative)
- improvement actions with owners and target dates
- verification of whether prior improvements took effect

---

## 4. Core Concepts

### 4.1 Retrospective Record
An assessment of an initiative or time-boxed cycle, scoped to a clear unit of delivery.

### 4.2 Effectiveness Assessment
A judgement (with optional flow metrics) of how well the process/approach served the goal: e.g. lead time, rework rate, escaped defects, decision latency, plan-vs-actual.

### 4.3 Lesson
A reusable insight about *how we work*. Distinct from a feedback signal (`b8.807`) and from an outcome verdict (`b1.104`).

### 4.4 Improvement Action
A concrete, owned, trackable change to process/policy. Links to a work item (`b8.801`) and/or a normative change in `00-policies`.

### 4.5 Improvement Verification
A later check on whether a prior improvement action actually changed practice/metrics — closing the loop on the loop.

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Retrospective | retrospective.* |
| Effectiveness assessment | effectiveness.* |
| Lesson | lesson.* |
| Improvement action | improvement.* |
| Improvement verification | improvement_verification.* |

---

## 6. Required Structure

### 6.1 Retrospective
- id, scope (initiative/cycle ref), participants/owner
- assesses (work items, releases, decisions evaluated)

### 6.2 Effectiveness Assessment
- id, dimensions (process, decisions, approach)
- optional flow metrics, plan-vs-actual
- effectiveness rating + rationale

### 6.3 Lessons
- id, statement, polarity (kept|change), evidence

### 6.4 Improvement Actions
- id, action, owner (`b11`), target date
- linked work item (`b8.801`) or policy change (`00-policies`)

### 6.5 Improvement Verification
- id, verifies (prior improvement ref), observed effect, status

---

## 7. Preferred Representation

- Markdown narrative + structured YAML per retrospective
- Optional flow-metric table (lead time, cycle time, rework, escaped defects)
- Links to evaluated work items, releases, decisions

---

## 8. Relationships

retrospective → assesses → b8.801.work_item | b8.804.release | b8.805.decision
retrospective → produces → effectiveness | lesson
retrospective → produces → improvement
improvement → implemented_by → b8.801.work_item
improvement → changes → 00-policies.policy | 00-policies.method
improvement → verified_by → improvement_verification
retrospective → paired_with → b1.104.outcome (same initiative)

(`evaluates` / `assesses_outcome_of` reused from `b9.906`.)

---

## 9. Cross-Layer Links

- `b1.104` — outcome verdict for the same initiative (paired)
- `b8.802` — planning/flow that the retrospective informs
- `b8.807` — feedback loops (mechanical) vs retrospective (evaluative)
- `b8.805` — decisions evaluated and improvement decisions recorded
- `b11.1103` — governance/decision-rights adjustments
- `00-policies` — ways-of-working/method changes are normative and live here

---

## 10. Boundaries

Does NOT include:

- value/outcome verdicts (that is `b1.104`)
- raw mechanical feedback signals (that is `b8.807`)
- the decision log container (that is `b8.805`)
- the ways-of-working policies themselves (those live in `00-policies`; this layer proposes changes to them)

---

## 11. Minimal Valid Content

- at least one retrospective scoped to a real initiative/cycle
- an effectiveness assessment with rationale
- at least one improvement action with an owner

---

## 12. Completeness Criteria

- significant initiatives/cycles have retrospectives
- improvement actions are owned and linked to work items or policy changes
- prior improvement actions are eventually verified (loop closure)
- retrospectives are paired with outcome records in `b1.104` where applicable

---

## 13. Summary

This layer answers **"was the way we worked good, and how do we improve it?"** It captures process effectiveness, lessons, and owned improvement actions, and verifies that improvements actually land — the process-side complement to `b1.104` outcome validation.

<!-- AISMM:END -->
