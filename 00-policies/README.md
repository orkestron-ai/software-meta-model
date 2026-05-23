# 00-policies — Product Operating Model (v3)

`00-policies/` is the home for a product's own **normative content** (`kind_class: normative`): the rules, methods, policies, standards and extraction recipes the team chooses to follow.

It is one of the three top-level surfaces of a product AISMM repository:

```text
00-meta/      → how the MODEL works (meta-model canon + template)
00-policies/  → how the PRODUCT/TEAM works (normative)  ← this folder
aismm/        → the DATA (descriptive facts + projection records)
```

> This folder in the canon repository is a documentation stub. In a real **product** repository, `00-policies/` is populated with the product's normative records.

## What goes here

- ways of working, Definition of Done / Definition of Ready
- branching, review and release gating policy
- hypothesis-validation method (drives `b1.104`) and retrospective method (drives `b8.810`)
- ingestion policy and **extraction recipes/prompts** referenced by `b9.907`
- owner-validation / auto-validation policy
- product-level standards adopted from the shared baseline via `inherits_from` / `override`

## What does NOT go here

- product facts (requirements, components, incidents) → `aismm/` (descriptive)
- domain-local policies that belong next to their domain → keep in the bundle layer, tagged `kind_class: normative`, grouped under `_normative/`
- the meta-model canon → `00-meta/`

## Suggested layout

```text
00-policies/
  README.md
  ways-of-working/
    policy-YYMMDDNNNNN-definition-of-done.md
  delivery/
    policy-YYMMDDNNNNN-branching-and-release-gating.md
  methods/
    method-YYMMDDNNNNN-hypothesis-validation.md
    method-YYMMDDNNNNN-retrospective.md
  ingestion/
    recipe-YYMMDDNNNNN-requirements-from-gitlab-issues.md
    policy-YYMMDDNNNNN-owner-validation.md
```

Records follow the canonical naming rule `{record_kind}-{record_file_id}-{memo}.md` with normative record kinds: `policy`, `rule`, `proc`, `standard`, `method`, `recipe`.

## See also

- [`../aismm-content-classification.md`](../aismm-content-classification.md) — `kind_class` and the rules-vs-data split
- [`../aismm-standards-and-inheritance.md`](../aismm-standards-and-inheritance.md) — inheriting shared baselines
- [`../aismm-ingestion-and-source-binding.md`](../aismm-ingestion-and-source-binding.md) — where extraction recipes live
- [`../aismm-owner-validation-and-attestation.md`](../aismm-owner-validation-and-attestation.md) — owner-validation policy
