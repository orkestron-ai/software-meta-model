# AISMM External Model Binding

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.external-model-binding
  spec_version: 3.1.0
  status: accepted
  created: "2026-06-27"
  updated: "2026-06-27"
  authors: ["AISMM Governance"]
```

---

## Purpose

AISMM describes a product's own entities, but most products also handle concepts
the world already models — postal addresses, money, units, country/currency codes,
organizations, identifiers, provenance. Re-inventing these as ad-hoc fields forks
them: two AISMM models (or two layers of the same model) end up with diverging
shapes that cannot be joined, validated or federated.

This document defines how an AISMM entity or field **binds to an external
standard** instead of re-defining it. It is AISMM's adoption of the **Meta-Universe
v2 composition layer** — the decision rule for *field vs. nested model vs.
reference*, and the mechanisms to wire them together.

> **Scope distinction.** [`aismm-standards-and-inheritance.md`](./aismm-standards-and-inheritance.md)
> governs inheritance of **internal policy baselines** (Definition of Done,
> branching, gating). This document governs binding to **external semantic and data
> standards** (ISO 4217, OASIS xAL, UCUM, W3C PROV, LEI, …). They are
> complementary: one shapes *how the team works*, the other *what the data means*.

The companion [`aismm-connector-bindings.md`](./aismm-connector-bindings.md) lists
the concrete, recommended bindings per bundle/layer; this document defines the
mechanism behind them.

---

## 1. Where the standards come from

External standards are catalogued in the **Meta-Universe External Models Registry**
(1180 standards across 37 domains), each classified by a **compositional role**
(R1–R8) and a **default link type**. The curated **Connector Catalogue** lists the
foundational connectors (Address, Money, Quantity, Country, Currency, LEI, GTIN,
PROV, ODRL, …). AISMM does not duplicate that registry; it **references** it.[^mu]

[^mu]: Meta-Universe v2 — `External-Models-Registry` (MU-V2-ECO-008),
`Connector-Catalogue` (MU-V2-ECO-009), `Meta-Model-Composition` (MU-V2-ARCH-016),
`Extension-Model` (MU-V2-ARCH-005), `MMAS-Interchange` / Semantic Fingerprint
(MU-V2-ARCH-009), `Semantic-Mapping` (MU-V2-FED-006), `Validation` (MU-V2-ARCH-006).
See <https://github.com/orkestron-ai/Meta-Universe>.

---

## 2. The concept-kind rubric (field vs. nested model vs. reference)

Before binding, classify the concept. Every field or entity is exactly one
**`composition_kind`**:

| Kind | Definition | Test | Default treatment |
|------|------------|------|-------------------|
| `attribute` | A literal with no identity, no internal structure, no external authority | "Just a value the host owns?" | plain field |
| `value_object` | A structured bundle whose parts only make sense together; no identity of its own | "≥2 parts that travel as a unit?" | **EMBED** an external model |
| `entity` | A thing with its own identity and lifecycle, referenceable independently | "Can other records point at it; can it be created/owned on its own?" | **REFERENCE** by identifier |
| `code` | A member of a value set / classification / identifier scheme governed elsewhere | "Is the allowed-value set maintained by someone else?" | **REFERENCE** the scheme |
| `facet` | A cross-cutting concern attached to many records (provenance, valid-time, policy, security marking, labels) | "Is it *about* the data, not part of the domain payload?" | **MIX-IN** a facet model |

Apply the tests in order: **facet → identity → authority → structure →** otherwise
`attribute`. The first match fixes the kind.

### When duplication is acceptable

Binding is the default, not an absolute. A plain field — even one that recurs
across models — is **correct** for: atomic attributes (a `title`, a free-text
`note`); deliberate immutable **snapshots** (a frozen value copy for audit, marked
as such); and genuinely local one-off values where a binding would add coupling
without payoff. Duplication is a **defect** only when it forks a *structured*
value-object, copies a *governed* code list, inlines an *entity*, or re-invents a
*facet*.

---

## 3. Link types

Six mechanisms connect an AISMM concept to an external standard. The
`composition_kind` selects the default, but the author records the chosen
`link_type` explicitly.

| `link_type` | Meaning | Used for | Coupling / sovereignty |
|-------------|---------|----------|------------------------|
| `embed` | The field's type **is** the external model; the value travels inside the record | `value_object` | tight; host owns the composite, external shape reused |
| `reference` | The field holds an **identifier** resolved against an external entity or code scheme | `entity`, `code` | loose; referent stays sovereign |
| `mixin` | A cross-cutting facet's properties are applied uniformly under their own namespace | `facet` | orthogonal |
| `extend` | The AISMM concept **specializes** an external schema/modeling language | reusable schema languages (XSD, JSON Schema, SHACL, OpenAPI, SPDX) | tight (subtype) |
| `align` | AISMM declares ontological grounding against an upper ontology, without nesting | upper ontologies (BFO, Common Logic) | none structural |
| `annotate` | A controlled-vocabulary concept is attached for meaning, not structure | any field (SKOS/schema.org tag) | none |

Cross-authority **equivalence** between two sovereign models (e.g. AISMM
`stakeholder` ↔ `schema:Person` ↔ `foaf:Person`) is *not* a structural binding —
it is a **Semantic Mapping** (§7), declared separately.

---

## 4. The `external_binding` block

A binding is declared as an optional metadata object on an entity or field. It is
additive: records without it remain valid.

```yaml
external_binding:
  target: b2.202.data_entity.customer.address   # what is being bound (entity or field path)
  composition_kind: value_object                # the rubric result (§2)
  link_type: embed                              # the mechanism (§3)
  standard_id: "OASIS CIQ xAL"                  # registry standard
  connector: "Postal Address"                   # optional: curated connector (ECO-009)
  namespace: "urn:oasis:names:tc:ciq:xal:3"     # canonical URI/prefix
  version: "3.0"                                # pinned standard version
  semantic_package: "ciq-address@3.0"           # the Semantic Package this import belongs to (§5)
  fingerprint: "sha256:…"                       # optional Semantic Fingerprint of the imported shape (§6)
  notes: "embedded; do not flatten into street/city/postcode fields"
