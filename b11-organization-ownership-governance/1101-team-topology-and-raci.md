<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1101
layer_key: team_topology_and_raci
document_id: spec.org.team_topology
document_type: layer_specification
module_scope: b11
status: accepted
spec_version: 2.0.0
title: Team Topology and RACI Layer Specification
<!-- AISMM:META_END -->

# 1101 — Team Topology and RACI

## 1. Purpose

Define team structures, roles, collaboration modes, and responsibility assignments (RACI) across the organization.

## 2. Identifiable Entities

```text
team.*
role.*
raci_matrix.*
responsibility.*
collaboration_mode.*
```

## 3. Required Structure

```yaml
team:
  id: team.<org>.<name>
  name: string
  type: stream_aligned | platform | enabling | complicated_subsystem | external
  members: [role entity_id]
  collaboration_mode: collaboration_mode entity_id

raci_matrix:
  id: raci_matrix.<scope>
  scope: entity_id  # what is being governed
  responsible: [team entity_id]
  accountable: team entity_id
  consulted: [team entity_id]
  informed: [team entity_id]
```

## 4. Cross-Layer Links

- b6.603 — on-call responsibilities
- b7.704 — audit responsibilities
- b8.802 — planning ownership
