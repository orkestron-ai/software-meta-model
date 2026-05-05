<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1104
layer_key: skills_and_capability_matrix
document_id: spec.org.skills
document_type: layer_specification
module_scope: b11
status: accepted
spec_version: 2.0.0
title: Skills and Capability Matrix Layer Specification
<!-- AISMM:META_END -->

# 1104 — Skills and Capability Matrix

## 1. Purpose

Track skills, competencies, and gaps across the organization to support planning, hiring, and risk management.

## 2. Identifiable Entities

```text
skill.*
capability.*
competency_level.*
team_capability.*
skill_gap.*
```

## 3. Required Structure

```yaml
team_capability:
  id: team_capability.<team>.<skill>
  team: team entity_id
  skill: skill entity_id
  level: competency_level entity_id
  count: integer

skill_gap:
  id: skill_gap.<team>.<skill>
  team: team entity_id
  skill: skill entity_id
  severity: critical | high | medium | low
  resolution_plan: string or null
