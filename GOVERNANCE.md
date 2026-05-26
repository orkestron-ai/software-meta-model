# Governance

This document describes how decisions are made about AISMM — who decides, how
changes are accepted, and how the maintainer group evolves. It complements
[`CONTRIBUTING.md`](./CONTRIBUTING.md) (how to contribute) and
[`MAINTAINERS.md`](./MAINTAINERS.md) (who the maintainers are).

AISMM is a **specification**. The goal of this governance model is to keep the
spec coherent, stable, and trustworthy while remaining open to well-reasoned
change.

## Roles

**Contributors** are anyone who opens an issue, joins a discussion, or submits
a pull request. No prior status is required.

**Maintainers** are listed in [`MAINTAINERS.md`](./MAINTAINERS.md). They triage
issues, review pull requests, own versioning decisions, and cut releases. The
**lead maintainer** is the tie-breaker and the steward of overall direction
until a broader steering group exists.

## How decisions are made

AISMM uses **lazy consensus** with maintainer approval:

1. A change is proposed as an issue (see the change-request flow in
   [`CONTRIBUTING.md`](./CONTRIBUTING.md)) or a discussion.
2. Maintainers triage it: *Accepted in principle*, *Needs discussion*, or
   *Out of scope* (with reasoning).
3. For accepted changes, a pull request is opened. A PR needs **at least one
   maintainer approval** and must pass automated checks before merge.
4. If maintainers disagree and consensus cannot be reached, the **lead
   maintainer decides** and records the rationale in the issue or PR.

Editorial fixes (typos, formatting, dead links) may skip triage and go straight
to a PR.

## Scope of decisions

Decisions are classified the same way changes are
(see [`aismm-versioning-and-conformance.md`](./aismm-versioning-and-conformance.md)):

| Decision type | Examples | Bar |
| ------------- | -------- | --- |
| Editorial | typos, link fixes, clarifying prose | one maintainer approval |
| Additive | new optional fields, new layers, examples | one maintainer approval + change request |
| Breaking | removing/renaming, semantic changes, new conformance levels | lead maintainer sign-off + change request + migration notes |

Breaking changes must ship with migration guidance (a `migrations/*.json` entry
and notes in [`CHANGELOG.md`](./CHANGELOG.md)).

## Versioning authority

Maintainers own version numbers. AISMM follows semantic versioning as defined
in [`aismm-versioning-and-conformance.md`](./aismm-versioning-and-conformance.md).
The mechanics of cutting a release are in [`RELEASING.md`](./RELEASING.md).

## Adding and removing maintainers

- **Adding:** any maintainer may nominate a contributor with a sustained track
  record of accepted, high-quality contributions and constructive review.
  Addition requires approval of a majority of current maintainers (and, while
  the group is small, the lead maintainer).
- **Stepping down:** maintainers may resign at any time by opening a PR that
  removes them from [`MAINTAINERS.md`](./MAINTAINERS.md).
- **Inactivity:** a maintainer inactive for an extended period may be moved to
  emeritus status by the remaining maintainers.

## Changing this document

Changes to governance are themselves breaking-level decisions: they require
lead-maintainer sign-off and are announced in [`CHANGELOG.md`](./CHANGELOG.md).

## Code of conduct

All participation is governed by [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md).
Maintainers are responsible for enforcement.
