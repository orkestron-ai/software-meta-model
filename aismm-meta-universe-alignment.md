# AISMM and the Meta-Universe Stack

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.meta-universe-alignment
  spec_version: 3.1.0
  status: accepted
  created: "2026-06-27"
  updated: "2026-06-27"
  authors: ["AISMM Governance"]
```

---

## Purpose

AISMM is one meta-model in a larger federation. This document positions AISMM in
the **Meta-Universe** stack, maps AISMM's home-grown mechanisms to the Meta-Universe
v2 standards that now formalize the same concepts, and states — for each — whether
AISMM **adopts**, **maps to**, or **keeps and cross-references** the external
standard. It also defines AISMM's Semantic Fingerprint adoption and its federation
projections.

The intent is *not* to rebuild AISMM on top of Meta-Universe, but to stop AISMM
from re-inventing what the federation already standardizes, and to make AISMM data
flow cleanly into the landscape models above it.

---

## 1. The stack

```text
Meta-Universe (MUC · MMAS · MUFP)        the open federation + semantics substrate
        ▲   imports external standards via the External Models Registry
        │   federates via MUFP + Semantic-Mapping
   ┌────┴─────────────────────────────────────────────┐
   │  ELMM — Enterprise Landscape Meta-Model           │  enterprise landscape (above PLMM)
   │  PLMM — Product Landscape Meta-Model              │  landscape between products
   │  AISMM — AI-driven Software Meta-Model  ◀── here  │  one product / system
   └───────────────────────────────────────────────────┘
