# Bundle 10 — Data and AI Lifecycle

## Overview

Bundle 10 defines **how data products, ML features, AI models, training runs, evaluations, drift signals, labeling tasks, and data quality measurements are modeled as first-class AISMM entities**.

It is the **data and intelligence layer of AISMM**, making AI-native systems fully describable within the meta-model.

Bundle 10 is **separate from b2 (System Design) and b3 (Implementation)** because:

- b2 models data structure and APIs as design artifacts
- b3 models code, builds, and deployment artifacts
- **b10 models the operational lifecycle of data assets and AI/ML artifacts** — how they are created, versioned, evaluated, degraded, retrained, labeled, and observed

This separation is critical for AI-native systems where models drift, embeddings become stale, datasets evolve, and retraining is a continuous operational concern.

---

## Layers Overview

### 1001 — Data Products and Contracts

Models data products as owned, versioned, reusable assets with explicit contracts and SLAs.

```text
Who owns this data? What is its contract? Who consumes it?
```

### 1002 — Data Lineage and Schema Evolution

Tracks how data moves, transforms, and changes schema over time.

```text
Where does this data come from? How has it changed?
```

### 1003 — Feature and Embedding Lifecycle

Models ML features, embeddings, and vector indexes — their creation, refresh policy, invalidation, and serving.

```text
How are features created, refreshed, and invalidated?
```

### 1004 — Model Registry and Versioning

Tracks AI/ML models as versioned artifacts with model cards, inference endpoints, and promotion history.

```text
Which model version is deployed? What does it do?
```

### 1005 — Training, Evaluation and Experimentation

Models training runs, evaluation runs, experiments, and golden datasets.

```text
How was this model trained and evaluated?
```

### 1006 — Drift, Monitoring and Retraining

Detects and responds to data and model drift with explicit signals, checks, and retraining triggers.

```text
Is the model still performing as expected?
```

### 1007 — Labeling, Annotation and Ground Truth

Manages human or automated labeling tasks, annotations, and ground truth datasets.

```text
Who labeled this? How confident are we in the labels?
```

### 1008 — Data Quality and Observability

Measures quality and reliability of data with quality rules, metrics, and observability signals.

```text
Is this data reliable enough to use?
```

---

## Cross-Bundle Links

Bundle 10 connects to:

- b2.202 — data entity definitions (b10 tracks their lifecycle)
- b3.303 — build artifacts (b10 model artifacts are a specialization)
- b6.602 — monitoring (b10 adds drift and ML-specific observability)
- b7.701 — tests (b10 adds evaluation runs and golden datasets)
- b7.702 — risks (data and model risks)
- b8.804 — releases (model deployments are releases)
- b9.903 — provenance (dataset and model provenance)
- b9.905 — RAG (embedding indexes are b10 entities)

---

## Schema

```text
b10.schema.json
```

---

## Summary

Bundle 10 makes AI-native systems fully modelable in AISMM by treating data products, ML models, training pipelines, embeddings, and drift signals as governed, traceable entities — not invisible infrastructure.
