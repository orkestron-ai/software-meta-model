# AISMM Owner Validation and Attestation

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.owner-validation-and-attestation
  spec_version: 3.0.0
  status: accepted
  created: "2026-05-23"
  updated: "2026-05-23"
```

---

## Purpose

When an agent ingests source data and produces a model record, the record is **new derived data that speaks on behalf of the source**. V3 requires that such derived records be **validated by the owner of the source data** before they are treated as authoritative.

This answers the analyst conclusion *"data entering the meta-model must be validated by its owners, because it is processed and new data is born on behalf of the original."*

> What V2 already had, and why it was not enough: V2 has `authorship_class` and an optional `attestation` block (`aismm-context-security.md`). But that tracks **who wrote** the block, not **whether the source owner endorses** the derived content, and it is **not gated** into the lifecycle. V3 makes owner validation a **mandatory, source-owner-keyed gate**.

---

## 1. Two Ownership Roles

V3 distinguishes:

| Role | Meaning | Source |
|------|---------|--------|
| `owner_of_source` | owns the original data in the owning system; the authority that must validate derived records | `b11.1102` + `b9.907` binding |
| `owner_of_record` | maintains the AISMM record (often an agent or the model team) | `b11.1102` |

Validation = the **`owner_of_source`** attesting that the derived record faithfully represents their data. The `owner_of_record` cannot self-validate derived content.

---

## 2. Derivation Block

Every derived record carries a `derivation` block:

```yaml
derivation:
  derived_from:
    - ingestion_run.req_gitlab.2026-05-23T10-00   # b9.907
    - source:gitlab/issue/4821                     # original source
  produced_by: agent:requirements_ingestor:2.1.0
  derivation_type: extracted        # extracted | summarized | inferred | synthesized
  transform_summary: "1:1 extraction of issue title/body into requirement"
```

`derivation_type` sets the validation bar:

| Type | Fidelity | Default requirement |
|------|----------|---------------------|
| `extracted` | high (verbatim/structured copy) | owner validation or `auto_validated` policy |
| `summarized` | medium | owner validation |
| `inferred` | low (agent reasoning over source) | owner validation, stronger evidence |
| `synthesized` | low (combined/created) | owner validation, explicit review |

---

## 3. Validation Lifecycle

```yaml
validation_status: unvalidated      # see states below
validated_by: null
validated_at: null
validation_scope: null
revalidate_by: null
```

States:

```text
unvalidated
  → owner_review_requested
    → owner_validated      (owner endorses; record may become authoritative)
    → owner_disputed       (owner rejects or corrects; record cannot be authoritative)
auto_validated             (policy allows automatic validation for low-risk extracted data)
expired                    (validation aged out; needs re-validation)
```

```yaml
# a validated record
validation_status: owner_validated
validated_by: b11.1102.owner.streaming_platform_team
validated_at: "2026-05-23T12:30:00Z"
validation_scope: "faithful representation of GitLab issue #4821 as of snapshot digest"
revalidate_by: "2026-11-23"     # ties to temporal validity + confidence decay
```

---

## 4. The Gate

This is the load-bearing rule of V3:

> A `descriptive` record produced by derivation MUST NOT reach `status: stable`, MUST NOT enter an authoritative agent context package, and MUST NOT be cited as a source of truth until its `validation_status` is `owner_validated` or `auto_validated`.

Enforcement points:

| Surface | Enforcement |
|---------|-------------|
| `aismm-context-security.md` | `context_access_policy` denies records whose `validation_status` is `unvalidated`/`owner_disputed`/`expired` from authoritative packages |
| `aismm-strict-mode.md` | strict mode fails a record promoted to `stable` without owner validation |
| Conformance L5 | owner-validation gate is a requirement of `Agent-Grade Governance` |
| `b9.905` retrieval | unvalidated records may be retrieved only with an explicit `unvalidated` flag, never as authoritative |

---

## 5. Auto-Validation Policy

Not everything needs a human. A normative policy in `00-policies` may declare auto-validation for low-risk cases:

```yaml
auto_validation_policy:
  id: policy.auto_validation.extracted_low_risk
  kind_class: normative
  applies_when:
    derivation_type: extracted
    criticality: low
    field_mapping: deterministic
  result: auto_validated
  audit: ingestion_run reference retained
```

`inferred` and `synthesized` derivations are never auto-validated.

---

## 6. Attestation Record

Owner validation produces an attestation (extends the V2 `attestation` format):

```yaml
attestation:
  id: attestation.b4.401.requirement.live_event_scheduling
  block_id: b4.401.requirement.live_event_scheduling
  attested_by: b11.1102.owner.streaming_platform_team
  attested_role: owner_of_source
  attested_at: "2026-05-23T12:30:00Z"
  method: human_review        # human_review | digital_signature | automated_verification
  scope: "faithful to source snapshot sha256:..."
  signature: optional_hash
```

---

## 7. Routing Validation to the Owner

The owner is found via the `b9.907` ingestion binding (`owner_of_source`) → `b11.1102` ownership. Validation requests MAY be routed back to the owning system through the same connector (e.g. an MCP action that opens a review task), closing the human loop where the data actually lives.

---

## 8. Relationship Types (added to `b9.906`)

```text
derived_from        record → source/ingestion_run
attested_by         record → owner attestation
validates           owner → record
disputes            owner → record
```

---

## 9. Validation Rules

| Rule | Requirement |
|------|-------------|
| Derivation declared | Every derived record carries a `derivation` block |
| Owner-keyed | Validation is performed by `owner_of_source`, not `owner_of_record` |
| Gate enforced | No authoritative use before `owner_validated`/`auto_validated` |
| No self-validation | An agent/owner-of-record cannot validate its own derived output |
| Re-validation | `revalidate_by` ages validation out; expired records leave authoritative context |
| Disputes block | `owner_disputed` records are quarantined and surfaced in health checks |

---

## Summary

V3 introduces a mandatory, source-owner-keyed validation gate for derived data. Each derived record declares how it was produced (`derivation`), and cannot be trusted as authoritative until the owner of the source data attests to it (or an explicit auto-validation policy applies). This closes the integrity gap created when agents "birth" new records on behalf of source systems.
