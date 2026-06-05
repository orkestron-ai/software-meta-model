# AISMM Canonical Upstream Bootstrap

## Purpose

This bootstrap defines the **standard upstream pass** for AISMM.

Use it when a new signal, source material, contradiction, incident, audit finding, feedback item, or product/model change request appears.

Upstream exists to turn a raw signal into an **approved, structured, source-of-truth-aware change package**.

---

## Mandatory Preflight

Before `U1`, the agent must:

1. read `aismm.registry.json` when present
2. read root `AGENTS.md`
3. confirm expected layers vs declared empty layers
4. confirm the product model uses layer directories and artifact files
5. locate the canonical write map in `Bootstraps/reference_aismm_bootstrap_layer_write_map_2026-05-19.md`
6. confirm the current repository work is happening on a non-`main` branch unless an explicit approval or repository policy says otherwise
7. classify the incoming material as `fact`, `proposal`, `draft`, `open_question`, or mixed

If the material mixes fact and proposal without separation, upstream must stop with `blocked_by_missing_context` until the distinction is made explicit.

---

## Canonical State Flow

```text
signal_received
  -> intake_completed
  -> change_formalized
  -> source_of_truth_routed
  -> scope_planned
  -> scenario_specified
  -> tests_ready
  -> approved_for_execution
```

Terminal states:

```text
merged_into_existing_change
rejected
deferred
blocked_by_missing_context
```

---

## Global Rules

- follow `U1 -> U7` in order
- use only the step read set first; do not widen search without a concrete blocker
- create artifact files inside layer directories, not one shared layer file
- update provenance, gaps, and routing decisions as part of the step, not later by memory
- for actor-facing changes, make value impact explicit in `b0.002` before `U5` may close; do not leave actor-visible intent only in `401` or `501`
- do not emit downstream handoff before explicit approval
- keep proposal documents as proposal memory until they are accepted, rejected, deferred, or merged into an owning change
- decompose execution-facing work items into simple task records when the work depends on one named executor, one team queue, or one external service request
- each such task should be assignable independently and should be able to carry a request/ticket identifier, a completion fact, and a result artifact or result summary
- upstream normally runs on a dedicated `feature/upstream-*` branch; baseline creation may use `feature/baseline-*`
- a draft PR may be opened early for visibility, but upstream must not merge before `U7` reaches an explicit approval outcome
- after upstream merges, the branch should close and any architecture or downstream work must restart from updated `main`

---

## Repository Branch And PR Expectations

- `U1` to `U4` may happen while the PR is still in draft state.
- `U5` and `U6` should make the package reviewable by product, QA, and architecture stakeholders.
- `U7` is the point where the branch becomes mergeable or explicitly rejected/deferred.
- If upstream is split across several semantic waves, each wave should still map to a reviewable branch/PR pair rather than one never-ending branch.

---

## Steps

### `U1` Intake Signal

Read set:

- `aismm.registry.json`
- canonical write map
- `b9.903`
- `b9.904`
- `b8.801` only if already present

Write set:

- provenance/evidence artifacts in `903`
- ambiguity or coverage notes in `904`
- link to existing `801` change only if a clear continuation already exists

Exit criteria:

- signal origin is explicit
- evidence/source is registered
- new change vs continuation is decided

### `U2` Formalize Change

Read set:

- `aismm.registry.json`
- canonical write map
- `b8.801`
- directly relevant source-of-truth layers only if already known

Write set:

- `change`, `task`, or `workitem` artifacts in `801`

Exit criteria:

- the change has intent, scope, and at least one work item
- the change is distinguishable from unrelated scope
- if external execution already depends on infra, platform, legal, security, procurement, or another single queue/service, the owning work item is ready to be decomposed further in `U4`

### `U3` Route Source Of Truth

Read set:

- `aismm.registry.json`
- canonical write map
- `b8.801`
- `b9.903`
- `b9.904`

Write set:

- routing decision and affected-layer map in `801`
- provenance classification in `903`
- ambiguity note in `904` when needed

Exit criteria:

- authoritative bundle/layer set is explicit
- projections vs source-of-truth are explicit
- proposal/backlog vs implemented fact is explicit

### `U4` Plan And Decompose

Read set:

- `aismm.registry.json`
- canonical write map
- `b8.801`
- `b8.802`

Write set:

- planning artifacts in `802`
- decomposed execution tasks in `801` when a work item must be handed to one executor, one team queue, or one external service request path

Exit criteria:

- sequencing is explicit
- dependencies are explicit
- the current cycle vs later cycle split is explicit
- multi-party or queue-bound work is decomposed into simple task records whenever independent request tracking or completion evidence will be needed downstream

### `U5` Specify Scenarios And Requirements

Read set:

- `aismm.registry.json`
- canonical write map
- `b0.002` when the change is actor-facing
- `b5.501`
- `b4.401`
- `b4.402` and `b4.405` only when directly relevant

Write set:

- value artifacts in `002` when the change is actor-facing
- scenario artifacts in `501`
- requirement artifacts in `401`
- rule/state artifacts only when directly affected

Exit criteria:

- actor-facing changes have explicit value outcome and anti-value framing linked across `002`, `501`, and `401`
- scenarios and requirements are linked
- expected downstream behavior is testable
- edge cases are either captured or explicitly marked missing

### `U6` Prepare Test Basis And Quality Gates

Read set:

- `aismm.registry.json`
- canonical write map
- changed `501` and `401`
- `b7.701`

Write set:

- test/gate artifacts in `701`
- acceptance linkage update in `401` only if needed

Exit criteria:

- downstream can verify readiness without guessing
- quality gates are explicit

### `U7` Approval To Execute

Read set:

- `aismm.registry.json`
- canonical write map
- `b8.801`
- `b11.1103`
- `b7.702` when risk materially affects approval

Write set:

- approval/authority artifacts in `1103`
- final execution decision in `801`

Exit criteria:

- change is approved, rejected, deferred, or blocked explicitly
- downstream handoff package exists only for `approved_for_execution`

---

## Required Handoff Package

If upstream ends in `approved_for_execution`, the emitted package must contain:

- approved change request
- work items and dependencies
- decomposed task records for executor-specific or service-request-specific work where downstream will need separate tracking
- source-of-truth routing decision
- scenarios and requirements
- test basis and quality gates
- approval record
- known gaps and explicit deferred items

---

## Mandatory Upstream Validation

Before upstream may close with `approved_for_execution`:

- changed artifacts must satisfy the current directory/artifact layout
- provenance and coverage must be updated
- no unresolved ambiguity may remain between `fact` and `proposal`
- actor-facing changes must not close upstream without explicit `b0.002` value articulation or a recorded gap explaining why it is deferred
- repository-facing expectations from `../aismm-repository-workflow.md` must be satisfiable by downstream