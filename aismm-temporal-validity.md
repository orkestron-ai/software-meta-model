# AISMM Temporal Validity Model

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.temporal-validity
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

AISMM uses Git history for time, but Git diffs do not support model-level temporal reasoning such as:

- What requirements were active during release v1.4.0?
- Which architecture decisions were valid when incident X occurred?
- When did a requirement become valid?
- Which APIs are deprecated but still supported?
- When does a deprecation end?

Temporal validity adds explicit time-awareness to AISMM entities.

---

## 1. Core Temporal Concepts

| Field | Type | Description |
|-------|------|-------------|
| `valid_from` | ISO date | Date from which the entity is valid |
| `valid_to` | ISO date or null | Date after which the entity is no longer valid; null means currently valid |
| `effective_from_release` | entity ID | AISMM release ID from which this entity takes effect |
| `effective_until_release` | entity ID or null | AISMM release ID after which this entity no longer applies |
| `superseded_by` | entity ID or null | ID of the entity that replaced this one |
| `supersedes` | entity ID or null | ID of the entity this one replaced |
| `archived_at` | ISO date or null | Date the entity was archived |
| `review_at` | ISO date or null | Scheduled review date |
| `temporal_scope` | string | Describes the time scope: `point_in_time`, `interval`, `open_ended`, `deprecated` |

---

## 2. Entity Temporal Pattern

Any AISMM entity may include temporal validity fields:

```json
{
  "id": "b4.401.requirement.user_login",
  "valid_from": "2026-01-01",
  "valid_to": null,
  "effective_from_release": "b8.804.release.v1_2_0",
  "effective_until_release": null,
  "superseded_by": null,
  "supersedes": "b4.401.requirement.user_login_legacy",
  "temporal_scope": "open_ended"
}
```

```json
{
  "id": "b2.203.api_operation.get_user_v1",
  "valid_from": "2024-03-01",
  "valid_to": "2026-06-01",
  "effective_from_release": "b8.804.release.v1_0_0",
  "effective_until_release": "b8.804.release.v3_0_0",
  "superseded_by": "b2.203.api_operation.get_user_v2",
  "temporal_scope": "deprecated"
}
```

---

## 3. Temporal Scopes

| Scope | Meaning |
|-------|---------|
| `point_in_time` | Valid at a single moment only |
| `interval` | Valid between `valid_from` and `valid_to` |
| `open_ended` | Valid from `valid_from` with no end date |
| `deprecated` | Once valid, now deprecated; `valid_to` may be future |
| `scheduled` | Declared but not yet valid; `valid_from` is in the future |

---

## 4. Temporal Query Examples

AISMM temporal validity enables the following model-level queries:

### What requirements were active during release v1.4.0?

```text
Find all b4.401 entities where:
  effective_from_release <= b8.804.release.v1_4_0
  AND (effective_until_release IS NULL OR effective_until_release > b8.804.release.v1_4_0)
```

### Which decisions were valid when incident X happened?

```text
Find all b8.805.decision entities where:
  valid_from <= incident.occurred_at
  AND (valid_to IS NULL OR valid_to > incident.occurred_at)
```

### Which APIs are deprecated but still supported?

```text
Find all b2.203 entities where:
  temporal_scope = "deprecated"
  AND valid_to > today
```

### When did requirement Y become superseded?

```text
Find b4.401.requirement.Y.valid_to
And b4.401.requirement.Y.superseded_by → follow chain
```

---

## 5. Temporal Validity in Trace Links

Trace links also support temporal validity:

```json
{
  "source": "b4.401.requirement.user_login",
  "target": "b3.302.module.auth_service",
  "relationship_type": "implements",
  "valid_from": "2026-01-15",
  "valid_to": null
}
```

This allows reasoning over which traceability paths were valid at any given point in time.

---

## 6. Interaction with Git History

Temporal validity fields are **model-level declarations** and complement Git history:

| Mechanism | Purpose |
|-----------|---------|
| Git commit timestamps | When the file was changed |
| `valid_from` / `valid_to` | When the entity was semantically valid |
| `effective_from_release` | When the entity took product-level effect |

These are intentionally separate. A requirement may be declared in Git on January 1 but become effective only from release v2.0.0.

---

## Summary

| Concept | Definition |
|---------|-----------|
| `valid_from` / `valid_to` | Calendar-based validity window |
| `effective_from_release` | Product-release-based validity |
| `superseded_by` | Successor entity for deprecated or replaced items |
| `temporal_scope` | Semantic characterization of validity type |
| Temporal queries | Model-level time-travel over requirements, decisions, APIs, trace links |

Temporal validity enables AISMM to reason over its own history without depending solely on Git log parsing.