```

- **Meta-Universe** is the implementation-neutral standard family (constitution,
  meta-model architecture, federation protocol) plus the **External Models
  Registry** (1180 external standards) and **Connector Catalogue** that AISMM binds
  to (see [`aismm-external-model-binding.md`](./aismm-external-model-binding.md)).
- **AISMM** describes a single product/system.
- **PLMM** (Product Landscape Meta-Model) is the landscape **above** AISMM — the
  graph between products. **ELMM** (Enterprise Landscape Meta-Model) is the
  enterprise landscape above PLMM.

> **Naming note.** Older AISMM text referred to the landscape layer as **AIPLMM**.
> The landscape meta-model is published as **PLMM**
> (`orkestron-ai/product-landscape-meta-model`); **AIPLMM** and **PLMM** denote the
> same layer. New text uses **PLMM**.

AISMM remains a **standalone specification** — it does not require Meta-Universe to
be used. Meta-Universe alignment is what makes an AISMM model *federatable*.

---

## 2. Mechanism alignment

AISMM independently developed several mechanisms that Meta-Universe v2 now
standardizes. None are torn out; each is reconciled.

| AISMM mechanism | Meta-Universe v2 equivalent | Decision |
|-----------------|-----------------------------|----------|
| External standards as **preferred representations** / `url:` refs | `Extension-Model` Semantic Packages (ARCH-005) + Composition (ARCH-016) | **ADOPT** — formalized in [`aismm-external-model-binding.md`](./aismm-external-model-binding.md) |
| `inherits_from`/`override` policy baselines | (internal; no MU equivalent — these are operating-model policies) | **KEEP** — distinct from external binding |
| Unified entity ID + `product_id`/`model_instance_id` | Identity / Canonical Semantic Name (CORE-004) | **MAP** — AISMM IDs stay product-local; §4 defines projection to canonical identity |
| `aismm-semantic-diff.md` (model-change deltas) | MUIF **Semantic Fingerprint** (ARCH-009, content-addressable hash) | **ADD** — different purpose; §3 adopts fingerprint alongside diff |
| `aismm-temporal-validity.md` (`valid_from`/`valid_to`, release-bound) | Lifecycle / three times (CORE-011) + OWL-Time facet | **MAP** — reframe as a valid-time **facet**; allow absolute time beside release-bound |
| `derivation` / ingestion run / `confidence` | Provenance-Graph (ARCH-011) + PROV-O facet | **MAP** — bind to the PROV connector (mixin) |
| `authorship_class` / `security_classification` | Security-Model (FED-012) + classification marking facet | **MAP** — bind to the marking facet (mixin) |
| Conformance **L1–L5** (model-population maturity) | Validation **V0–V5** (ARCH-006, semantic verification) | **MAP** — orthogonal axes; §5 cross-maps them |
| `aismm.registry.json` (intra-product source discovery) | Registered-Meta-Models / federated registry (ECO-001) | **MAP** — AISMM registry may reference the MU registry as a standards source |
| Extension namespace `x-<org>...` | `Extension-Model` local extensions | **ALIGN** — same intent |

The four **ADOPT/ADD** items are the substantive v3.1 changes; the **MAP** items are
cross-references that preserve AISMM's existing fields.

---

## 3. Semantic Fingerprint

AISMM adopts the Meta-Universe **Semantic Fingerprint** (MU-V2-ARCH-009): a
content-addressable hash over a canonicalized shape. It is **complementary** to
AISMM's existing model-level semantic diff:

- `aismm-semantic-diff.md` answers *"what changed in the model between two
  states?"* (typed deltas for PR review).
- The Semantic Fingerprint answers *"is this exact shape the same one I saw
  before / that you have?"* (integrity + agreement across models).

Where a record carries an `external_binding` (or projects to MUIF), it MAY carry the
fingerprint of the imported/projected shape. The fingerprint also strengthens
`aismm-physical-identity-binding.md`'s `digest_binding`. Adoption is **recommended
at L4+**, never mandatory.

---

## 4. Identity projection

AISMM IDs (`b{bundle}.{layer}.{entity_type}.{slug}`, scoped by `product_id` +
`model_instance_id`) stay the product-local source of truth. For federation, an
AISMM entity projects to a Meta-Universe **Canonical Semantic Name** without
changing its local ID:

```text
local:    b0.003.stakeholder.acme_corp   (product_id, model_instance_id scoped)
canonical: maps to schema:Organization referenced by ISO 17442 LEI (via external_binding)
```

The mapping is carried by the entity's `external_binding` (for connector-bound
entities) and/or a Semantic Mapping in `b9.906`. AISMM does not surrender its
identifiers; it publishes how they correspond.

---

## 5. Conformance vs. validation (orthogonal axes)

AISMM **L1–L5** measures *how fully a product model is populated and governed*.
Meta-Universe **V0–V5** measures *how strongly a semantic artifact is verified*.
They are different axes and are tracked independently:

| Axis | Question | Levels |
|------|----------|--------|
| AISMM conformance | How complete/governed is the product model? | L1 format → L5 agent governance |
| MU validation | How verified is the semantic artifact? | V0 → V5 |

A product MAY declare both. Binding-related checks (§8 of
[`aismm-external-model-binding.md`](./aismm-external-model-binding.md)) raise the
*validation* strength of bound shapes without changing the AISMM conformance level.

---

## 6. Federation projections (forward-looking)

To participate in PLMM/ELMM and cross-product federation, an AISMM model SHOULD be
able to emit:

- **MUIF projections** of its key shared entities — Person, Organization, Place,
  Product, Monetary Amount — using their `external_binding` connectors as the
  canonical shape.
- **Semantic Mappings** (MU-V2-FED-006) from those entities to canonical meanings,
  published once in `b9.906` and reused, so a new federation partner integrates by
  *reference* rather than bespoke negotiation.

This is the payoff of binding: the same entity an AISMM product owns locally can be
recognized by the landscape above it without copying a single field. Tooling for
emitting projections is on the [`ROADMAP.md`](./ROADMAP.md).

---

## Summary

| Topic | Position |
|-------|----------|
| Stack | Meta-Universe substrate → AISMM (product) → PLMM (landscape) → ELMM (enterprise) |
| External standards | bound via Semantic Packages + Composition (ADOPT) |
| Identity / temporal / provenance / security / registry | MAP — existing AISMM fields kept, cross-referenced |
| Semantic Fingerprint | ADD — content-addressable integrity, complements semantic diff |
| Conformance L1–L5 vs validation V0–V5 | orthogonal, both declarable |
| Federation | MUIF projections + Semantic Mappings of key entities |

AISMM stays sovereign and standalone, but now plugs into the federation: it binds
to the world's standards below it and projects cleanly into the landscape above it.
