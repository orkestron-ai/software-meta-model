# AISMM Project-Specific Bootstraps (Non-Canonical Tier)

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.project-bootstraps
  spec_version: 3.0.0
  status: accepted
  created: "2026-07-10"
  updated: "2026-07-10"
```

---

## Purpose

AISMM recognizes **two tiers of bootstrap documents**:

1. **Canonical bootstraps** — `00-meta/software-meta-model-main/Bootstraps/` (mirrored in `00-meta/software-meta-model-template-main/Bootstraps/`). Generic, product-agnostic, reusable across every AISMM product repository. See `Bootstraps/README.md`.
2. **Project bootstraps** — an optional `Bootstraps-projects/` directory at the **root of a product repository**, sibling to `aismm/`. Concrete, product/tool-specific operational runbooks that cannot be generalized without losing the specificity that makes them useful.

This document defines the rules for the second tier and how it relates to the first.

---

## 1. Why A Second Tier Exists

Some operational knowledge is genuinely valuable but inseparable from one product's concrete stack: exact file paths, exact class/service names, exact vendor tool call syntax, exact framework conventions, exact codegen commands. Forcing this into the canon would either:

- pollute the canon with product-specific detail that doesn't apply to other products, or
- get stripped of the detail that made it useful in the first place.

`Bootstraps-projects/` gives this material a legitimate home without contaminating the canon.

---

## 2. Location And Structure

```text
<product-repo-root>/
├── aismm/
│   └── 00-meta/
│       └── software-meta-model-main/Bootstraps/   ← canonical bootstraps
├── Bootstraps-projects/                            ← project bootstraps (this tier)
│   ├── README.md
│   └── reference_aismm_<slug>_<date>.md
└── AGENTS.md
```

`Bootstraps-projects/` is a sibling of `aismm/`, not a subdirectory of it — it is explicitly **not** part of the product model (it is not discovered through `aismm.registry.json`, it has no bundle/layer, and it is not subject to `aismm-layer-artifact-naming.md`).

---

## 3. What Belongs In `Bootstraps-projects/`

A document belongs here, instead of in the canon, when it:

- References concrete tool call syntax, MCP servers, or vendor systems specific to one product's toolchain
- References concrete file paths, class names, or framework conventions of one specific codebase
- Would stop being useful if the product-specific detail were removed (i.e. there is no generic residue worth canonizing separately)

If a document is **partly generic and partly product-specific**, split it: canonize the generic method in `00-meta/.../Bootstraps/`, and keep the concrete, product-specific instantiation here, cross-linked to the canonical bootstrap it specializes.

---

## 4. What Must Never Happen Here

- A project bootstrap must never **redefine or contradict** a canonical bootstrap's rules (phase order, structural rules, mandatory hooks). It may only add product-specific concreteness on top.
- A project bootstrap must never store secrets, credentials, or access tokens.
- A project bootstrap must never be treated as authoritative for another product repository — it is scoped to the repository it lives in.
- Canonical bootstraps must never be copied verbatim into `Bootstraps-projects/` — that creates drift-prone duplicates. If a canonical bootstrap applies unchanged, reference it by name; do not copy it.

---

## 5. Naming And Cross-Referencing

- Use the same `reference_aismm_<slug>_<date>.md` naming pattern as canonical bootstraps, so tooling can treat both tiers uniformly for discovery purposes.
- When a project bootstrap specializes a canonical one, state it explicitly at the top of the file, e.g.:

```markdown
> Specializes: `../aismm/00-meta/software-meta-model-main/Bootstraps/reference_aismm_write_tests_2026-07-09.md`
> Scope: this product's <language/framework> stack only.
```

- `Bootstraps-projects/README.md` must list every file it contains with a one-line purpose, and must state clearly that the directory is non-canonical.

---

## 6. Promotion Path

When a project bootstrap turns out to encode a genuinely generic method (not just product-specific detail):

1. Extract the generic method into a new or existing canonical bootstrap in `00-meta/.../Bootstraps/`, stripped of product-specific names/paths/tools, using generic placeholders and examples (e.g. "a documentation source such as Confluence or similar" rather than naming one vendor as if it were the only option).
2. Keep the concrete, product-specific version in `Bootstraps-projects/`, updated to cross-reference the new canonical bootstrap instead of duplicating its method.
3. Update `00-meta/software-meta-model-template-main/Bootstraps/` to mirror the canon change.
4. Update `00-meta/changelog.md`.

This mirrors the proposal-to-change routing discipline in `aismm-repository-workflow.md` — a candidate direction (here: a project-specific bootstrap) becomes canon only through an explicit promotion step, never by silent copy.

---

## 7. Relationship To Registry And Completeness

`Bootstraps-projects/` is deliberately **outside** `aismm.registry.json` discovery — it is operational tooling guidance for agents/humans working in the repository, not product-model content. Its presence or absence must never affect `completeness_status`.

---

## Summary

| Tier | Location | Scope | Subject to layer/naming rules? | Copied into canon? |
|---|---|---|---|---|
| Canonical bootstrap | `00-meta/.../Bootstraps/` | Every AISMM product | No (governance docs, not layer artifacts) | N/A — this is the canon |
| Project bootstrap | `<repo-root>/Bootstraps-projects/` | This repository only | No | Only via explicit promotion (Section 6) |
