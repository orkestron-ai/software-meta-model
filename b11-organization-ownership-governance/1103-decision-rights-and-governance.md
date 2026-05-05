<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1103
layer_key: decision_rights_and_governance
document_id: spec.org.decision_rights
document_type: layer_specification
module_scope: b11
status: accepted
spec_version: 2.0.0
title: Decision Rights and Governance Layer Specification
<!-- AISMM:META_END -->

# 1103 — Decision Rights and Governance

## 1. Purpose

Define who has authority to make which decisions, what requires approval, and how governance bodies operate.

## 2. Identifiable Entities

```text
decision_right.*
governance_body.*
approval_policy.*
review_board.*
decision_authority.*
```

## 3. Required Structure

```yaml
decision_right:
  id: decision_right.<scope>.<decision_type>
  decision_type: string
  authority: team or person entity_id
  approval_policy: approval_policy entity_id
  escalation_path: [team entity_id]

approval_policy:
  id: approval_policy.<name>
  required_approvers: [team or person entity_id]
  minimum_approvals: integer
  timeout: duration or null
```

## 4. Cross-Layer Links

- b7.704 — compliance decisions
- b8.805 — architecture decisions
- b9.902 — traceability of decisions
