# AISMM Consistency Rules

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.consistency-rules
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## 1. Purpose

This document defines **source-of-truth boundaries and projection rules** between AISMM bundles.

AISMM v2 has 13 bundles (b0–b12). Some concepts intentionally appear in multiple bundles as different views or projections of the same reality. Without explicit rules, the model risks accumulating duplications, contradictions, and unclear authority boundaries.

This document answers:

- Which bundle owns which concept?
- When the same concept appears in multiple bundles, which one wins?
- How should projections work?
- What lifecycle flows connect the bundles?

---

## 2. Source of Truth Rules

Each concept has exactly one **source of truth** bundle. Other bundles may **project** or **reference** the concept but do not own it.

| Concept | Source of Truth | Projection / Consumer |
|---------|-----------------|----------------------|
| Product value and revenue model | b0 | b1, b8, b12 |
| Product strategy and hypotheses | b1 | b8 |
| System structure and architecture | b2 | b3, b6, b9 |
| Physical implementation and artifacts | b3 | b2, b6, b8 |
| Product behavior and requirements | b4 | b5, b7, b8 |
| UX and user interaction | b5 | b4, b7 |
| Runtime state and operational facts | b6 | b7, b8, b12 |
| Quality policy and test model | b7 | b8 |
| Test execution and change history | b8 | b7, b9 |
| Organizational ownership | b11 | b9 |
| Technical cost model | b12 | b0 |
| Traceability graph and knowledge index | b9 | all bundles (projection only) |
| Knowledge retrieval context | b9 | AI/RAG consumers |

---

## 3. Data Responsibility Boundaries

### b2 / b3 / b10 Data Boundary

```text
b2 = WHAT the data model is
b3 = HOW it is implemented, packaged and deployed
b10 = HOW data assets evolve, flow, are measured, used for AI/ML and governed over time
```

Examples:

```text
data_entity.customer                    → defined in b2.202
database_migration.add_customer_status  → implemented in b3.304 / b8.809
schema_version.customer_v2             → governed in b10.1002
data_quality_rule.customer_status_not_null → governed in b10.1008
```

### b0 / b12 Economics Boundary

```text
b0 = product-level economics: value, revenue, product P&L, value/cost expectations
b12 = technical economics: infrastructure cost, service cost, tenant/request cost, token/inference cost, carbon/capacity economics
```

Cross-bundle economic flow:

```text
b12 service_unit_cost → feeds → b0 product margin
b12 token_budget → constrains → b9 context retrieval / AI usage
b0 value target → constrains → b12 cost threshold
b12 cost_of_incident → informs → b0 economics model
```

### b11 / b9 Ownership Boundary

```text
b11 is the source of truth for ownership.
b9 may contain ownership edges only as traceability projections.
```

Example:

```text
b11.1102.ownership.auth_service_team
  → projected as →
b9.902.trace_link.team_owns_auth_service
```

If b11 and b9 ownership edges disagree, **b11 wins** and b9 must be corrected.

---

## 4. Projection Rules

**b9 is a graph/index/projection layer — it does not own facts.**

```text
b9 does not own business or system facts.
b9 indexes, links, scores and projects facts from other bundles.
```

Projections from other bundles into b9:

| Fact | Source Bundle | b9 Projection |
|------|--------------|--------------|
| Component ownership | b11.1102 | `owns` edge in 902 |
| Requirement-to-test link | b4.401 + b7.701 | `validates` edge in 902 |
| Risk-to-control link | b7.702 + b7.703 | `mitigates` edge in 902 |
| Release-to-artifact link | b8.804 + b3.303 | `produces` edge in 902 |
| Data entity lineage | b10.1002 | `derives_from` edge in 902 |

When b9 is regenerated or rebuilt, it must draw from source bundles — not invent new facts.

---

## 5. Duplication Rules

### Allowed Duplication

- **Summary projections**: b9 may summarize facts from other bundles in context packages
- **Reference duplication**: a bundle may reference entities from another bundle by ID
- **Cross-layer link records**: relationship records in b9.902 reference source bundle entities

