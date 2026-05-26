# Security Policy

AISMM is a **specification**, not a running service. There is no deployed
system to attack here. "Security" for this repository covers two things:

1. **Defects in the security-relevant parts of the specification** — for
   example a rule in [`aismm-context-security.md`](./aismm-context-security.md),
   [`aismm-security.md`](./aismm-security.md), the owner-validation gate, or the
   strict-mode rules that, if followed as written, would lead implementers to
   build something unsafe.
2. **Problems in shipped artifacts** — a malformed or unsafe example, schema,
   or snippet under `examples/` or in a `*.schema.json` that could cause harm
   when copied into a real product repository.

## Supported versions

Security fixes are applied to the current major line. Older majors are kept for
historical reference only.

| Version | Supported          |
| ------- | ------------------ |
| 3.x     | :white_check_mark: |
| 2.x     | :x: (history only) |
| < 2.0   | :x:                |

## Reporting a vulnerability

Please **do not** open a public issue for a security-relevant report.

Use one of the following private channels:

- GitHub's [private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability)
  on this repository (Security → "Report a vulnerability"), or
- email **security@orkestron.ai**.

When reporting, please include:

- the file and section (or layer ID, e.g. `b7.703`) the report concerns
- a description of the problem and why it is a security concern
- a concrete example of how a correct implementer could be led into an unsafe
  state
- any suggested wording or fix

> **Maintainer note:** confirm `security@orkestron.ai` is monitored, or enable
> GitHub private vulnerability reporting under repository **Settings →
> Security**, before publishing.

## Response expectations

- **Acknowledgement:** within 5 business days.
- **Assessment:** we will confirm whether the report is in scope and agree on
  severity.
- **Fix and disclosure:** in-scope issues are addressed in a patch release and
  recorded in [`CHANGELOG.md`](./CHANGELOG.md) under `Security`. We will credit
  reporters who wish to be credited.

Thank you for helping keep AISMM and the products built on it safe.