```

Required: `target`, `composition_kind`, `link_type`, `standard_id`, `version`.
Recommended: `namespace`, `connector`, `semantic_package`. Optional: `fingerprint`,
`notes`.

A machine-readable schema is provided as
[`aismm-external-binding.schema.json`](./aismm-external-binding.schema.json).

---

## 5. Imports are Semantic Packages

An external standard is **not** imported as a loose copy of fields. It is imported
as a **Semantic Package**: a named, versioned, self-describing unit
(`<name>@<version>`) with its source authority, imported namespace, selected
concepts, local extensions and a compatible version range — exactly as a software
package manager treats a dependency. This mirrors Meta-Universe `Extension-Model`
and AISMM's own thin-adoption discipline for policy baselines: **adopt by
reference, never by silent copy.**

- The originating standard remains the authority for its semantics, identifiers and
  versions; AISMM owns only its local extensions.
- A binding pins a `version`; a newer upstream version raises a **drift signal**
  (surfaced by `aismm-health.md`), never an automatic migration.
- Local extensions to an imported concept use the AISMM extension namespace
  `x-<org>.<domain>.<entity>` and remain separately identifiable.

---

## 6. Semantic Fingerprint (optional, recommended at L4+)

A binding MAY carry a **Semantic Fingerprint** — a content-addressable hash of the
imported shape, computed per the Meta-Universe interchange canonicalization
(MU-V2-ARCH-009). It lets two AISMM models that both "have an address" prove they
agree on *what an address is*, and lets `aismm-physical-identity-binding.md`'s
`digest_binding` and `aismm-semantic-diff.md` detect when an imported standard
changed shape. The fingerprint complements, and does not replace, AISMM's
model-level semantic diff.

---

## 7. Federation: projections and mappings

Once a concept is bound, AISMM data can flow into the landscape models (PLMM,
ELMM) and across products without field duplication:

- **Projection** — an AISMM entity bound to a connector can be emitted as a
  Meta-Universe interchange (MUIF) projection of that canonical shape.
- **Semantic Mapping** — where two models use different standards, a crosswalk to a
  shared **canonical meaning** (per MU-V2-FED-006) is published once and reused,
  instead of negotiating a bespoke per-pair integration. AISMM declares such
  mappings in `b9.906` (ontology, vocabulary and relationship taxonomy).

This keeps interoperability a matter of *reference* rather than *negotiation*, and
preserves each standard's sovereignty.

---

## 8. Validation rules

| Rule | Requirement | Severity |
|------|-------------|----------|
| Kind declared | A bound field/entity declares `composition_kind` | warning |
| Link valid | `link_type` ∈ {embed, reference, mixin, extend, align, annotate} and consistent with the kind (§3) | error |
| Version pinned | `external_binding.version` is present and pinned | error |
| Code carries scheme | A `code` binding records `namespace` (scheme URI) + `version` so drift is detectable | error |
| No value-object fork | A cluster of flat fields reproducing a known value-object connector (address parts, amount+currency, value+unit) SHOULD bind instead of flattening | warning |
| Snapshot marked | An embedded copy of an `entity` is marked as a snapshot with its source identifier | warning |
| Drift visible | An `external_binding.version` older than the upstream is reported by health checks | info |

These extend [`aismm-consistency-checks.md`](./aismm-consistency-checks.md) and
[`aismm-strict-mode.md`](./aismm-strict-mode.md). Binding metadata is part of the
record and therefore covered by semantic diff and (where present) the Semantic
Fingerprint.

---

## 9. Conformance

External-model binding is **optional at L1–L3** and **recommended at L4–L5**:

- **L4** — value-objects and code lists that recur in the model SHOULD bind to
  connectors rather than flatten; bindings carry `namespace` + `version`.
- **L5** — bindings additionally carry a `semantic_package` and, where federation
  is in scope, a Semantic Fingerprint and published Semantic Mappings for the key
  shared entities (Person, Organization, Place, Product, Money).

Binding never becomes *mandatory* — a product with no external standards in a layer
simply has no bindings there. The goal is to stop re-inventing what already exists,
not to force adoption.

---

## Summary

| Topic | Definition |
|-------|-----------|
| Concept kind | `attribute` · `value_object` · `entity` · `code` · `facet` (§2) |
| Link type | `embed` · `reference` · `mixin` · `extend` · `align` · `annotate` (§3) |
| Declaration | `external_binding` block on entity/field (§4) |
| Import unit | Semantic Package `<name>@<version>` — adopt by reference (§5) |
| Integrity | optional Semantic Fingerprint of the imported shape (§6) |
| Federation | MUIF projection + Semantic Mapping to canonical meaning (§7) |
| Source of standards | Meta-Universe External Models Registry + Connector Catalogue (§1) |

AISMM binds to the world's standards instead of forking them — a concept is
modelled once, reused everywhere, and every external authority keeps its
sovereignty.
