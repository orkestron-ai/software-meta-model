# AISMM Changelog

All notable changes to AISMM are documented here.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
AISMM uses [Semantic Versioning](./aismm-versioning-and-conformance.md).

---

## AISMM 3.0.0 — Content Classification, Outcomes, Domain Knowledge, Governed Ingestion and Owner Validation

MAJOR release. Driven by product-model field experience (DatsTech.TV) and six analyst conclusions.

### Added

- `aismm-content-classification.md` — `kind_class` (normative/descriptive/projection) and the `00-policies` product operating model (rules-vs-data split)
- `aismm-standards-and-inheritance.md` — shared standards baseline + `inherits_from`/`override` inheritance contract; new tree `software-meta-model-standards-main/`
- `aismm-ingestion-and-source-binding.md` — governed ingestion pipeline (owning system → MCP connector → versioned recipe → run provenance)
- `aismm-owner-validation-and-attestation.md` — mandatory, source-owner-keyed validation gate for derived records; `derivation` block; `validation_status` lifecycle
- `b0-product-core/007-subject-domains-and-domain-knowledge.md` — problem-space domain knowledge layer
- `b1-business-dynamics/104-outcomes-and-hypothesis-validation.md` — value/outcome verdict layer
- `b8-change-execution/810-retrospectives-and-process-effectiveness.md` — process-effectiveness retrospective layer
- `b9-knowledge-traceability/907-ingestion-bindings-and-extraction-recipes.md` — ingestion binding layer
- `migrations/migration.aismm_v2_to_v3.json` — migration map

### Changed

- `aismm-feedback-loops.md` — added loop 7 (Outcome/Retrospective), split into value branch (7a) and process branch (7b)
- `aismm-layer-inventory.md` — 82 → 86 layers; added the four v3 layers; documented `00-policies` and the standards tree
- `aismm-layer-artifact-naming.md` — new record kinds (`outcome`, `retro`, `binding`, `ingest`, `recipe`, `method`, `standard`, `validation`, `attestation`); layer mappings for 007/104/810/907
- `aismm-consistency-rules.md` — source-of-truth rows for new layers + `00-policies`; problem-vs-solution, outcome-vs-process, normative-vs-descriptive boundary notes
- `aismm-versioning-and-conformance.md` — version 3.0.0; MAJOR rationale
- `README.md` — v3 additions section; new layers in bundle lists; version bump
- `b9.906` (ontology) — relationship taxonomy to be extended with v3 typed relationships *(spec note in change spec; apply to file)*

### Migration notes

- `kind_class` becomes expected on records (defaulted by record kind during migration).
- Cross-cutting product policies move from `aismm/` data layers into `00-policies`.
- Derived records gain a `derivation` block and an owner-validation gate before reaching `stable`.
- Owner-validation gate is part of conformance L5; ingestion bindings part of L4/L5.

---

## AISMM 2.0.2 — Model Registry, Empty Layer Standard and Product Identity

### Added

- `aismm-model-registry.md` — full model registry specification: source discovery, source types (8), source status values (8), completeness status values (5), 14-step registry-first parsing pipeline, distributed multi-repo example, validation artifact extension
- `aismm-empty-layer-standard.md` — empty layer block standard: completion status values (6 canonical), required metadata fields including `model_instance_id` and `product_id`, canonical empty layer block format, missing vs empty distinction, strict mode integration
- `examples/aismm.registry.example.json` — complete registry example with b0–b12 bundle coverage, restricted source, module source, empty declared layers, and completeness status
- `examples/empty-layer.example.md` — copy-paste empty layer template with `completion_status: empty`, product/model identity fields, known gaps, and expected structure

### Changed

**aismm-structure.md:**
- Section 2.2 canonical block form now includes `model_instance_id`, `product_id`, `product_key`, `completion_status`
- Section 2.3 metadata example updated to v2 canonical form
- Section 3 field table updated with new required fields
- New sections 27, 27a, 27b: Model Registry, Product Identity, Empty vs Missing Layers
- Sections renumbered accordingly

**aismm-unified-id-strategy.md:**
- New section 17: Product and Model Instance Identity — product-local IDs, cross-product qualified references, required block fields, validation rules
- Section 16 renamed to 18 Summary

**aismm-strict-mode.md:**
- New rule group: Product and Model Instance Identity Rules (RULE-PID-001 to RULE-PID-006)
- New rule group: Registry and Completeness Rules (RULE-REG-001 to RULE-REG-005)
- New rule group: Empty and Missing Layer Rules (RULE-LAYER-001 to RULE-LAYER-005)
- Validation artifact example extended with `model_instance_id`, `product_id`, `model_completeness_status`, `loaded_sources`, `missing_sources`, `restricted_sources`, `declared_empty_layers`

**aismm-consistency-checks.md:**
- New check category 3: Registry and Completeness Checks
- New check category 4: Product Identity Checks
- New pseudo-code: `check_registry_and_completeness()`, `check_expected_layers_covered()`, `check_product_identity()`

