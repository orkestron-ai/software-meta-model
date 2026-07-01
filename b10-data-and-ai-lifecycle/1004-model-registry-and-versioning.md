<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1004
layer_key: model_registry_and_versioning
document_id: spec.data_ai.model_registry
document_type: layer_specification
module_scope: b10
status: accepted
spec_version: 2.0.0
title: Model Registry and Versioning Layer Specification
<!-- AISMM:META_END -->

# 1004 — Model Registry and Versioning

## 1. Purpose

Track **AI/ML models as versioned artifacts** with model cards, inference endpoints, and promotion history.

This layer answers which model version is deployed, what it does, where its artifact lives, and how it is served.

---

## 2. Layer Role in AISMM

This layer is the **system of record for models**. It gives every model a stable identity and a versioned history, and links each version to the training and evaluation evidence (b10.1005) that justified its promotion.

---

## 3. Main Output

Registered models with versioned artifacts, model cards, and inference endpoints.

---

## 4. Core Concepts

**model** — A named model with a stable identity across versions.

**model_version** — A specific, immutable version of a model.

**model_card** — Documentation of a model version's intended use, limitations, and evaluation results.

**model_artifact** — The stored binary or package for a model version.

**inference_endpoint** — A serving endpoint exposing a model version for inference.

---

## 5. Identifiable Entities

```text
model.*
model_version.*
model_card.*
model_artifact.*
inference_endpoint.*
```

---

## 6. Required Structure

```yaml
model:
  id: model.<domain>.<name>
  name: string
  task: string                      # e.g. classification, ranking, generation

model_version:
  id: model_version.<model>.<version>
  model: model entity_id
  version: string
  status: candidate | staging | production | retired

model_card:
  id: model_card.<model_version>
  model_version: model_version entity_id
  intended_use: string
  limitations: string

model_artifact:
  id: model_artifact.<model_version>
  model_version: model_version entity_id
  uri: string
  format: string

inference_endpoint:
  id: inference_endpoint.<model_version>
  model_version: model_version entity_id
  url: string
  status: active | deprecated
```

---

## 7. Cross-Layer Links

- `b10.1005` — training and evaluation runs that produced the model version
- `b3.303` — build and deployment artifacts (model artifacts are a specialization)
- `b8.804` — release and rollout management (model deployments are releases)
- `b6.601` — runtime environment and topology (where endpoints run)

---

## 8. Summary

Layer 1004 makes **every model version an identifiable, documented, and traceable artifact**, replacing ambiguous "latest model" references with a governed registry.

<!-- AISMM:END -->
