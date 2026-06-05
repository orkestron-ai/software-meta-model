# AISMM Repository Workflow

## Purpose

This document defines how repository work must be performed so that Git history, product-model history, bootstrap execution, and canonical AISMM rules stay consistent.

It applies to:

- canon changes in `00-meta/software-meta-model-main/`
- template mirror changes in `00-meta/software-meta-model-template-main/`
- product-model changes under `aismm/`
- bootstrap/proposal/backlog maintenance

---

## 1. Repository-First Rules

1. Start from `aismm.registry.json` when it exists.
2. Product repositories must contain root `AGENTS.md` as the repository-local agent operating contract.
3. Do not infer model completeness from visible files alone.
4. Do not create a new layer unless it is expected by registry or covered by an explicit structural decision.
5. Use bundle directories, layer directories, and artifact files exactly as defined by the canon.
6. Treat canon, template, and working product model as three different surfaces with different responsibilities.
7. Active AISMM change work should happen on a non-`main` branch unless an explicit approval record or repository policy allows an exception.

---

## 2. One Logical Change Set

One repository change set should correspond to one semantic unit of work.

That unit may include:

- one product change
- one canon change
- one template alignment change
- one bootstrap/regulation update
- one proposal/backlog routing decision

Do not mix unrelated semantic changes into one commit or one review unless they are inseparable for consistency.

---

## 3. Artifact Creation Rules

When repository work creates product-model content:

- write into the target layer directory
- create a new artifact file for a new semantic unit
- preserve existing artifact identity when updating an existing unit
- follow `aismm-layer-artifact-naming.md`

Legacy one-file-per-layer behavior is not allowed in product repositories.

---

## 4. Proposal And Backlog Governance

Proposal files are backlog memory, not implementation truth.

Use the following rule:

- a `proposal_*.md` file may hold an idea, option, or candidate direction
- once the item becomes executable, it must be routed into `b8.801` as a first-class change or work item
- the proposal must then be marked as one of: `promoted_to_change`, `deferred`, `rejected`, or `superseded`

Do not keep an active backlog item only as free text outside the modeled change surfaces.

---

## 5. Commit Readiness Checklist

Before a repository change is ready to commit, the responsible agent or human must ensure:

1. the touched source-of-truth layers are updated
2. provenance/evidence are updated if new facts were introduced
3. traceability is updated if relationships changed materially
4. gaps and contradictions are updated if uncertainty remains
5. canon/template mirrors are synchronized when the canon changed
6. bootstrap mirrors are synchronized when canonical bootstrap docs changed
7. proposal/backlog documents reflect the current routing decision
8. relevant consistency and health checks were run or explicitly deferred with reason

---

## 6. Commit Message Discipline

Recommended commit format:

```text
<type>: <short summary>

AISMM:
- scope: <canon|template|product-model|bootstrap|proposal>
- refs: <layer ids, artifact ids, proposal ids, change ids>
- reason: <why the repository change is needed>
```

Examples:

```text
docs: canonize bootstrap family and repo workflow

AISMM:
- scope: canon, template, bootstrap
- refs: b8.801, b8.802, b8.803, b8.804, b8.805, b8.807, b9.902
- reason: align standard passes with directory/artifact model and close missing governance gaps
```

```text
model: formalize release acceptance cycle for product model

AISMM:
- scope: product-model
- refs: b8.803, b8.804, b8.805
- reason: reconcile actual release evidence with modeled SDLC state
```

The exact Git convention may vary by repository, but the AISMM semantic scope and references must stay recoverable from history.

---

## 7. Canon And Template Sync Rules

When the canon changes:

- update the template mirror if the change affects structure, bootstrap family, naming, or formatting expectations
- update `00-meta/changelog.md`
- do not leave the template teaching an obsolete layout or obsolete process

When only the product model changes:

- update canon/template only if a real governance or structure rule changed
- otherwise keep the rule in product artifacts, provenance, traceability, and backlog surfaces

---

## 8. Release And Reconciliation Rule

If repository work reflects a released or accepted change, the cycle is not complete until:

- the actual delivered state is written back to source-of-truth layers
- `b8.805` records the history/reconciliation result
- `b9.902`, `b9.903`, and `b9.904` are updated when applicable

Git history alone is not a substitute for AISMM reconciliation.

---

## 9. Git Glossary In AISMM Context

