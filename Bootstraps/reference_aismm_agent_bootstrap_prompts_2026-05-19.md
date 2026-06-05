# AISMM Canonical Bootstrap Prompts

## Purpose

This file contains short canonical prompts for agents that must execute AISMM bootstrap passes deterministically.

Use these prompts together with the canonical bootstrap family in this directory and the repository workflow rules in `../aismm-repository-workflow.md`.

---

## Upstream Prompt

```text
Run the AISMM upstream bootstrap.

Use only these canonical files:
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_upstream_bootstrap_2026-05-19.md
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_bootstrap_layer_write_map_2026-05-19.md
- 00-meta/software-meta-model-main/aismm-repository-workflow.md

Rules:
1. Follow U1 -> U7 in order.
2. Start from aismm.registry.json when it exists.
3. Read root AGENTS.md and follow repository-local agent instructions.
4. Use only the declared read set for each step before widening search.
5. Write into layer directories and artifact files, never legacy shared layer files.
6. Distinguish fact, proposal, draft, and open question explicitly.
7. For actor-facing changes, establish or update value articulation in b0.002 before closing scenario and requirement work.
8. Work on a non-main branch unless an explicit exception exists.
9. Use a dedicated upstream or baseline branch and treat draft PR visibility as separate from merge readiness.
10. Do not emit downstream handoff before approved_for_execution.
11. Keep proposal documents as backlog memory until accepted, rejected, deferred, or merged.
12. Close the upstream branch after merge and restart later stages from updated main.
13. Decompose executor-specific work items into simple task records when one named executor, one team queue, or one external service request must be tracked independently.

Return:
- final upstream status
- created or updated artifacts per step
- known gaps
- downstream handoff package when approved_for_execution
```

---

## Downstream Prompt

```text
Run the AISMM downstream bootstrap.

Use only these canonical files:
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_downstream_bootstrap_2026-05-19.md
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_bootstrap_layer_write_map_2026-05-19.md
- 00-meta/software-meta-model-main/aismm-repository-workflow.md

Rules:
1. Accept downstream only with approved_for_execution and a complete handoff package.
2. Read root AGENTS.md and follow repository-local agent instructions.
3. Follow D1 -> D6 in order.
4. Work on a non-main branch unless an explicit exception exists.
5. Start downstream work from the accepted main state, not from an unmerged upstream draft.
6. Use stage branches by default: architecture, downstream, qa-deploy, reconcile.
7. Do not release without explicit acceptance.
8. Do not end downstream before D6 reconciliation.
9. Update source-of-truth layers, traceability, provenance, and gaps.
10. Run consistency/health close-out before treating the cycle as complete.
11. Treat release merge, runtime observation, and reconciliation as separate results unless a small change keeps them explicit in one branch.
12. Keep executor-facing tasks auditable through request ids, completion facts, and result evidence when downstream depends on external queues or service provisioning.

Return:
- final downstream status
- created or updated artifacts per step
- return-path decisions, if any
- reconciliation result
```

---

## Bootstrap Compliance Audit Prompt

```text
Audit the current AISMM product model against the canonical bootstrap family.

Sources:
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_upstream_bootstrap_2026-05-19.md
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_downstream_bootstrap_2026-05-19.md
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_bootstrap_layer_write_map_2026-05-19.md
- 00-meta/software-meta-model-main/aismm-repository-workflow.md

For each step U1-U7 and D1-D6 report one of:
- supported
- partially_supported
- missing

Explain:
- which current layers/artifacts cover the step
- which first-class artifacts are missing
- which gaps block deterministic execution
```

---

## Product Model Improvement Prompt

```text
Compare the current product model with the canonical AISMM bootstrap family.

Use:
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_upstream_bootstrap_2026-05-19.md
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_downstream_bootstrap_2026-05-19.md
- 00-meta/software-meta-model-main/Bootstraps/reference_aismm_bootstrap_layer_write_map_2026-05-19.md
- 00-meta/software-meta-model-main/aismm-repository-workflow.md
- current product artifacts under aismm/

Produce:
1. covered bootstrap steps
2. partially covered steps
3. missing first-class artifacts
4. minimal backlog needed for deterministic agent execution
```