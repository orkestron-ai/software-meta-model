# AISMM Ingestion and Source Binding

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.ingestion-and-source-binding
  spec_version: 3.0.0
  status: accepted
  created: "2026-05-23"
  updated: "2026-05-23"
```

---

## Purpose

V3 makes **agent-driven population of layer records from owning systems** a first-class, governed pipeline. This is the governance companion to layer `b9.907`.

It answers the analyst conclusion *"data in each layer's folders should be populated by agents via MCP from the systems that own that data — do we need a layer with prompts, or where do we put this?"*

Answer: **split it.** The recipe/prompt is normative (a method) → `00-policies`. The binding and run provenance are descriptive → `b9.907` + `b9.903`.

---

## 1. The Ingestion Pipeline

```text
owning system (b2.205 / b2.204)
  → connector (MCP server / tool / API)        [connector_reference]
  → extraction recipe (prompt + mapping)        [00-policies, normative, versioned]
  → agent run                                   [ingestion_run, provenance]
  → target layer records (kind_class: descriptive, derivation.*)
  → owner validation gate                       [aismm-owner-validation-and-attestation.md]
```

Each arrow is an explicit, referenceable object. Nothing in the pipeline is implicit.

---

## 2. Separation of Concerns

| Concern | Class | Home |
|---------|-------|------|
| Which system owns the data | descriptive | `b2.205` / `b2.204` |
| How we connect (MCP/tool) | reference | `connector_reference` in `b9.907` |
| What/how to extract (the prompt + mapping) | **normative** | `00-policies` recipe record |
| The binding tying it together | descriptive | `b9.907` ingestion_binding |
| Each actual run | provenance | `b9.907` ingestion_run + `b9.903` |
| Whether the result is trusted | governance | owner-validation gate |

The recipe is deliberately **not** inlined in the binding: recipes are reusable across products and bindings, are versioned, and are evaluated like other normative methods.

---

## 3. Connector References (MCP)

AISMM does not define connectors or agent contracts (see `aismm-agent-contract-references.md`). It **references** them:

```yaml
connector_reference:
  id: connector_reference.gitlab_mcp
  kind: mcp_server          # mcp_server | mcp_tool | api | export
  endpoint_ref: "mcp:gitlab"        # stable identifier, no secrets
  auth: redacted                    # secrets are redacted per context-security
  owning_system: b2.205.external_system.gitlab
```

Secrets are never stored; secret references are redacted in any context package (`aismm-context-security.md`).

---

## 4. Extraction Recipe (Normative, in `00-policies`)

```yaml
recipe:
  id: recipe.requirements_from_gitlab_issues
  kind_class: normative
  version: 1.0.0
  target_layer: b4.401
  emits_record_kind: req
  source: gitlab issues with label "requirement"
  prompt: |
    Read each GitLab issue labeled "requirement". For each, emit one b4.401
    requirement record. Preserve the issue ID as a source reference. Do not
    invent acceptance criteria; if absent, record an explicit gap.
  field_mapping:
    - source: issue.title        -> record.title
    - source: issue.description  -> record.statement
    - source: issue.iid          -> record.source_ref
  derivation_type: extracted     # extracted | summarized | inferred | synthesized
```

`derivation_type` matters for trust: `extracted` is high-fidelity; `inferred`/`synthesized` require stronger validation (see owner-validation gate).

---

## 5. Ingestion Run (Provenance)

```yaml
ingestion_run:
  id: ingestion_run.req_gitlab.2026-05-23T10-00
  binding: ingestion_binding.requirements_gitlab
  recipe_version: 1.0.0
  connector: connector_reference.gitlab_mcp
  agent_identity: agent:requirements_ingestor:2.1.0
  ran_at: "2026-05-23T10:00:00Z"
  source_snapshot_digest: sha256:...
  produced_record_ids: [b4.401.requirement.live_event_scheduling]
  resulting_validation_status: unvalidated
```

Every derived record links back to its `ingestion_run` via `derivation.derived_from`, giving a complete audit chain: record → run → recipe version → connector → owning system.

---

## 6. Triggers

| Trigger | Use |
|---------|-----|
| `manual` | one-off / on-demand ingestion |
| `scheduled` | periodic refresh (cron) |
| `event` | webhook/event from the owning system |

Scheduled and event triggers SHOULD be paired with the scheduled-task / automation surfaces, not hard-coded.

---

## 7. Validation Rules

| Rule | Requirement |
|------|-------------|
| Binding required | Any layer populated by agents must have an ingestion binding in `b9.907` |
| Recipe is normative | Recipes live in `00-policies`, are versioned, and are referenced (not inlined) |
| Provenance mandatory | Every ingestion run produces a provenance record; derived records carry `derivation.*` |
| No secrets in model | Connector secrets are redacted; only stable references are stored |
| Validation routing | Each binding names `owner_of_source`, enabling the owner-validation gate |
| Deterministic mapping | Field mappings are explicit to support semantic diff on re-ingestion |

---

## Summary

V3 turns "an agent pulled this from somewhere" into a governed pipeline: owning system → MCP connector → versioned recipe (normative, in `00-policies`) → run provenance → target records → owner validation. The binding lives in `b9.907`; the recipe lives in `00-policies`; the run lives in provenance. This is the precise home for the analyst's "layer with prompts."
