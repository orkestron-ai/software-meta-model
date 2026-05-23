<!-- AISMM:BEGIN -->
type: global_specification
document_id: spec.global.layer.artifact.naming
document_type: global_specification
module_scope: root
status: accepted
spec_version: 1.0.0
title: AISMM Layer Record Naming
<!-- AISMM:META_END -->

# AISMM Layer Record Naming

## 1. Purpose

This document defines the canonical naming and identity rules for model record files stored inside product AISMM layer directories.

## 2. Scope

These rules apply to product repositories that keep their working model under `aismm/`.

They do not rename the canonical specification files inside `00-meta/software-meta-model-main`. Those files remain the canon.

The file path of this specification retains the older `artifact` term for compatibility.

In user-facing AISMM language, files inside product layer directories are `record files`.

The term `artifact` remains reserved for physical or generated engineering outputs such as build artifacts, release packages, migration files, logs, exported reports, and similar concrete outputs.

## 3. Canonical Product Layout

Every product AISMM repository must contain:

```text
repo/
  00-meta/
    software-meta-model-main/
    software-meta-model-template-main/
  aismm.registry.json
  aismm/
```

Inside `aismm/`, each bundle contains layer directories, and each layer directory contains record files.

## 4. Core Rules

1. Each layer in the product model is a directory.
2. Each new task, change, addition, research item, risk, procedure, requirement refinement, decision or similar unit of work creates a new record file.
3. The record file ID is immutable after creation.
4. The same record file ID must never be reused for a different semantic unit.
5. Record filenames must remain human-readable and sortable.

## 4A. Granularity Rule

Record files must follow a 1:1 granularity rule:

1. One minimal initiative, task, change request, work item, approval, release candidate, feedback loop, or similar semantic unit must live in one record file.
2. A product repository must not aggregate multiple independent initiatives into one shared record file just because they belong to the same layer.
3. If initiatives are related, the relationship must be expressed through links and `Related Objects`, not by collapsing them into one file.
4. A summary or index record may link multiple initiative files, but it must not become the source-of-truth container for those initiatives.
5. If a file contains several independently routable initiatives, it must be split.

Supporting prose, evidence, and `Related Objects` are allowed around the primary semantic block.

The rule is about one primary semantic unit per record file, not about banning explanatory text.

## 4B. Reference Link Rule

When a markdown section, table, or list points to a workspace record or source file, it should use a resolvable relative markdown link.

When the reference lives inside a fenced YAML or other code block, store a plain workspace-relative file reference or stable source identifier there instead of markdown-link syntax.

If clickable links are needed for the same references, place them in an adjacent markdown section such as `Product Sources`, `Evidence`, or `Related Objects`.

Examples:

- `Related Objects`
- `Product Sources`
- `Evidence`
- structured fields represented outside fenced code blocks

## 5. Canonical Filename Pattern

```text
{record_kind}-{record_file_id}-{memo}.md
```

Where:

- `record_kind` is the canonical short name for the record type in the layer
- `record_file_id` uses the format `YYMMDDNNNNN`
- `memo` is a short lowercase kebab-case mnemonic

Example:

```text
req-26051900001-user-login.md
risk-26051900002-payment-timeout.md
change-26051900003-blue-green-cutover.md
```

## 6. Record File ID Semantics

```text
YYMMDDNNNNN
```

- `YYMMDD` = creation date of the record file
- `NNNNN` = zero-padded sequence number for files created in the same layer on the same day

Example:

```text
26051900001
```

means: first record created on 2026-05-19 in the given layer.

## 7. Memo Rules

The memo part of the filename:

- must be lowercase
- must use latin letters, digits and hyphens
- should be short and semantically specific
- must not duplicate the artifact kind or date unless needed for clarity

Good:

```text
user-login
pricing-policy
release-cutover
```

Bad:

```text
new-file
temp
final-version
260519-fix
```

## 8. Canonical Artifact Vocabulary

