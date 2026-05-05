# AISMM Health Model

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.health
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

AISMM needs **observability over itself**. A repository should be able to report its own structural health: broken references, orphaned entities, stale blocks, low confidence areas, and retrieval blind spots.

The AISMM Health Model defines the checks, metrics, and artifact for self-assessment.

---

## 1. Health Check Categories

| Category | Description |
|----------|-------------|
| `structural` | ID uniqueness, reference resolution, schema validity |
| `traceability` | Orphan detection, trace link coverage, missing relationships |
| `confidence` | Low-confidence blocks, unverified provenance |
| `coverage` | Missing layers, undocumented entities, retrieval gaps |
| `freshness` | Stale blocks, expired validity windows |
| `security` | Untrusted content in authoritative contexts, missing authorship class |

---

## 2. Core Entities

```text
health_metric.*
health_check.*
health_issue.*
orphan_entity.*
broken_reference.*
stale_block.*
low_confidence_area.*
low_coverage_area.*
retrieval_blind_spot.*
schema_violation.*
```

---

## 3. Health Artifact

The health model may produce a machine-readable artifact:

```text
aismm.health.json
```

This file should not be committed to the repository but may be published as a CI artifact or dashboard feed.

### Example

```json
{
  "health_run_id": "health.main.2026_05_05",
  "repository": "https://github.com/orkestron-ai/software-meta-model",
  "aismm_version": "2.0.0",
  "generated_at": "2026-05-05T08:00:00Z",
  "overall_status": "warning",
  "conformance_level": "L3",
  "metrics": [
    {
      "metric_id": "health_metric.entity_coverage",
      "category": "coverage",
      "value": 0.84,
      "threshold": 0.90,
      "status": "warning"
    },
    {
      "metric_id": "health_metric.reference_resolution",
      "category": "structural",
      "value": 1.0,
      "threshold": 1.0,
      "status": "pass"
    },
    {
      "metric_id": "health_metric.confidence_p50",
      "category": "confidence",
      "value": 0.76,
      "threshold": 0.70,
      "status": "pass"
    }
  ],
  "issues": [
    {
      "issue_id": "health_issue.orphan.b4_401_requirement_old_flow",
      "type": "orphan_entity",
      "entity_id": "b4.401.requirement.old_checkout_flow",
      "severity": "warning",
      "description": "Entity has no incoming or outgoing trace links",
      "file": "b4-product-behavior/401-requirements.md"
    },
    {
      "issue_id": "health_issue.stale.b6_602_monitor_dashboard",
      "type": "stale_block",
      "entity_id": "b6.602.monitor.main_dashboard",
      "severity": "info",
      "description": "Block not updated in 90+ days; review_at date has passed",
      "file": "b6-runtime-operations/602-observability-and-monitoring.md"
    }
  ]
}
```

---

## 4. Health Status Levels

| Status | Meaning |
|--------|---------|
| `pass` | All critical and error checks passed |
| `warning` | Some warning-level issues exist; no critical failures |
| `degraded` | Error-level issues present; structural integrity at risk |
| `critical` | Critical issues found; model integrity compromised |

---

## 5. Recommended Health Checks

| Check | Severity | Rule Basis |
|-------|----------|-----------|
| All entity IDs unique | critical | RULE-ID-001 |
| All cross-layer references resolve | critical | RULE-REF-001 |
| No orphaned entities at L3+ | warning | RULE-REF-002 |
| All trace links have type | error | RULE-REF-002 |
| No blocks older than 180 days without review | info | freshness |
| Coverage above 80% for declared layers | warning | coverage |
| No untrusted content in authoritative context packages | critical | RULE-CTX-002 |

---

## 6. CI Integration

```yaml
- name: Run AISMM Health Check
  run: |
    npx aismm-health \
      --root . \
      --output aismm.health.json

- name: Upload health report
  uses: actions/upload-artifact@v3
  with:
    name: aismm-health
    path: aismm.health.json
```

---

## Summary

| Concept | Definition |
|---------|-----------|
| Health artifact | `aismm.health.json` (CI output) |
| Health categories | structural, traceability, confidence, coverage, freshness, security |
| Overall status | `pass` / `warning` / `degraded` / `critical` |

AISMM health observability makes the meta-model self-aware and continuously governable.
