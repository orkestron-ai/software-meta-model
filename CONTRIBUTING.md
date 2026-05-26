# Contributing to AISMM

Thank you for your interest in improving AISMM. This document explains how to
propose changes to the AI-driven Software Meta-Model specification.

AISMM is a **specification**, not a library. Most contributions are changes to
the meta-model itself — new layers, refined rules, clarifying examples,
schema fixes. The process below is tuned for that.

---

## Ways to contribute

- **Discussion** — open a [GitHub Discussion](https://github.com/orkestron-ai/software-meta-model/discussions) for questions, ideas, or design conversations before they crystallize into a concrete proposal.
- **Bug in the spec** — open an issue using the *Bug in spec* template (typo, broken cross-reference, contradiction between documents).
- **Change request** — open an issue using the *Change request* template to propose adding, modifying, or removing layers, rules, schemas, or governance content.
- **Pull request** — submit a PR after a change request has been triaged and accepted in principle. Small editorial fixes (typos, formatting, link rot) can skip the change-request step and go directly to PR.

If you are unsure whether a change is editorial or substantive, open a
discussion first. We would rather scope it together than have you redo work.

---

## Change classification

Every change to AISMM falls into one of the categories defined in
[`aismm-versioning-and-conformance.md`](./aismm-versioning-and-conformance.md):

| Category | Examples | Versioning impact |
|---|---|---|
| **Editorial** | typos, formatting, link fixes, clarifying prose without changing meaning | none |
| **Additive** | new optional fields, new layers, additional examples | MINOR bump |
| **Breaking** | removing fields, changing required/optional, renaming IDs, semantic changes to existing rules | MAJOR bump |

Your PR should declare which category it belongs to and reference the issue
or discussion where the change was proposed.

---

## Proposing a substantive change

Substantive changes (anything beyond editorial) go through this flow:

1. **Open a change-request issue** describing the problem the change solves
   and the proposed solution at a high level.
2. **Wait for triage**. A maintainer will respond with one of: *Accepted in
   principle* (proceed to PR), *Needs discussion* (move to Discussions),
   *Out of scope* (with reasoning).
3. **Submit a PR** referencing the accepted change request.
4. **Iterate on review**. The PR must pass automated checks (markdown lint,
   link check, schema validation) and at least one maintainer review.
5. **Merge**. Accepted changes are merged into `main` and listed in the
   `[Unreleased]` section of [`CHANGELOG.md`](./CHANGELOG.md) until the next
   release.

For larger proposals (new bundle, restructured governance, conformance level
changes), expect the process to take several rounds. Substantial changes to a
stable specification require care.

---

## Pull request requirements

Every PR must:

- update [`CHANGELOG.md`](./CHANGELOG.md) under `## [Unreleased]` with one
  line per user-visible change, classified as `Added / Changed / Deprecated
  / Removed / Fixed / Security`
- update [`aismm-layer-inventory.md`](./aismm-layer-inventory.md) if layers
  were added, removed, or had their schema/README coverage change
- include or update at least one example in `examples/` if the change is
  user-facing and not purely internal
- pass all checks in `.github/workflows/`

When a PR changes a layer specification, also update the corresponding
`bN.schema.json`. Schema and spec drift is a common bug class — keep them in
sync within the same PR.

---

## Commit and PR style

- Write commit messages in the imperative mood: *"Add layer b8.811 for
  customer feedback hooks"* not *"Added"* or *"Adding"*.
- One logical change per PR. Split unrelated changes into separate PRs even
  if you noticed them in the same session.
- PR title is one sentence, ≤ 70 characters. PR body explains *why*, links
  the change request, and lists what changed at a high level.
- Reference layer IDs by their full form: `b4.405`, `b10.1004` — not
  `405` alone (it's ambiguous across bundles).

---

## Local checks

Before opening a PR, run the same checks CI runs:

```bash
# Markdown lint
npx markdownlint-cli2 "**/*.md" "#node_modules"

# Link check
npx markdown-link-check **/*.md

# Schema validation (validates *.schema.json against JSON Schema draft 2020-12)
npx ajv-cli validate -s b0-product-core/b0.schema.json -d examples/*.json
```

CI runs these on every push to `main` and every PR. PRs are not merged with
failing checks.

---

## Code of conduct

Participation in AISMM is governed by [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md).
Be kind, be specific, assume good faith.

---

## Licensing

By contributing to AISMM you agree that your contributions will be licensed
under the [Apache License 2.0](./LICENSE), the license under which the
project is distributed.

If you contribute material that is not your original work (a quote, a
diagram, an excerpt), you must clearly mark it and confirm the source allows
inclusion under Apache-2.0 terms.

---

## Maintainers

Active maintainers and decision rights are listed in
[`MAINTAINERS.md`](./MAINTAINERS.md). The change-approval process is described
in [`GOVERNANCE.md`](./GOVERNANCE.md).