| Artifact Kind | Meaning |
|---|---|
| `access` | Access right, role, permission, entitlement record |
| `accessibility` | Accessibility record |
| `alert` | Alerting rule or alert record |
| `api` | API-level record or contract note |
| `acceptance` | Acceptance criterion or acceptance record |
| `actor` | Actor or participant record |
| `allocation` | Allocation record |
| `annotation` | Annotation record |
| `artifact` | Build or deployable artifact record |
| `audit` | Compliance or audit artifact |
| `boundary` | Boundary or boundary-condition record |
| `backfill` | Backfill execution or plan |
| `budget` | Budget or resource budget record |
| `case` | Individual case record |
| `capability` | Capability record |
| `capacity` | Capacity planning or allocation record |
| `carbon` | Sustainability or emissions record |
| `change` | Change request, change scope, or modification unit |
| `check` | Check or verification record |
| `code` | Code-level record |
| `component` | Component or structural element |
| `commitment` | Commitment record |
| `compaction` | History compaction record |
| `config` | Configuration or feature-flag record |
| `confidence` | Confidence-scoring record |
| `constraint` | Constraint or boundary |
| `context` | Product, system, or domain context record |
| `control` | Security, risk, or governance control |
| `coverage` | Coverage or completeness record |
| `contract` | Contract record |
| `cost` | Cost record |
| `data` | Data asset or data behavior record |
| `dataset` | Dataset record |
| `decision` | Decision record or ADR-style entry |
| `dep` | Dependency record |
| `dependency` | Dependency relationship or dependency record |
| `deploy` | Deployment record |
| `dq` | Data quality record |
| `drift` | Drift or model degradation record |
| `dpia` | DPIA or privacy-impact record |
| `domain` | Domain-modeling record |
| `drill` | Drill or exercise record |
| `edge` | Trace edge record |
| `embedding` | Embedding record |
| `entity` | Entity definition record |
| `env` | Environment or topology variant |
| `error` | Error taxonomy or failure behavior record |
| `evidence` | Evidence item |
| `event` | Event, topic, or message record |
| `evolution` | Evolution record |
| `experiment` | Experiment or evaluation record |
| `external` | External system or external dependency record |
| `feedback` | Feedback or learning-cycle record |
| `feature` | Feature record |
| `failure` | Failure-mode record |
| `finding` | Finding record |
| `flag` | Feature-flag record |
| `flow` | Process or flow record |
| `form` | Form or structured UI form record |
| `gap` | Gap or deficiency record |
| `gov` | Governance record |
| `goal` | Goal or objective record |
| `history` | History record |
| `hypo` | Hypothesis record |
| `incident` | Incident or postmortem record |
| `index` | Index or navigation record |
| `automation` | Automation record |
| `abusecase` | Abuse-case record |
| `integration` | Integration record |
| `interface` | Interface record |
| `invariant` | Invariant or rule that must always hold |
| `journey` | User journey record |
| `job` | Job or automation step record |
| `label` | Labeling or annotation record |
| `lesson` | Lesson learned record |
| `lifecycle` | Lifecycle record |
| `lineage` | Lineage record |
| `link` | Link record |
| `load` | Load-test or load-profile record |
| `locale` | Localization record |
| `map` | Mapping record |
| `metric` | Metric or measurement definition |
| `milestone` | Milestone record |
| `mitigation` | Mitigation record |
| `migration` | Migration or long-running refactor record |
| `model` | AI/ML model record |
| `module` | Code module or package record |
| `monitor` | Monitoring or observability record |
| `motivation` | Motivation record |
| `nav` | Navigation record |
| `nfr` | Non-functional requirement record |
| `node` | Node or runtime-node record |
| `notify` | Notification record |
| `ontology` | Ontology or taxonomy record |
| `owner` | Ownership record |
| `package` | Package record |
| `perf` | Performance record |
| `plan` | Plan, milestone, or schedule record |
| `policy` | Policy or governing rule |
| `postmortem` | Postmortem record |
| `privacy` | Privacy record |
| `proc` | Procedure or operational process |
| `rag` | Retrieval or RAG-oriented record |
| `raci` | RACI record |
| `recovery` | Recovery or continuity record |
| `refactor` | Refactoring record |
| `release` | Release or rollout record |
| `readiness` | Readiness record |
| `req` | Requirement record |
| `research` | Research, exploration, or investigation record |
| `request` | Request record |
| `responsibility` | Responsibility record |
| `remediation` | Remediation record |
| `result` | Execution or acceptance result |
| `risk` | Risk record |
| `role` | Role record |
| `rule` | Business or domain rule |
| `runbook` | Runbook or operational playbook |
| `route` | Route record |
| `scenario` | User scenario or usage path |
| `schema` | Schema or schema evolution record |
| `screen` | Screen or UI surface record |
| `security` | Security control or security context record |
| `service` | Service record |
| `showback` | Showback record |
| `skill` | Skill or capability record |
| `sla` | SLA/SLO/operational objective record |
| `source` | Provenance or source record |
| `sprint` | Sprint record |
| `sbom` | SBOM record |
| `scope` | Scope record |
| `story` | Story record |
| `strategy` | Strategy record |
| `structure` | Structural record |
| `state` | State machine or lifecycle record |
| `stakeholder` | Stakeholder record |
| `surface` | Surface or exposure record |
| `summary` | Summary record |
| `suite` | Test suite record |
| `task` | Work task record |
| `taxonomy` | Taxonomy record |
| `team` | Team topology or team record |
| `tech` | Technology record |
| `term` | Glossary or terminology record |
| `tenant` | Tenant record |
| `test` | Test case or test definition |
| `threat` | Threat or abuse-case record |
| `topology` | Runtime or system topology record |
| `trace` | Traceability edge or relation record |
| `transition` | Transition record |
| `unitcost` | Cost-per-unit or per-request record |
| `ui` | UI record |
| `value` | Value-stream or economics value record |
| `vendor` | Vendor or external responsibility record |
| `version` | Version record |
| `vuln` | Vulnerability record |
| `workitem` | Work-item record |
| `backup` | Backup record |
| `execution` | Execution record |
| `improvement` | Improvement record |
| `inference` | Inference-economics or inference record |
| `pipeline` | Pipeline record |
| `product` | Product record |
| `quality` | Quality record |
| `registry` | Registry record |
| `retention` | Retention record |
| `retrain` | Retraining record |
| `retrieval` | Retrieval record |
| `token` | Token-economics record |
| `sustainability` | Sustainability record |
| `domain` | Subject-domain knowledge record *(also used by b0.007)* |
| `outcome` | Outcome / hypothesis-validation record *(v3, b1.104)* |
| `retro` | Retrospective / process-effectiveness record *(v3, b8.810)* |
| `binding` | Ingestion binding record *(v3, b9.907)* |
| `ingest` | Ingestion run / ingestion record *(v3, b9.907)* |
| `recipe` | Extraction recipe / prompt record *(v3, normative, 00-policies)* |
| `method` | Methodology / ways-of-working record *(v3, normative, 00-policies)* |
| `standard` | Baseline or adopted standard record *(v3, normative)* |
| `validation` | Owner-validation record *(v3)* |
| `attestation` | Attestation record *(v3)* |

