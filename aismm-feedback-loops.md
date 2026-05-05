# AISMM Feedback Loops and Learning Cycles

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.feedback-loops
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

AISMM contains many forward links — from requirements to implementation to tests to releases. But **feedback loops** — how operational reality flows back into the model — are implicit.

This document defines how signals from production, users, audits, and AI evaluation become structured knowledge and drive product change.

---

## Core Feedback Loop Pattern

```text
signal → triggers → feedback_loop
feedback_loop → creates → change_request
change_request → updates → requirement
requirement → implemented_by → work_item
work_item → validated_by → test
test → included_in → release
release → validates → loop_outcome
loop_outcome → captured_in → knowledge_summary
```

---

## Canonical Feedback Loop Types

### 1. Incident-Driven Learning

```text
runtime_incident → root_cause_analysis → design_or_requirement_gap → change_request → fix → release → monitoring
```

Layers involved: b6.603, b8.801, b4.401, b8.804, b6.602

### 2. Customer-Feedback-Driven Discovery

```text
customer_feedback → product_analysis → hypothesis → requirement → work_item → release → validation
```

Layers involved: b0.002, b1.101, b4.401, b8.801, b8.804

### 3. Metric-Driven Optimization

```text
metric_degradation → performance_analysis → optimization_target → nfr_update → implementation → release → metric_recovery
```

Layers involved: b6.602, b4.404, b3.302, b8.804

### 4. Audit-Finding Remediation

```text
compliance_finding → risk_assessment → control_gap → remediation_plan → work_item → evidence → audit_closure
```

Layers involved: b7.704, b7.702, b7.703, b8.801, b8.805

### 5. Test-Failure-Driven Correction

```text
test_failure → failure_analysis → root_cause → requirement_or_rule_update → fix → re-test → knowledge_update
```

Layers involved: b7.701, b8.803, b4.401, b4.402, b9.904

### 6. AI/RAG-Evaluation-Driven Improvement

```text
retrieval_failure → rag_evaluation → context_gap → knowledge_update → reindex → retrieval_re-evaluation
```

Layers involved: b9.905, b9.904, b9.901, b10.1003

---

## See Also

Layer 807 in bundle b8 defines the formal specification for feedback loop entities, including `feedback_loop.*`, `feedback_signal.*`, `learning_cycle.*`, and `loop_outcome.*`.

See: [`b8-change-execution/807-feedback-loops-and-learning-cycles.md`](./b8-change-execution/807-feedback-loops-and-learning-cycles.md)

---

## Summary

Feedback loops are **closed-loop learning paths** that connect operational signals back to product knowledge and change. AISMM v2 makes these loops first-class model entities, enabling AI agents to understand not just what the system is, but how it learns and improves.
