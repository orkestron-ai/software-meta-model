# AISMM Canonical Bootstrap Write Map

## Purpose

This file defines **where bootstrap steps write** in the current AISMM representation.

It is canonical for agents and humans that execute upstream and downstream bootstrap passes.

---

## Operating Invariants

Before any bootstrap write:

1. Start from `aismm.registry.json` when it exists.
2. Read root `AGENTS.md` before agent-assisted repository work.
3. Treat the product model as `bundle directory -> layer directory -> artifact files`.
4. Never recreate the legacy one-file-per-layer model in product repositories.
5. Create new semantic units as new artifact files using `../aismm-layer-artifact-naming.md`.
6. If a layer is not expected by registry, do not create it without a structural decision.
7. If a change is actor-facing, articulate product value in `b0.002` before finalizing `501` and `401` updates.
8. If facts are insufficient, record `partial`, `empty`, `deferred`, or `blocked` instead of inventing content.

---

## Upstream Write Map

| Step | Primary layer directories | Preferred artifact kinds | Mandatory companion updates |
|---|---|---|---|
| `U1` Intake signal | `aismm/b9-knowledge-traceability/903-source-provenance-and-confidence/`, `aismm/b9-knowledge-traceability/904-context-coverage-and-consistency/` | `source`, `evidence`, `coverage` | update `801` only if an active change already exists |
| `U2` Formalize change | `aismm/b8-change-execution/801-work-items-and-change-requests/` | `change`, `task`, `workitem` | link to relevant source/provenance artifacts |
| `U3` Route source of truth | `aismm/b8-change-execution/801-work-items-and-change-requests/`, `aismm/b9-knowledge-traceability/903-source-provenance-and-confidence/`, `aismm/b9-knowledge-traceability/904-context-coverage-and-consistency/` | `change`, `source`, `coverage` | classify `fact`, `proposal`, `draft`, `open_question`; record affected layers |
| `U4` Plan and decompose | `aismm/b8-change-execution/802-planning-and-delivery-flow/`, `aismm/b8-change-execution/801-work-items-and-change-requests/` | `plan`, `flow`, `milestone`, `task` | keep dependencies linked back to `801`; create simple one-executor tasks when execution will need separate request ids and completion evidence |
| `U5` Specify scenarios and requirements | `aismm/b0-product-core/002-aeilus-value-streams/` for actor-facing changes, `aismm/b5-user-interaction/501-user-scenarios-and-ux-logic/`, `aismm/b4-product-behavior/401-requirements/`, optionally `402` and `405` | `value`, `scenario`, `req`, `rule`, `state` | for actor-facing changes, link `002` value articulation to changed behavioral artifacts |
| `U6` Define test basis and quality gates | `aismm/b7-quality-risk-compliance/701-quality-assurance-and-testing/` | `test`, `qa`, `gate` | tie gates to `401`, `501`, and owning change in `801` |
| `U7` Approve execution | `aismm/b11-organization-ownership-governance/1103-decision-rights-and-governance/`, `aismm/b8-change-execution/801-work-items-and-change-requests/` | `decision`, `approval`, `change` | emit downstream handoff package only after approval |

---

## Downstream Write Map

| Step | Primary layer directories | Preferred artifact kinds | Mandatory companion updates |
|---|---|---|---|
| `D1` Orchestrate delivery | `aismm/b8-change-execution/802-planning-and-delivery-flow/`, `aismm/b8-change-execution/808-ci-cd-pipeline-and-automation/`, optionally `804`, and `801` when external queues are involved | `plan`, `flow`, `pipeline`, `release`, `task` | confirm sequencing, gates, rollout path, and task/request routing for external executors when applicable |
| `D2` Execute development | `aismm/b8-change-execution/808-ci-cd-pipeline-and-automation/`, `aismm/b3-implementation/303-build-deployment-and-runtime-artifacts/`, optionally `aismm/b8-change-execution/809-migration-backfill-and-long-running-refactors/`, and `801` when task-tracked dependencies are executed | `pipeline`, `artifact`, `migration`, `task` | record physical identity binding and code/file realization references when relevant; update request ids, completion facts, and result evidence for executor-specific tasks |
| `D3` Execute tests and acceptance | `aismm/b8-change-execution/803-testing-and-acceptance-execution/`, `aismm/b8-change-execution/801-work-items-and-change-requests/` | `test`, `acceptance`, `change` | record pass/fail/rework decision explicitly |
| `D4` Release and rollout | `aismm/b8-change-execution/804-release-version-and-rollout-management/`, `aismm/b8-change-execution/808-ci-cd-pipeline-and-automation/` | `release`, `rollout`, `pipeline` | record promotion, rollback, and go/no-go evidence |
| `D5` Observe runtime | `aismm/b6-runtime-operations/602-observability-and-monitoring/`, related runtime layers if present, `aismm/b8-change-execution/807-feedback-loops-and-learning-cycles/`, `aismm/b8-change-execution/805-change-history-and-decision-log/` | `monitor`, `feedback`, `history` | open corrective loop when runtime diverges from intended behavior |
| `D6` Reconcile actual state | changed source-of-truth layer directories, `aismm/b8-change-execution/805-change-history-and-decision-log/`, `aismm/b9-knowledge-traceability/902-traceability-graph/`, `aismm/b9-knowledge-traceability/903-source-provenance-and-confidence/`, `aismm/b9-knowledge-traceability/904-context-coverage-and-consistency/` | `history`, `trace`, `source`, `coverage` | run consistency, health, and repository readiness checks before closing the cycle |

---

## Proposal And Backlog Routing

When a signal stays at proposal level:

- keep the proposal as a proposal artifact or proposal document
- register provenance in `903`
- record the routing decision in `801` if a change container already exists
- do not silently treat a proposal as an implemented fact

When a proposal becomes executable:

- create or update a `change` artifact in `801`
- link the proposal to the owning change/request
- decompose queue-bound or executor-bound execution into simple `task` records when separate request, completion, and result evidence will be required
- keep the proposal document as backlog memory until accepted, rejected, or superseded

---

## Mandatory Validation Before Cycle Closure

Before a bootstrap cycle may be treated as complete, the responsible step must ensure:

1. changed source-of-truth artifacts are updated
2. provenance and evidence are refreshed
3. traceability is refreshed when links changed materially
4. drift and gaps are recorded in `904`
5. repeatable checks from `../aismm-consistency-checks.md` are run or explicitly deferred with reason
6. model health expectations from `../aismm-health.md` are checked for the touched slice
7. repository-facing outputs follow `../aismm-repository-workflow.md`

---

## Forbidden Legacy Patterns

Bootstrap execution must not:

- create product layer files such as `801-work-items-and-change-requests.md`
- treat one layer as one document by default
- bypass registry expectations
- commit or hand off work before the model is reconciled back to released reality
- treat projections as source-of-truth when the owning layer disagrees