## 9. Layer-Oriented Default Mapping

| Layer | Preferred Artifact Kinds |
|---|---|
| `001` Product Definition Context | `context`, `scope`, `term` |
| `002` Value Streams | `value`, `flow`, `actor` |
| `003` Stakeholders and Motivation | `stakeholder`, `goal`, `motivation` |
| `004` Business Architecture | `context`, `capability`, `constraint` |
| `005` Critical Path | `flow`, `dependency`, `constraint` |
| `006` Economics Model | `value`, `budget`, `unitcost` |
| `007` Subject Domains and Domain Knowledge | `domain`, `term`, `proc` |
| `101` Business Hypotheses | `hypo`, `experiment`, `decision` |
| `102` Strategy and Product Management | `strategy`, `plan`, `decision` |
| `103` Processes | `proc`, `flow`, `policy` |
| `104` Outcomes and Hypothesis Validation | `outcome`, `result`, `decision` |
| `201` Applications and System Architecture | `component`, `context`, `boundary` |
| `202` Data and Information Architecture | `data`, `schema`, `entity` |
| `203` API and Interfaces | `api`, `contract`, `interface` |
| `204` Integrations | `integration`, `external`, `dependency` |
| `205` External Systems and Ecosystem Surface | `external`, `dependency`, `context` |
| `206` Event Catalog and Event Mesh | `event`, `schema`, `flow` |
| `207` Bounded Contexts and Domain Architecture | `context`, `domain`, `component` |
| `301` Technology Architecture | `tech`, `decision`, `constraint` |
| `302` Code and Implementation | `module`, `component`, `code` |
| `303` Build Deployment and Runtime Artifacts | `artifact`, `deploy`, `version` |
| `304` Dependency Inventory SBOM and Reproducibility | `dep`, `sbom`, `artifact` |
| `305` Configuration Feature Flags and Environment Variants | `config`, `flag`, `env` |
| `401` Requirements | `req`, `story`, `acceptance` |
| `402` Domain Model and Business Rules | `rule`, `entity`, `invariant` |
| `403` Access Rights | `access`, `policy`, `role` |
| `404` NFR and Quality Attributes | `nfr`, `constraint`, `sla` |
| `405` State Machines and Lifecycles | `state`, `lifecycle`, `transition` |
| `406` Behavioral Contracts and Invariants | `contract`, `invariant`, `rule` |
| `407` Error Taxonomy and Failure Behavior | `error`, `failure`, `rule` |
| `501` User Scenarios and UX Logic | `scenario`, `journey`, `flow` |
| `502` Interface Structure and Navigation | `nav`, `route`, `structure` |
| `503` Screens Forms and UI States | `screen`, `form`, `state` |
| `504` Design System and UI Components | `ui`, `component`, `token` |
| `505` Accessibility Localization and Notifications | `access`, `locale`, `notify` |
| `601` Runtime Environment and Topology | `topology`, `env`, `node` |
| `602` Observability and Monitoring | `monitor`, `metric`, `alert` |
| `603` Incident Management and Response | `incident`, `runbook`, `postmortem` |
| `604` SLA SLO and Operational Governance | `sla`, `policy`, `gov` |
| `605` Capacity Scaling and Performance Engineering | `capacity`, `perf`, `load` |
| `606` Disaster Recovery Backup and Continuity | `recovery`, `backup`, `policy` |
| `607` Operational Readiness and Drills | `readiness`, `drill`, `runbook` |
| `701` Quality Assurance and Testing | `test`, `suite`, `case` |
| `702` Risk Management | `risk`, `control`, `mitigation` |
| `703` Security and Privacy | `security`, `privacy`, `control` |
| `704` Compliance and Audit | `audit`, `evidence`, `policy` |
| `705` Threat Modeling and Attack Surface | `threat`, `abusecase`, `surface` |
| `706` Vulnerability Management and Disclosure | `vuln`, `finding`, `remediation` |
| `707` Privacy Rights and DPIA | `privacy`, `dpia`, `request` |
| `801` Work Items and Change Requests | `task`, `change`, `workitem` |
| `802` Planning and Delivery Flow | `plan`, `sprint`, `milestone` |
| `803` Testing and Acceptance Execution | `execution`, `result`, `acceptance` |
| `804` Release Version and Rollout Management | `release`, `rollout`, `version` |
| `805` Change History and Decision Log | `decision`, `change`, `history` |
| `806` Knowledge Retention and History Compaction | `retention`, `summary`, `compaction` |
| `807` Feedback Loops and Learning Cycles | `feedback`, `lesson`, `improvement` |
| `808` CI/CD Pipeline and Automation | `pipeline`, `automation`, `job` |
| `809` Migration Backfill and Long Running Refactors | `migration`, `backfill`, `refactor` |
| `810` Retrospectives and Process Effectiveness | `retro`, `lesson`, `improvement` |
| `901` Knowledge Index and Navigation | `index`, `map`, `nav` |
| `902` Traceability Graph | `trace`, `link`, `edge` |
| `903` Source Provenance and Confidence | `source`, `evidence`, `confidence` |
| `904` Context Coverage and Consistency | `coverage`, `gap`, `check` |
| `905` Context Retrieval and RAG | `rag`, `package`, `retrieval` |
| `906` Ontology Vocabulary and Relationship Taxonomy | `term`, `ontology`, `taxonomy` |
| `907` Ingestion Bindings and Extraction Recipes | `binding`, `ingest`, `recipe` |
| `1001` Data Products and Contracts | `data`, `contract`, `product` |
| `1002` Data Lineage and Schema Evolution | `schema`, `lineage`, `evolution` |
| `1003` Feature and Embedding Lifecycle | `feature`, `embedding`, `version` |
| `1004` Model Registry and Versioning | `model`, `registry`, `version` |
| `1005` Training Evaluation and Experimentation | `experiment`, `result`, `model` |
| `1006` Drift Monitoring and Retraining | `drift`, `monitor`, `retrain` |
| `1007` Labeling Annotation and Ground Truth | `label`, `annotation`, `dataset` |
| `1008` Data Quality and Observability | `dq`, `metric`, `monitor` |
| `1101` Team Topology and RACI | `team`, `role`, `raci` |
| `1102` Ownership Graph | `owner`, `team`, `responsibility` |
| `1103` Decision Rights and Governance | `gov`, `decision`, `policy` |
| `1104` Skills and Capability Matrix | `skill`, `capability`, `gap` |
| `1105` Vendors and External Responsibilities | `vendor`, `responsibility`, `contract` |
| `1106` Capacity and Allocation | `capacity`, `allocation`, `plan` |
| `1201` Cost Allocation and Showback | `cost`, `allocation`, `showback` |
| `1202` Capacity and Commitment Management | `capacity`, `commitment`, `plan` |
| `1203` Token and Inference Economics | `budget`, `token`, `inference` |
| `1204` Cost of Quality and Incident | `cost`, `incident`, `quality` |
| `1205` Unit Economics by Service Tenant Request | `unitcost`, `service`, `tenant` |
| `1206` Carbon and Sustainability Accounting | `carbon`, `sustainability`, `metric` |

If a layer needs a more specific local vocabulary, it may extend these names, but it must not replace them with ambiguous generic names.

## 10. Validation Rules

1. Artifact filenames must match `^[a-z][a-z0-9_]*-[0-9]{11}-[a-z0-9-]+\.[a-z0-9]+$`.
2. The same `artifact_file_id` must not be reused within the same layer on the same date.
3. Artifact kinds should come from the canonical vocabulary or a documented layer-specific extension.
4. Renaming the memo part is allowed if the semantic unit remains the same.
5. Reusing an old file for a new semantic unit is forbidden.

## 11. Examples

```text
aismm/
  b4-product-behavior/
    401-requirements/
      req-26051900001-user-login.md
      req-26051900002-password-reset.md
    402-domain-model-and-business-rules/
      rule-26051900001-pricing-policy.md
      entity-26051900002-subscription.md
```

<!-- AISMM:END -->