| Git term | Meaning in Git | AISMM reading |
|---|---|---|
| repository | project folder with history | the place where the product model, layer records, traceability, gaps, and related artifacts live |
| `main` | main branch | the accepted state of the product model; only reviewed and merged work belongs here |
| branch | isolated line of work | a stage-specific AISMM work surface such as baseline, upstream, architecture, downstream, QA/deploy, or reconciliation |
| commit | recorded change | one semantic step such as formalizing a change, adding requirements, adding release evidence, or reconciling runtime facts |
| diff | change between states | the visible model delta across layers, records, links, evidence, and gaps |
| pull request / PR | request to merge a branch | the AISMM review package that roles inspect and approve |
| draft PR | early non-final PR | the visible in-progress package for a stage that is not yet ready to merge |
| review | PR inspection | role-specific verification of value, requirements, architecture, tests, release evidence, traceability, or governance |
| approval | acceptance of a review surface | a role confirms that the stage is correct in its area of responsibility |
| request changes | review rejection with feedback | the stage package is not ready and must be corrected before merge |
| merge | include branch into `main` | the stage result becomes part of the accepted product model |
| close branch | finish branch lifecycle | the stage is complete and future work must start from updated `main` |
| conflict | incompatible edits to the same place | a semantic or textual collision that must be resolved before the stage can merge |
| tag | version marker | an optional marker for important accepted states such as baseline or release |
| revert | reverse a change | explicit rollback of an incorrect repository change |

---

## 10. Stage Branch Lifecycle Rule

AISMM branches must not live indefinitely.

Use the following default lifecycle:

```text
create stage branch
	-> make stage commits
	-> open PR (draft or review-ready depending on stage maturity)
	-> complete role reviews
	-> receive approvals or requested changes
	-> merge into main
	-> close branch
	-> start next stage from updated main
```

Default rule:

- one AISMM stage = one branch = one PR = one merge = branch closed

This rule may be relaxed only for genuinely small changes where stage boundaries remain explicit and reviewers can still understand what is being accepted.

---

## 11. Canonical Stage Branches

Recommended branch families:

| AISMM stage | Recommended branch pattern | Merge result |
|---|---|---|
| baseline creation | `feature/baseline-<product-or-scope>` | initial accepted model in `main` |
| upstream | `feature/upstream-<change-or-wave>` | approved upstream package in `main` |
| architecture | `feature/architecture-<change-or-wave>` | accepted architectural decision and impact in `main` |
| downstream implementation | `feature/downstream-<change-or-wave>` | implementation-linked model updates in `main` |
| QA and deploy | `feature/qa-deploy-<change-or-wave>` | accepted test, release, and deploy evidence in `main` |
| reconciliation | `feature/reconcile-<change-or-wave>` | reconciled final state in `main` |

Optional branch families may exist for `gap`, `context`, `prod-validation`, or small corrective work, but they must still end in reviewed merge results rather than long-lived drift.

---

## 12. Stage-Specific PR And Merge Rules

### Baseline

- create the baseline on a dedicated `feature/baseline-*` branch
- merge only after the first accepted product-model package exists

### Upstream

- start from current `main`
- use `feature/upstream-*`
- a draft PR may be opened early to make the wave visible
- upstream may merge only after scenarios, requirements, quality gates, approvals, and known gaps are explicit

### Architecture

- create `feature/architecture-*` from accepted `main`
- do not continue architecture work from an unmerged upstream draft branch
- merge only after architectural approval

### Downstream Implementation

- create `feature/downstream-*` from accepted `main`
- link implementation work to the accepted upstream and architecture package already in `main`
- if code and model diverge, record the mismatch explicitly rather than hiding it in the branch history

### QA And Deploy

- use `feature/qa-deploy-*` when testing, release, rollout, rollback, and runtime-readiness evidence need their own stage
- merge only after explicit acceptance and release readiness are recorded
- release merge does not close the whole AISMM cycle by itself

### Reconciliation

- use `feature/reconcile-*` for post-release reconciliation, traceability refresh, provenance refresh, coverage refresh, and closure decisions
- merge only after intended versus actual state has been written back into source-of-truth layers

---

## 13. Release, Tags, And Reconciliation Discipline

- A release-ready branch is not yet a closed AISMM change cycle.
- Acceptance, release, runtime observation, and reconciliation are distinct results and should not be collapsed into one implicit Git event.
- A tag may mark a meaningful accepted state such as baseline creation, a release candidate, or a completed wave, but a tag does not replace source-of-truth reconciliation.
- Production validation may happen in a dedicated `feature/prod-validation-*` branch or inside the reconciliation branch when repository policy keeps those stages together.
- A change is not fully closed until released facts, runtime evidence, and resulting traceability/provenance updates are merged back into `main`.

---

## 14. Summary

Correct repository work in AISMM means:

- registry-first discovery
- root `AGENTS.md` present for repository-local agent rules
- one stage branch for one accepted stage result by default
- one logical change set at a time
- artifact-file discipline
- non-`main` branch discipline for active change work
- draft PRs may expose ongoing work, but merge happens only after the stage result is review-ready
- proposal-to-change routing without backlog drift
- release does not close the cycle without runtime observation and reconciliation
- commit readiness only after provenance, traceability, gaps, and reconciliation are handled