# Bundle 11 — Organization, Ownership and Governance

## Overview

Bundle 11 defines **how teams, ownership, decision rights, skills, vendors, and capacity are modeled as first-class AISMM entities**.

It is the **organizational layer of AISMM**, connecting human structures to the technical model.

AISMM has stakeholders (b0.003) and motivation, but b0 models business stakeholders from a value perspective. Bundle 11 models the **operating organization** that owns, funds, operates, approves, and evolves the product.

This bundle is **separate from b7 (Security/Compliance)** because:

- b7 models security controls, risks, and compliance obligations
- **b11 models organizational authority, ownership, and accountability** — who is responsible, who decides, and what capacity exists

---

## Layers Overview

### 1101 — Team Topology and RACI

Defines team structures, roles, and responsibility assignments.

```text
Who does what and how do teams interact?
```

### 1102 — Ownership Graph

Maps ownership of all AISMM entities to teams and individuals.

```text
Who owns this component, service, or data product?
```

### 1103 — Decision Rights and Governance

Defines who has authority to make which decisions.

```text
Who decides? What requires approval?
```

### 1104 — Skills and Capability Matrix

Tracks skills, competencies, and gaps across the organization.

```text
What capabilities do we have? What are we missing?
```

### 1105 — Vendors and External Responsibilities

Models vendor relationships, external responsibilities, and support contracts.

```text
What do we depend on externally? Who is responsible?
```

### 1106 — Capacity and Allocation

Tracks team capacity, allocation, load, and forecasts.

```text
How much capacity is available? What is allocated where?
```

---

## Cross-Bundle Links

Bundle 11 connects to:

- b0.003 — stakeholders (organizational actors from business perspective)
- b1 — strategy and processes (organizational context)
- b2.201 — components (ownership targets)
- b6.603 — incidents (on-call and escalation)
- b7 — controls and audits (accountability)
- b8 — work items, planning, releases (team capacity and ownership)
- b9 — traceability (ownership as trace link)
- b10.1001 — data products (team ownership)

---

## Schema

```text
b11.schema.json
```

---

## Summary

Bundle 11 connects the human organization to the technical model, ensuring that every component, service, data product, and decision has a clear owner, decision authority, and capacity context.
