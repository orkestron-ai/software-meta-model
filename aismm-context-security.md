# AISMM Context Security

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.context-security
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

AISMM is consumed by AI agents as authoritative product context. This makes **AISMM itself an attack surface**.

This document defines security concepts for protecting AISMM content from prompt injection, context poisoning, unauthorized retrieval, and untrusted content propagation.

---

## 1. Threat Model

| Threat | Description |
|--------|-------------|
| Prompt injection | Malicious instructions embedded in AISMM blocks that agents execute |
| Context poisoning | Unverified or false content introduced into authoritative blocks |
| Unauthorized retrieval | Restricted blocks accessed via context packages by unauthorized agents |
| Malicious block edits | Unauthorized modification of high-impact blocks |
| Untrusted imported content | Third-party or external content treated as authoritative by agents |
| False authority claims | Blocks that claim authoritative status without proper provenance |

---

## 2. Block Authorship Classes

Every AISMM block should declare an `authorship_class`:

| Class | Description |
|-------|-------------|
| `human_verified` | Reviewed and approved by a named human |
| `agent_authored` | Produced by an AI agent under a known contract |
| `imported_trusted` | Imported from a trusted, verified source |
| `imported_untrusted` | Imported from an external source not yet verified |
| `generated_unverified` | AI-generated content not yet human-reviewed |
| `external_reference` | A reference pointer to an external resource; not inline content |

---

## 3. Security Entities

```text
block_authorship_class.*
block_signature.*
attestation.*
untrusted_content_fence.*
context_poisoning_signal.*
retrieval_redaction_policy.*
context_access_policy.*
trusted_source_policy.*
```

---

## 4. Security Rules

| Rule | Requirement |
|------|-------------|
| Untrusted content marking | All `imported_untrusted` blocks must be marked and must not be treated as authoritative by agents |
| Third-party import authority | Imported third-party content does not inherit AISMM authority by default |
| Restricted block isolation | Restricted blocks must not enter unauthorized context packages |
| High-impact block signing | Blocks with `criticality: high` should support signing or attestation |
| Retrieval redaction | Context assembly must apply redaction policy before exposing content to agents |
| Injection fence | Blocks from untrusted sources must be fenced from instruction-processing context |

---

## 5. Context Access Policy

```yaml
context_access_policy:
  id: context_access_policy.agent_readonly
  applies_to: [agent_context_package.*]
  allowed_authorship_classes:
    - human_verified
    - imported_trusted
    - agent_authored
  denied_authorship_classes:
    - imported_untrusted
    - generated_unverified
  redact_fields:
    - secret_reference.*
    - restricted_content.*
```

---

## 6. Attestation Format

High-impact blocks may include an attestation record:

```yaml
attestation:
  id: attestation.<block_id>
  block_id: entity_id
  attested_by: human or agent ID
  attested_at: ISO datetime
  method: digital_signature | human_review | automated_verification
  signature: optional hash
```

---

## Summary

AISMM context security ensures that the product knowledge model cannot be subverted to inject malicious instructions into AI agent execution. Every block has a declared authorship class, restricted content is isolated, and retrieval applies redaction policy before context assembly.