### Forbidden Duplication

- Defining the same entity type in two bundles without a clear source-of-truth rule
- Copying and maintaining the same entity data in two bundles independently
- Defining ownership in both b11 and b9 as independent facts

### Reference-only Duplication

When a bundle needs to mention a fact owned by another bundle, it must use a **reference by ID** only:

```yaml
# correct: reference by ID
cost_signal:
  id: b12.1204.cost_of_incident.payment_outage_2026
  incident_reference: b6.603.incident.payment_outage_2026  # reference, not copy

# wrong: copy of incident data
cost_signal:
  incident_title: "Payment service outage"  # duplicating b6 fact
  incident_severity: P1                      # duplicating b6 fact
```

---

## 6. Conflict Resolution Rules

When two bundles appear to define conflicting facts about the same concept:

| Conflict | Resolution |
|----------|-----------|
| b11 ownership vs b9 ownership edge | b11 wins; b9 must be updated |
| b0 product economics vs b12 technical cost | b0 defines targets; b12 defines actuals — no conflict if roles are respected |
| b2 data entity vs b10 schema version | b2 defines structural definition; b10 defines evolution — both are valid and complementary |
| b7 test definition vs b8 test execution result | b7 defines the test; b8 records execution — no conflict if roles are respected |
| b4 requirement vs b8 work item scope | b4 defines the behavior requirement; b8 records what was done — if they diverge, b4 must be updated |

General rule:

```text
If a projection disagrees with its source, the source wins.
The projection must be regenerated or corrected.
```

---

## 7. Lifecycle Orchestration

**b8 is the lifecycle orchestration layer for product/system changes.**

Bundle responsibilities in the lifecycle:

```text
b6 = owns runtime facts and incidents
b7 = owns quality, risk and compliance policies
b8 = owns change execution, planning, release, feedback loops and learning cycles
b9 = owns traceability, indexing and knowledge projections
```

### Full Closed-Loop Lifecycle

```text
runtime signal (b6)
  → incident recorded (b6.603)
  → feedback loop triggered (b8.807)
  → change request created (b8.801)
  → requirement or behavior updated (b4)
  → tests updated (b7.701)
  → implementation changed (b3)
  → CI/CD validates (b8.808)
  → release created and deployed (b8.804)
  → runtime monitoring resumes (b6.602)
  → knowledge and traceability updated (b9)
```

### Risk / Incident / Feedback Lifecycle

```text
b7 risk = potential future uncertainty
b6 incident = realized runtime event/problem
b8 feedback loop = structured learning/change cycle
```

```text
risk → may_materialize_as → incident
incident → triggers → feedback_loop
feedback_loop → creates → change_request
change_request → implements → mitigation
release → validates → mitigation_outcome
```

### Test / Validation / Runtime Monitoring Lifecycle

```text
b7 = defines test strategy, test model, quality gates and quality policies
b8 = records actual test execution, acceptance runs and change/release approval
b6 = observes runtime behavior after release
```

```text
test_case defined in b7.701
test_run executed in b8.803
release approved in b8.804
runtime metric observed in b6.602
runtime anomaly → may trigger feedback loop in b8.807
```

---

## 8. Summary

| Rule | Statement |
|------|-----------|
| b9 does not own facts | b9 indexes and projects from source bundles |
| b11 owns organizational ownership | b9 owns only the graph projection |
| b0 owns product economics | b12 owns technical cost model |
| b2 owns data structure | b3 owns implementation; b10 owns lifecycle |
| b8 is the lifecycle orchestrator | b6/b7/b9 play supporting roles |
| Source of truth wins in conflicts | Projections must be updated, not re-argued |

See also:

- [`aismm-consistency-checks.md`](./aismm-consistency-checks.md) — repeatable consistency validation guidance
- [`aismm-layer-inventory.md`](./aismm-layer-inventory.md) — complete layer inventory
- [`aismm-strict-mode.md`](./aismm-strict-mode.md) — enforcement rules