**README.md:**
- New sections: Distributed AISMM and Model Registry, Product and Model Instance Identity, Empty Layers vs Missing Layers

### Compatibility

Fully additive. All existing v2.0.0 and v2.0.1 documents remain valid. New fields (`product_id`, `model_instance_id`, `completion_status`) are additive — existing blocks without them produce a validation warning, not an error, until the team migrates.

---

## AISMM 2.0.1 — Consistency and Governance Fixes

### Added

- `b12-finops-and-technical-economics/README.md` — complete bundle README with layer overview, b0 vs b12 boundary statement, cross-bundle flow, and schema reference
- `aismm-consistency-rules.md` — source-of-truth map for all concepts across b0–b12, projection rules, duplication rules, conflict resolution, and lifecycle flow rules
- `aismm-consistency-checks.md` — repeatable consistency validation checks with pseudo-code, CI integration guidance, and layer inventory maintenance instructions
- `aismm-layer-inventory.md` — complete inventory of all 82 layers across b0–b12 with README/schema coverage status

### Changed

**4-digit Layer ID support (b10+):**
- `aismm-unified-id-strategy.md` — updated layer ID format to support 3+ digits; regex changed from `^[0-9]{3}$` to `^[0-9]{3,}$`; added b10/b11/b12 ID examples; added warning against 3-digit truncation
- `aismm-strict-mode.md` — RULE-ID-002 now explicitly describes 4-digit layer IDs for b10+ bundles
- `aismm-structure.md` — section 6 updated to describe both 3-digit and 4-digit layer ID formats

**Schemas updated for new v2 layers:**
- `b2.schema.json` — added layers 205, 206, 207
- `b3.schema.json` — added layers 304, 305
- `b4.schema.json` — added layers 405, 406, 407
- `b5.schema.json` — added layers 504, 505
- `b6.schema.json` — added layers 605, 606, 607
- `b7.schema.json` — added layers 705, 706, 707
- `b8.schema.json` — added layers 807, 808, 809
- `b9.schema.json` — added layers 905, 906

**Lifecycle boundary clarifications:**
- `b8-change-execution/readme.md` — explicitly states b8 is the lifecycle orchestration layer; added 10-step closed-loop example
- `b8-change-execution/807-feedback-loops-and-learning-cycles.md` — strengthened boundaries with risk/incident/feedback lifecycle roles
- `b6-runtime-operations/603-incident-management-and-response.md` — added lifecycle boundary: b6 owns incidents, b8 owns learning cycles
- `b7-quality-risk-compliance/702-risk-management.md` — added lifecycle boundary: b7 owns risk definitions, b6 records realization, b8 owns feedback
- `b6-runtime-operations/602-observability-and-monitoring.md` — clarified b6 is post-release runtime validation
- `b7-quality-risk-compliance/701-quality-assurance-and-testing.md` — clarified b7 defines test strategy; b8 records execution
- `b8-change-execution/803-testing-and-acceptance-execution.md` — clarified b8 records test execution and acceptance decisions

**Data responsibility boundary clarifications:**
- `b2-system-design/readme.md` — added data boundary: b2=WHAT, b3=HOW, b10=lifecycle
- `b3-implementation/readme.md` — added data boundary with same rule
- `b10-data-and-ai-lifecycle/README.md` — added data boundary statement

**Ownership source-of-truth clarifications:**
- `b11-organization-ownership-governance/README.md` — states b11 is source of truth for ownership; b9 is projection
- `b9-knowledge-traceability/readme.md` — states b9 ownership edges are projections; b11 wins in conflicts

**Economics bridge clarifications:**
- `b0-product-core/readme.md` — added b0 vs b12 economics boundary statement and cross-layer flow
- `b0-product-core/006-economics-model.md` — added reference to b12 technical economics inputs

**Documentation structure:**
- `aismm-structure.md` — added section 28 (Consistency Rules and Source of Truth); renumbered sections to 29–31
- `README.md` — added Consistency and Source of Truth, Layer Inventory, Temporal Validity, Physical Identity, and Feedback Loops sections with all required document links

### Compatibility

All changes are additive or clarifying. No entities removed. No schemas broken. Layer inventory confirms 82/82 layers are consistent across README/schema/file.

---

## AISMM 2.0.0

### Added

#### Root Governance Documents

- `aismm-versioning-and-conformance.md` — Semantic versioning model, layer lifecycle statuses, migration policy, extension namespace, and five conformance levels (L1–L5)
- `aismm-strict-mode.md` — Validation levels, mandatory rules, `aismm.validation.json` artifact, and CI integration
- `aismm-semantic-diff.md` — Semantic change types, `aismm.semantic_diff.json` artifact, and PR review rules
- `aismm-physical-identity-binding.md` — Logical-to-physical traceability via symbol, artifact, commit, schema, runtime, and digest bindings
- `aismm-temporal-validity.md` — `valid_from`, `valid_to`, `effective_from_release`, `superseded_by`, and temporal query patterns
- `aismm-feedback-loops.md` — Six canonical feedback loop types connecting operational signals to product change
- `aismm-context-security.md` — Block authorship classes, context access policies, attestation, and prompt injection defense
- `aismm-agent-contract-references.md` — Bridge to external agent contract repositories with clear boundary statement
- `aismm-health.md` — `aismm.health.json` artifact, health check categories, and CI health gate

