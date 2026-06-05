# AISMM Canonical Bootstraps

## Purpose

This directory is the **canonical bootstrap family** for AISMM.

Bootstrap documents define the standard execution passes that agents and humans must use when they:

- accept new signals and source materials
- formalize changes and route source-of-truth updates
- execute approved work through downstream delivery
- reconcile released reality back into the product model
- validate model consistency, health, traceability, and repository hygiene

These documents are part of the canon.

They are not ad hoc working notes.

The source of truth for bootstrap structure and rules lives here in `00-meta/software-meta-model-main/Bootstraps/`.

---

## Canonical Bootstrap Set

The canonical bootstrap family consists of:

- `reference_aismm_bootstrap_layer_write_map_2026-05-19.md`
- `reference_aismm_upstream_bootstrap_2026-05-19.md`
- `reference_aismm_downstream_bootstrap_2026-05-19.md`
- `reference_aismm_agent_bootstrap_prompts_2026-05-19.md`

Together they define:

- the deterministic state flows
- the allowed read and write surfaces per step
- the required evidence and output artifacts
- the minimum validation hooks before a step can be treated as complete

---

## Structural Rules

All bootstrap execution rules must follow the current AISMM product representation:

- product knowledge lives under `aismm/`
- each bundle is a directory
- each layer is a directory inside its bundle
- semantic units are stored as artifact files inside the layer directory

Therefore bootstrap documents must never instruct agents to create or update one shared layer file such as `801-work-items-and-change-requests.md` in the product model.

Instead they must instruct agents to:

- locate the target layer directory
- create or update one or more artifact files inside that directory
- preserve artifact identity using the naming rules from `../aismm-layer-artifact-naming.md`
- keep initiative granularity at one minimal semantic unit per artifact file and link related initiatives through `Related Objects`

---

## Required Bootstrap Discipline

Every canonical bootstrap pass must enforce the following discipline:

1. Start from `aismm.registry.json` when it exists.
2. Resolve only the step-specific read set first.
3. Write only to the step-specific write set.
4. Use layer directories and artifact files, not legacy one-file-per-layer assumptions.
5. Distinguish source-of-truth updates from projections.
6. Update provenance, traceability, and gaps when the step requires it.
7. Run required consistency and health validation before closing the cycle.
8. Record repository-facing outcomes so Git history, change history, and AISMM history do not diverge semantically.

---

## Mandatory Cross-Cutting Hooks

Canonical bootstraps must explicitly integrate the following canon surfaces:

- `../aismm-model-registry.md` for registry-first discovery and completeness handling
- `../aismm-consistency-checks.md` for repeatable structural validation
- `../aismm-consistency-rules.md` for conflict resolution and lifecycle orchestration
- `../aismm-health.md` for model health reporting after meaningful updates
- `../aismm-physical-identity-binding.md` for linking logical changes to real code, files, symbols, and commits when relevant
- `../aismm-layer-artifact-naming.md` for artifact creation rules

If a bootstrap omits one of these hooks, that omission must be justified explicitly.

---

## Relationship To Template Mirrors

The template mirror under `00-meta/software-meta-model-template-main/` must keep the same bootstrap family so that:

- the canon explains the rules
- the template mirrors the same operational set
- product repositories can copy or adapt the bootstrap family without inventing local variants first

Product repositories should use these canonical files from `00-meta/` rather than storing separate bootstrap mirrors inside the product model.

---

## Summary

Bootstrap documents are canonical AISMM governance artifacts.

They standardize not only **what layers exist**, but also **how a change moves through the model**, **how repository work is recorded safely**, and **how the model is reconciled back to reality without drift**.