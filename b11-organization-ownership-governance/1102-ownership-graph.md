<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 1102
layer_key: ownership_graph
document_id: spec.org.ownership_graph
document_type: layer_specification
module_scope: b11
status: accepted
spec_version: 2.0.0
title: Ownership Graph Layer Specification
<!-- AISMM:META_END -->

# 1102 — Ownership Graph

## 1. Purpose

Map ownership of all AISMM entities to teams and individuals. Every component, service, and data product must have a declared owner.

## 2. Identifiable Entities

```text
ownership.*
owned_entity.*
owner.*
steward.*
supporting_team.*
```

## 3. Required Structure

```yaml
ownership:
  id: ownership.<entity_id>
  owned_entity: entity_id  # any AISMM entity
  owner: team entity_id
  steward: team or person entity_id
  supporting_teams: [team entity_id]
  effective_from: ISO date
```

## 4. Standard Ownership Patterns

```text
team → owns → component (b2.201)
team → owns → service (b2.201)
team → owns → data_product (b10.1001)
team → owns → agent_contract_reference (b11 reference)
```

## 5. Cross-Layer Links

- b2.201 — components
- b10.1001 — data products
- b6.603 — incident accountability
