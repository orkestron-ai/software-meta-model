# Roadmap

This roadmap describes **direction**, not commitments or dates. Priorities shift
with feedback and field experience. Items move forward through the process in
[`CONTRIBUTING.md`](./CONTRIBUTING.md) and [`GOVERNANCE.md`](./GOVERNANCE.md).

The current released version is **AISMM 3.0.0**. See [`CHANGELOG.md`](./CHANGELOG.md)
for what shipped and [`MIGRATION-v2-to-v3.md`](./MIGRATION-v2-to-v3.md) for the
upgrade path.

## Near term (v3.x)

The spec already describes validation, health, and semantic-diff behavior. The
near-term focus is making those **executable** rather than only specified.

- **Reference validator** — a CLI that checks a product repository against
  strict mode ([`aismm-strict-mode.md`](./aismm-strict-mode.md)) and emits
  `aismm.validation.json`.
- **Schema validation in CI, blocking** — promote the JSON/schema job in
  `.github/workflows/ci.yml` from report-only to required once content is clean.
- **Health report generator** — produce `aismm.health.json` per
  [`aismm-health.md`](./aismm-health.md).
- **More examples** — at least one worked example per bundle under `examples/`,
  plus a minimal end-to-end product repository skeleton.

## Mid term

- **Semantic-diff tooling** — generate `aismm.semantic_diff.json` for PR review
  per [`aismm-semantic-diff.md`](./aismm-semantic-diff.md).
- **Conformance test suite** — machine-checkable fixtures for conformance
  levels L1–L5.
- **Shared standards baseline** — publish `software-meta-model-standards-main`
  and exercise the `inherits_from` / `override` contract from
  [`aismm-standards-and-inheritance.md`](./aismm-standards-and-inheritance.md).

## Longer term

- **Ingestion connectors** — reference MCP connectors and extraction recipes
  for the governed ingestion pipeline
  ([`aismm-ingestion-and-source-binding.md`](./aismm-ingestion-and-source-binding.md)).
- **Traceability graph export** — canonical export to a graph store for the
  b9 traceability graph.
- **Adoption** — case studies and templates that lower the cost of putting a
  real product under AISMM.

## How to influence the roadmap

Open a [discussion](https://github.com/orkestron-ai/software-meta-model/discussions)
or a change-request issue. Field reports from real product repositories are the
most valuable input — AISMM v3 itself was driven by them.
