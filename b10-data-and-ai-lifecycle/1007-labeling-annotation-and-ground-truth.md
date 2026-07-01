<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1007
layer_key: labeling_annotation_and_ground_truth
document_id: spec.data_ai.labeling_ground_truth
document_type: layer_specification
module_scope: b10
status: accepted
spec_version: 2.0.0
title: Labeling, Annotation and Ground Truth Layer Specification
<!-- AISMM:META_END -->

# 1007 — Labeling, Annotation and Ground Truth

## 1. Purpose

Manage **human or automated labeling tasks, annotations, and ground truth datasets**.

This layer answers who labeled a data item, how confident we are in the labels, and how ground truth datasets are assembled from annotations.

---

## 2. Layer Role in AISMM

This layer models the **provenance and quality of labels** that feed evaluation (b10.1005) and training. It makes annotator identity and label confidence explicit so ground truth is auditable rather than assumed.

---

## 3. Main Output

Labeling tasks with annotations, annotator identities, resulting ground truth datasets, and label quality assessments.

---

## 4. Core Concepts

**labeling_task** — A defined task producing labels over a set of data items.

**annotation** — A single label applied to a data item within a task.

**annotator** — The human or automated agent that produced an annotation.

**ground_truth** — A curated dataset of accepted labels used as reference.

**label_quality** — An assessment of annotation reliability (agreement, confidence).

---

## 5. Identifiable Entities

```text
labeling_task.*
annotation.*
annotator.*
ground_truth.*
label_quality.*
```

---

## 6. Required Structure

```yaml
labeling_task:
  id: labeling_task.<domain>.<name>
  name: string
  dataset: entity_id
  guidelines: string

annotation:
  id: annotation.<task>.<n>
  labeling_task: labeling_task entity_id
  annotator: annotator entity_id
  item: entity_id
  label: string

annotator:
  id: annotator.<name>
  kind: human | automated

ground_truth:
  id: ground_truth.<domain>.<name>
  dataset: entity_id
  source_task: labeling_task entity_id

label_quality:
  id: label_quality.<task>
  agreement: number                 # e.g. inter-annotator agreement
  confidence: number
```

---

## 7. Cross-Layer Links

- `b10.1005` — golden datasets built from ground truth
- `b11.1104` — skills and capability matrix (annotator competency)
- `b9.903` — source provenance and confidence of labels
- `b7.701` — quality assurance (label quality feeds evaluation validity)

---

## 8. Summary

Layer 1007 makes **ground truth traceable to who produced it and how reliable it is**, so downstream evaluation rests on auditable, quality-assessed labels.

<!-- AISMM:END -->
