# AISMM Canonical Downstream Bootstrap

## Purpose

This bootstrap defines the **standard downstream pass** for AISMM after an upstream cycle has produced `approved_for_execution`.

Downstream exists to turn the approved package into:

- executed work
- verified acceptance
- released product state
- observed runtime evidence
- reconciled product model state

---

## Mandatory Preconditions

Do not start downstream unless the input package includes:

- approved change request
- work items and dependencies
- decomposed task records when execution depends on one named executor, one team queue, or one external service request path
- source-of-truth routing decision
- scenario and requirement basis
- test basis and quality gates
- approval record
- known gaps and explicit deferred items

---

## Canonical State Flow

```text
approved_for_execution
  -> scheduled_for_delivery
  -> in_execution
  -> in_test
  -> accepted_for_release | returned_for_rework
  -> released_to_production
  -> observed_in_runtime
  -> reconciled_in_model
```

Terminal or interruption states:

```text
blocked_in_execution
rejected_in_acceptance
rolled_back
deferred_after_runtime
```

---

## Global Rules

- follow `D1 -> D6` in order
- do not release without explicit acceptance
- do not treat downstream as complete until `D6` finishes
- update source-of-truth layers, not just projections
- capture physical realization and repository-facing effects when code, files, or deployment artifacts change
- when downstream depends on infrastructure, security, procurement, legal, or another independent executor/service queue, track that dependency as its own task record in `801`
- each executor-facing task should be able to carry the outgoing request/ticket number, the completion fact, and the resulting evidence or output reference
- infrastructure-facing and other executor-facing tasks must stay execution-only: direct actions, explicit required outputs, and explicit escalation paths for any unresolved approval or policy choice
- start downstream from the accepted package already merged into `main`, not from an unmerged upstream draft branch
- use stage-specific branches by default: `feature/architecture-*`, `feature/downstream-*`, `feature/qa-deploy-*`, and `feature/reconcile-*`
- small changes may collapse several downstream stages into one branch only when approvals, evidence boundaries, and close-out remain explicit
- release merge is not the end of the AISMM cycle; runtime observation and reconciliation still remain mandatory

---

## Repository Branch And PR Expectations

- `D1` should make the delivery branch/PR plan explicit before execution deepens.
- Architecture work should land through its own branch when architecture approval is materially separate from implementation approval.
- `D2` normally lives on `feature/downstream-*`.
- `D3` and `D4` may continue on the same branch for a small change, but `feature/qa-deploy-*` is the default when acceptance and release evidence need separate review.
- `D5` and `D6` may continue on a dedicated `feature/reconcile-*` branch when runtime observation happens after release and cannot be honestly closed in the release branch.

---

## Steps

### `D1` Orchestrate Delivery

Read set:

- `aismm.registry.json`
- canonical write map
- upstream handoff package
- `b8.802`
- `b8.808`
- `b8.804` if a release path is already known

Write set:

- delivery sequencing in `802`
- gate map and automation path confirmation in `808`
- release path confirmation in `804` when relevant
- task routing and request-tracking preparation in `801` when delivery depends on external queues or service provisioning

Exit criteria:

- sequencing is explicit
- dependencies are explicit
- gates are explicit
- repository branch and PR path for the downstream stages is explicit
- executor-specific work is decomposed into simple task records whenever separate request ids, completion evidence, or result artifacts will be needed

### `D2` Execute Development

Read set:

- `aismm.registry.json`
- canonical write map
- `b8.802`
- `b8.808`
- `b3.303`
- `b8.809` when migration/refactor path is relevant

Write set:

- execution evidence in `808`
- build/deployment artifacts in `303`
- migration/backfill evidence in `809` when relevant
- task status, request references, completion facts, and result links in `801` when work runs through independent executor/service tasks
- physical identity binding and realization links in the owning artifacts when code/files/symbols changed materially

Exit criteria:

- implementation-stage work is executed or blocked explicitly
- produced artifacts are linkable to the owning change
- executor-specific tasks can be audited through their request ids, completion facts, and result evidence
- external executor tasks do not hide unresolved business or policy decisions behind wording like `confirm whether`; those decisions are already resolved in the handoff package or routed into an explicit escalation

### `D3` Execute Tests And Acceptance

Read set:

- `aismm.registry.json`
- canonical write map
- upstream handoff package
- `b7.701`
- `b8.803`
- `b8.801`

Write set:

- test run and acceptance artifacts in `803`
- acceptance or return-to-rework linkage in `801`

Exit criteria:

- outcome is explicit: accepted, rejected, conditional, or return-for-rework

### `D4` Release And Rollout

Read set:

- `aismm.registry.json`
- canonical write map
- `b8.804`
- `b8.808`
- `b8.803`

Write set:

- release/rollout artifacts in `804`
- promotion/rollback evidence in `808`

Exit criteria:

- change is released, rolled back, or formally stopped before release
- release outcome is explicit, but the branch is not treated as cycle-complete before runtime observation and reconciliation are routed

### `D5` Observe Runtime And Feedback

Read set:

- `aismm.registry.json`
- canonical write map
- `b6.602`
- related runtime layers when present
- `b8.807`
- `b8.805`
- `b8.804`

Write set:

- runtime observation evidence in runtime source-of-truth layers
- feedback loop artifacts in `807` when drift appears
- observation summary in `805` when runtime materially changes understanding

Exit criteria:

- runtime behavior is either confirmed or sent back into a corrective cycle

### `D6` Reconcile Current State

Read set:

- `aismm.registry.json`
- canonical write map
- changed source-of-truth layers
- `b8.805`
- `b9.902`
- `b9.903`
- `b9.904`
- `b8.804`

Write set:

- actual delivered facts in changed source-of-truth layers
- reconciliation/history artifacts in `805`
- updated trace links in `902`
- refreshed provenance in `903`
- drift/staleness/inconsistency notes in `904`

Exit criteria:

- intended vs actual state has been reconciled
- consistency and health checks for the touched slice were run or explicitly deferred with reason
- repository work is ready to be recorded without violating canon
- the stage branch is ready to merge and close without hiding unresolved runtime or reconciliation work

---

## Mandatory Return Paths

- `D1 -> U4` when planning or package completeness is insufficient
- `D2 -> D1` when execution requires replanning
- `D3 -> D2` when implementation rework is required
- `D3 -> U5/U6` when acceptance or test basis is insufficient
- `D4 -> D2` when release is blocked by implementation reality
- `D4 -> D3` when release is blocked by quality/acceptance reality
- `D5 -> U1` when runtime creates a new change cycle
- `D5 -> D2` when a direct corrective cycle is sufficient

---

## Mandatory Close-Out Validation

Before downstream may end in `reconciled_in_model`:

- changed source-of-truth artifacts must be updated
- traceability, provenance, and gaps must be updated
- repeatable checks from `../aismm-consistency-checks.md` must be run for the touched slice
- health expectations from `../aismm-health.md` must be checked for the touched slice
- repository-facing rules from `../aismm-repository-workflow.md` must be satisfied

Downstream is not complete before these conditions are true.