#### Bundle 10 — Data and AI Lifecycle (new)

- `b10-data-and-ai-lifecycle/README.md`
- `1001-data-products-and-contracts.md`
- `1002-data-lineage-and-schema-evolution.md`
- `1003-feature-and-embedding-lifecycle.md`
- `1004-model-registry-and-versioning.md`
- `1005-training-evaluation-and-experimentation.md`
- `1006-drift-monitoring-and-retraining.md`
- `1007-labeling-annotation-and-ground-truth.md`
- `1008-data-quality-and-observability.md`
- `b10.schema.json`

#### Bundle 11 — Organization, Ownership and Governance (new)

- `b11-organization-ownership-governance/README.md`
- `1101-team-topology-and-raci.md`
- `1102-ownership-graph.md`
- `1103-decision-rights-and-governance.md`
- `1104-skills-and-capability-matrix.md`
- `1105-vendors-and-external-responsibilities.md`
- `1106-capacity-and-allocation.md`
- `b11.schema.json`

#### Bundle 12 — FinOps and Technical Economics (new)

- `b12-finops-and-technical-economics/README.md`
- `1201-cost-allocation-and-showback.md`
- `1202-capacity-and-commitment-management.md`
- `1203-token-and-inference-economics.md`
- `1204-cost-of-quality-and-incident.md`
- `1205-unit-economics-by-service-tenant-request.md`
- `1206-carbon-and-sustainability-accounting.md`
- `b12.schema.json`

#### New Layers in Existing Bundles

**Bundle 8 — Change Execution:**
- `807-feedback-loops-and-learning-cycles.md` — Explicit feedback loop and learning cycle modeling
- `808-ci-cd-pipeline-and-automation.md` — CI/CD pipeline, stages, gates, and promotion rules as SDLC entities
- `809-migration-backfill-and-long-running-refactors.md` — Migration plans, compatibility windows, cutover plans

**Bundle 9 — Knowledge Traceability:**
- `906-ontology-vocabulary-and-relationship-taxonomy.md` — 23 canonical relationship types, edge semantics, and shared vocabulary

**Bundle 2 — System Design:**
- `205-external-systems-and-ecosystem-surface.md`
- `206-event-catalog-and-event-mesh.md`
- `207-bounded-contexts-and-domain-architecture.md`

**Bundle 3 — Implementation:**
- `304-dependency-inventory-sbom-and-reproducibility.md`
- `305-configuration-feature-flags-and-environment-variants.md`

**Bundle 4 — Product Behavior:**
- `405-state-machines-and-lifecycles.md`
- `406-behavioral-contracts-and-invariants.md`
- `407-error-taxonomy-and-failure-behavior.md`

**Bundle 5 — User Interaction:**
- `504-design-system-and-ui-components.md`
- `505-accessibility-localization-and-notifications.md`

**Bundle 6 — Runtime Operations:**
- `605-capacity-scaling-and-performance-engineering.md`
- `606-disaster-recovery-backup-and-continuity.md`
- `607-operational-readiness-and-drills.md`

**Bundle 7 — Quality, Risk and Compliance:**
- `705-threat-modeling-and-attack-surface.md`
- `706-vulnerability-management-and-disclosure.md`
- `707-privacy-rights-and-dpia.md`

#### Migration Records

- `migrations/migration.aismm_v1_to_v2.versioning_and_conformance.json`

### Changed

- `README.md` — Updated for AISMM v2 with full bundle overview (b0–b12), new governance sections, agent contract boundary, strict mode, health, RAG, and conformance levels
- `aismm-structure.md` — Updated with all new bundles, layers, conformance levels, temporal validity, physical identity binding, semantic diff, strict mode, and health model references

### Compatibility Notes

AISMM v2 is **fully additive** relative to v1. All existing v1 documents remain valid.

New fields and entities introduced in v2 are optional at conformance levels L1 and L2. They become progressively required at L3 (typed relationships), L4 (RAG metadata), and L5 (agent governance).

### Migration Notes from AISMM v1 to v2

1. **Declare AISMM version** — Add `aismm_version: 2.0.0` to your root README or `aismm-manifest.yaml`.

2. **Add layer lifecycle status** — Add a `status` field to each layer specification using the canonical lifecycle values.

3. **Declare conformance level** — Add a `conformance_level: L1|L2|L3|L4|L5` declaration.

4. **Add relationship types** — At L3+, ensure all trace links in b9.902 declare a `relation_type` from the canonical taxonomy in layer 906.

5. **Add summaries for RAG** — At L4+, ensure all significant blocks include a `summary` field.

6. **Review new bundles** — Evaluate whether b10, b11, or b12 are relevant to your product and add them if needed.

None of these steps break existing documents. All are additive.
