# AISMM v3.1.0 — External Model Binding and Meta-Universe Stack Alignment

**Released 2026-06-27. MINOR (additive) release.** Every AISMM 3.0.0 document
remains valid and unchanged in meaning.

## Highlights

AISMM v3.1 lets a product **bind to the world's existing standards instead of
re-inventing their fields**, and positions AISMM inside the **Meta-Universe**
federation stack.

- **External model binding** — a first-class `external_binding` mechanism with a
  concept-kind rubric (`attribute` / `value_object` / `entity` / `code` / `facet`)
  and six link types (`embed` / `reference` / `mixin` / `extend` / `align` /
  `annotate`). Stop forking addresses, money, units, codes and identifiers across
  layers and products. → [`aismm-external-model-binding.md`](./aismm-external-model-binding.md)
- **Connector bindings catalogue** — recommended bindings per bundle (Address →
  OASIS xAL, currency → ISO 4217, units → UCUM, organization → LEI, provenance →
  W3C PROV, …), with machine-readable
  [`aismm-external-bindings.json`](./aismm-external-bindings.json) and a JSON Schema
  for the binding object. → [`aismm-connector-bindings.md`](./aismm-connector-bindings.md)
- **Meta-Universe stack alignment** — AISMM positioned under PLMM/ELMM on the
  Meta-Universe substrate; home-grown mechanisms (identity, temporal validity,
  provenance, security, registry) mapped to their Meta-Universe v2 equivalents; the
  content-addressable **Semantic Fingerprint** adopted alongside the existing
  model-level semantic diff. → [`aismm-meta-universe-alignment.md`](./aismm-meta-universe-alignment.md)

## What this builds on

The bindings draw from the Meta-Universe v2 **External Models Registry** (1180
external standards across 37 domains, each classified by compositional role and
default link type) and its curated **Connector Catalogue**. See
<https://github.com/orkestron-ai/Meta-Universe>.

## Compatibility

Fully additive. `external_binding` and `composition_kind` are **optional** block
metadata. Binding is recommended at conformance **L4+** but never mandatory — a
product with no external standards in a layer simply has no bindings there.

See [`CHANGELOG.md`](./CHANGELOG.md) and
[`migrations/migration.aismm_v3_0_to_v3_1.external_model_binding.json`](./migrations/migration.aismm_v3_0_to_v3_1.external_model_binding.json).
