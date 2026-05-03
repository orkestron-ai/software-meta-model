<!-- AISMM:BEGIN -->
type: layer_document
layer_id: 002
layer_key: aeilus_value_streams
document_id: value.streams.definition
document_type: value_streams
module_scope: root
status: active
title: Aeilus Value Streams (VSS)
references:
  - file:vss.schema.json
<!-- AISMM:META_END -->

# Aeilus Value Streams (VSS)

## Overview

This layer defines **Value Streams using Æilus methodology**.

Value Streams describe how value is:
- created
- transformed
- transferred
- accumulated or lost (anti-value)

across the system.

The canonical representation of Value Streams is defined in:

👉 `vss.schema.json`

---

## What is VSS (Value System Schema)

VSS (Value System Schema) is a structured representation of a system in terms of:

- **Converters** — entities that transform value
- **Flows** — value and anti-value moving between converters
- **Actors (contextual)** — participants relevant for value context (may be external to system interaction)
- **Inputs / Outputs** — entry and exit points of value
- **Value / Anti-value metrics** — quantitative or qualitative measures

VSS focuses on **value transformation**, not implementation details.

---

## Purpose of this Layer

This layer answers:

- Where does value come from?
- How does value move through the product?
- Where is value increased or lost?
- Which parts of the system create or destroy value?

---

## Source of Truth

All Value Stream definitions must conform to:

👉 `vss.schema.json`

This schema defines:
- structure of converters
- structure of flows
- identifiers and relationships
- allowed attributes

AISMM Studio parses VSS definitions using this schema.

---

## Core VSS Entities

### Converter
A unit that transforms value.

### Flow
A directed connection representing value or anti-value movement.

### Value Node
A measurable point of value creation or loss.

### Context Actor
An actor relevant for value perception but not necessarily a system user.

---

## Relationship to Other AISMM Layers

VSS is connected to the rest of AISMM through shared identifiers and semantic mappings.

---

### 1. Product Definition & Context (Layer 002)

- VSS converters may represent product modules or external systems
- Context actors in VSS must map to context entities

```
VSS.converter → product.module / external_system
VSS.actor → context actor
```

---

### 2. Stakeholders (Layer 003)

- Value perception is defined per stakeholder
- Stakeholders evaluate value created by flows

```
VSS.value → stakeholder motivation
```

---

### 3. Business Architecture (Layer 004)

- Converters correspond to business capabilities or domains

```
VSS.converter → business capability / domain
```

---

### 4. Critical Path (Layer 005)

- Critical path is a selected chain of VSS flows

```
Critical Path ⊂ VSS graph
```

---

### 5. Product Behavior (Bundle 4)

- Converters are realized by controls/actions
- Flows are realized by state transitions

```
VSS.converter → control
VSS.flow → state transition
```

---

### 6. Events Layer (implicit via behavior)

- Value changes are observable via events

```
VSS.flow → event
```

---

### 7. Economics (P&L Model)

- Value flows are mapped to revenue and cost

```
VSS.flow → revenue / cost
```

---

### 8. Observability (Layer 602)

- Metrics track value realization

```
VSS.node → metric
```

---

## Important Notes

- VSS is **technology-agnostic**
- VSS does not describe UI, API, or code
- VSS operates at **value abstraction level**

---

## Interpretation

VSS provides the **semantic backbone** for:

- product understanding
- economic modeling
- decision-making
- optimization

---

## Summary

> VSS describes how value moves through the system.  
> Other layers describe how the system makes that movement possible.

<!-- AISMM:END -->
