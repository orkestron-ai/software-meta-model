# AISMM Standards and Inheritance

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.standards-and-inheritance
  spec_version: 3.0.0
  status: accepted
  created: "2026-05-23"
  updated: "2026-05-23"
```

---

## Purpose

The `00-policies` content of many products is **largely the same** (Definition of Done, branching policy, validation gates, ingestion policy, security baselines). V3 formalizes a **shared standards baseline** that products **inherit and specialize**, so each product's `00-policies` stays small and only records its *deviations*.

This answers the analyst conclusion *"a separate layer will propose standards for filling layer 00, because it will be similar across products."*

---

## 1. Where Standards Live

Standards are a **landscape-level (AIPLMM) concern** by definition — they are shared across products. V3 supports two placements:

```text
A) Landscape (recommended):
   a shared standards repository referenced from each product's aismm.registry.json
   ( e.g. product-landscape/standards/ ... )

B) Local mirror:
   00-meta/software-meta-model-standards-main/
   a product-agnostic baseline tree, mirrored into the product repo (like the template mirror)
```

The third leg of the `00-meta` family:

```text
00-meta/
  software-meta-model-main/            # canon: how the model works
  software-meta-model-template-main/   # structural mirror: empty examples
  software-meta-model-standards-main/  # NEW in v3: normative baseline for 00-policies
```

`-standards-main` contains **baseline normative records** (`kind_class: normative`, `scope: baseline`) that products import into `00-policies`.

---

## 2. The Inheritance Contract

The value is not "another folder of standards" — it is the **inheritance + override contract**. A product policy may adopt a baseline as-is, extend it, or override it, always explicitly.

```yaml
# a product 00-policies record
policy:
  id: policy.branching
  kind_class: normative
  scope: product
  inherits_from: standard:branching.trunk_based@1.2.0
  override:
    - field: freeze_windows
      reason: "DatsTech.TV has live-event freeze windows around scheduled streams"
  status: active
```

Resolution precedence (highest wins):

```text
product override  >  product adoption  >  inherited baseline default
```

| Field | Meaning |
|-------|---------|
| `inherits_from` | `standard:<id>@<version>` the record adopts |
| `override` | explicit list of deviations, each with a `reason` |
| `scope` | `baseline` (in standards tree) or `product` (in `00-policies`) |
| `adopts_verbatim` | boolean; if true and no override, the record may be a thin pointer |

---

## 3. Thin Adoption

A product that fully adopts a baseline does **not** copy its text. It stores a thin pointer:

```yaml
policy:
  id: policy.definition_of_done
  inherits_from: standard:definition_of_done@2.0.0
  adopts_verbatim: true
  scope: product
```

This keeps `00-policies` lean across many products — the whole point of the analyst's conclusion. Only deviations carry weight.

---

## 4. Versioning and Drift

- Baselines are semantically versioned (`@MAJOR.MINOR.PATCH`).
- When a baseline updates, products see a **drift signal**: `inherits_from` points to an older version. This is surfaced in `aismm-health.md`.
- Products choose when to re-adopt; nothing is force-pushed into a product.
- Overrides must be re-justified on major baseline changes.

---

## 5. Relationship to the Template Mirror

| Tree | Provides | Class |
|------|----------|-------|
| `software-meta-model-main` | the canon: bundles, layers, the questions each layer answers | meta |
| `software-meta-model-template-main` | structural examples (empty layer format) | structural |
| `software-meta-model-standards-main` | normative baselines for `00-policies` | normative |

The template tells you **how to format** an empty layer. The standards tree tells you **what your default policies are**. They are complementary.

---

## 6. Validation Rules

| Rule | Requirement |
|------|-------------|
| Explicit inheritance | A product policy that matches a baseline SHOULD declare `inherits_from`, not silently duplicate it |
| Override justification | Every `override` entry must carry a `reason` |
| Version pinning | `inherits_from` must pin a version |
| Drift visibility | Outdated `inherits_from` versions are reported by health checks |
| Baseline immutability | A published baseline version is immutable; changes create a new version |

---

## Summary

V3 adds a shared, versioned standards baseline and an explicit inheritance/override contract so that the product operating model (`00-policies`) is mostly inherited and only deviations are product-specific. Standards are ideally a landscape (AIPLMM) artifact; a local `software-meta-model-standards-main` mirror is the concrete mechanism in a single repo.
