# Product Definition & Value-Economics Core (Bundle 0)

## Overview

Bundle 0 represents the **core semantic foundation of the product** within AISMM.

It defines:
- what the product is
- how it creates value
- how it is structured at a business level
- how value translates into economics

This bundle is the **entry point for understanding the product** and the base for all other layers.

---

## Included Layers

- 001 — Product Definition & Context
- 002 — Aeilus Value Streams (VSS)
- 003 — Stakeholders and Motivation
- 004 — Business Architecture (Static)
- 005 — Critical Path (High-Level)
- 006 — Product P&L Model

---

## Purpose

This bundle provides:

- **Semantic definition of the product**
- **Value flow representation (VSS)**
- **Business structure**
- **Critical execution path**
- **Economic model**

It is used by AISMM Studio to generate:

- Product Card
- Value Graph
- Economic Model
- Core Product Overview

---

## Schema Definition

The structure of this bundle is formally defined in:

👉 `b0aismm.schema.json`

This schema:

- aggregates all layers of Bundle 0
- defines how they are composed into a unified model
- is used during:
  - export
  - composition
  - decomposition

---

## Integration into AISMM

Bundle 0 is included into the global model:

👉 `aismm.json`

Where:

```json
{
  "bundles": {
    "b0": {
      "$ref": "b0aismm.schema.json"
    }
  }
}
```

---

## Notes

- Bundle 0 is **mostly static**
- Changes here indicate **major product evolution**
- All entities must have stable IDs
- Other bundles depend on this bundle

---

## Summary

> Bundle 0 defines the product, its value, and its economic foundation.
