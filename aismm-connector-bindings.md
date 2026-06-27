# AISMM Connector Bindings

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.connector-bindings
  spec_version: 3.1.0
  status: accepted
  created: "2026-06-27"
  updated: "2026-06-27"
  authors: ["AISMM Governance"]
```

---

## Purpose

This is the **recommended binding catalogue** for AISMM: which common AISMM
concepts should bind to which external standard, with what `link_type`. It applies
the mechanism in [`aismm-external-model-binding.md`](./aismm-external-model-binding.md)
to concrete bundle/layer fields, using the Meta-Universe **Connector Catalogue**
(MU-V2-ECO-009) as the source of connectors.

These bindings are **recommendations, not requirements**. A product binds the ones
it uses; the `link_type` shown is the default and MAY differ in a justified context
(e.g. an Address embedded normally, but snapshot-embedded inside an audit record).

A machine-readable copy is [`aismm-external-bindings.json`](./aismm-external-bindings.json).

---

## 1. Foundational value-objects → EMBED

Structured, identity-free concepts. Embed the canonical model; do **not** flatten
its parts into separate AISMM fields.

| Concept | Connector (registry) | `kind` | `link` | Typical AISMM home |
|---------|----------------------|--------|--------|--------------------|
| Postal address | OASIS CIQ xAL · schema:PostalAddress | value_object | embed | b2.202 data entities; b0.003 stakeholder; b11.1105 vendor |
| Person name | OASIS CIQ xNL · schema name parts | value_object | embed | b0.003 stakeholder; b11.1101 person |
| Monetary amount | schema:MonetaryAmount · ISO 20022 amount | value_object | embed | b0.006 economics; b12.1201 cost; b12.1205 unit economics |
| Quantity / measure | QUDT QuantityValue · schema:QuantitativeValue | value_object | embed | b6.602 metrics; b12 cost quantities; b10.1008 data quality |
| Geographic point / geometry | GeoJSON · ISO 6709 | value_object | embed | b6.601 runtime topology (region); b0.003 location |
| Temporal interval | OWL-Time · ISO 8601 interval | value_object | embed (or mixin as valid-time) | b4.405 lifecycles; b8.804 release windows |
| Contact point | vCard · schema:ContactPoint | value_object | embed | b0.003 stakeholder; b11.1105 vendor |

## 2. Reference data / code lists → REFERENCE

Curated value sets. Reference the scheme by code (carry scheme URI + version);
never copy members into a local enum.

| Concept | Connector | `kind` | `link` | Typical AISMM home |
|---------|-----------|--------|--------|--------------------|
| Currency | ISO 4217 | code | reference | b0.006 economics; b12 finops |
| Country / region | ISO 3166 | code | reference | b0.003; b7.704 compliance jurisdiction; b12 |
| Language | ISO 639 / BCP 47 | code | reference | b5.505 localization |
| Unit of measure | UCUM · UN/CEFACT Rec 20 · QUDT unit | code | reference | b6.602 metrics; b12.1203 token/inference; b10.1008 |
| Industry / activity | NACE · ISIC · NAICS | code | reference | b0.003 market context; b11.1105 vendor |
| Occupation / skill | ESCO · O*NET | code | reference | b11.1104 skills & capability matrix |
| Security classification | CVSS · data-classification labels | code | reference (+ mixin facet) | b7.703 security; b7.705 threat; b7.706 vuln |
| Compliance control | ISO 27001 · NIST CSF · SOC 2 | code | reference | b7.703/b7.704 |
| Relationship taxonomy | SKOS concept scheme | code | reference / align | b9.906 ontology & vocabulary |

## 3. Identifier schemes → REFERENCE

Keys that point at entities. Reference the key; resolve the entity through it.

| Concept | Connector | `kind` | `link` | Typical AISMM home |
|---------|-----------|--------|--------|--------------------|
| Legal entity | ISO 17442 LEI | code | reference | b11.1102 ownership; b11.1105 vendor; b0.003 |
| Organization (DUNS) | D-U-N-S | code | reference | b11.1105 vendor |
| Software package / dependency | SPDX · CycloneDX · purl | code | reference / extend | b3.304 dependency inventory / SBOM |
| Vulnerability | CVE · CVSS | code | reference | b7.706 vulnerability management |
| Researcher / author | ORCID · ISO 27729 ISNI | code | reference | b9.903 provenance (human source) |
| Digital identity | W3C DID | code | reference | b7.703 identity; agent identity |
| Trade item / product | GS1 GTIN | code | reference | b0.003 product context (if physical goods) |

## 4. Entity models → REFERENCE (snapshot-embed only for audit)

Things with identity and lifecycle. Reference them; embed only as a marked,
immutable snapshot when audit demands a frozen copy.

| Concept | Connector | `kind` | `link` | Typical AISMM home |
|---------|-----------|--------|--------|--------------------|
| Organization | schema:Organization · W3C ORG | entity | reference | b11.1102 ownership; b11.1105 vendor |
| Person | schema:Person · FOAF | entity | reference | b0.003 stakeholder; b11.1101 |
| Place / facility | schema:Place · GeoSPARQL Feature | entity | reference | b6.601 runtime topology |
| Dataset / data product | DCAT Dataset · ODCS | entity | reference / extend | b10.1001 data products |

## 5. Cross-cutting facets → MIX-IN

Concerns applied across many records. Mix them in under their own namespace; do not
re-invent them as bespoke domain fields.

| Concept | Connector | `kind` | `link` | AISMM equivalent today |
|---------|-----------|--------|--------|------------------------|
| Provenance | W3C PROV-O | facet | mixin | b9.903 source provenance; `derivation` block |
| Data lineage | PROV-O · OpenLineage | facet | mixin | b10.1002 data lineage |
| Valid-time / temporal validity | OWL-Time · MU Lifecycle | facet | mixin | `aismm-temporal-validity.md` fields |
| Usage / access policy | W3C ODRL | facet | mixin | b4.403 access rights; `00-policies` |
| Security / classification marking | data-classification labels | facet | mixin | `security_classification`; `aismm-context-security.md` |
| Multilingual labels | SKOS-XL · RDF langString | facet | mixin | b5.505 localization; b9.906 |

## 6. Schema / modeling languages → EXTEND; upper ontologies → ALIGN

| Concept | Connector | `link` | AISMM home |
|---------|-----------|--------|------------|
| REST API contract | OpenAPI | extend | b2.203 API & interfaces (preferred representation) |
| Event/API contract | AsyncAPI | extend | b2.206 event catalog |
| Data schema | JSON Schema · XSD · SHACL | extend | b2.202 data & information architecture |
| Business process | BPMN | extend | b1.103 processes |
| SBOM | SPDX · CycloneDX | extend | b3.304 dependency inventory / SBOM |
| Upper ontology | BFO · Common Logic | align | b9.906 ontology (grounding of root types) |

---

## 7. How to apply a binding

1. Pick the concept's `composition_kind` with the rubric in
   [`aismm-external-model-binding.md`](./aismm-external-model-binding.md) §2.
2. Find the row above; note its connector and default `link`.
3. Import the standard as a Semantic Package (pin a `version`); for `reference`
   bindings record the `namespace` (scheme URI) + `version`.
4. Add the `external_binding` block to the entity/field.
5. For federation, publish a Semantic Mapping to the connector as the canonical
   meaning (b9.906).

This catalogue is a starting set — extend it per product in `00-policies` using the
same `external_binding` mechanism.
