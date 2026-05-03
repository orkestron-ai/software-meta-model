# AISMM Structure Specification

## Overview

AISMM (AI-driven Software Meta-Model) is a **block-based, machine-readable meta-model embedded in Markdown files**.

The core principle:

> AISMM is not file-based.  
> AISMM is **block-based** and extracted from arbitrary files.

---

## 1. Core Concept

### 1.1 File as Container

Any file (`.md`, `.txt`, etc.) is treated as a **container of AISMM blocks**.

AISMM Studio:

```
scan files → extract blocks → build graph → render model
```

---

### 1.2 AISMM Block

The fundamental unit of AISMM is a **block**.

```
<!-- AISMM:BEGIN -->
...meta...
<!-- AISMM:META_END -->

...content...

<!-- AISMM:END -->
```

---

## 2. Block Structure

### 2.1 Block Markers

| Marker | Description |
|------|-------------|
| `AISMM:BEGIN` | Start of block |
| `AISMM:META_END` | End of metadata |
| `AISMM:END` | End of block |

---

### 2.2 Canonical Form

```
<!-- AISMM:BEGIN -->
type: <block_type>
layer_id: <3-digit>
layer_key: <string>
document_id: <unique_id>
document_type: <type>
module_scope: <scope>
status: <status>
title: <optional>
references:
  - <refs>
<!-- AISMM:META_END -->

# Human-readable content

...

<!-- AISMM:END -->
```

---

## 3. Required Metadata Fields

| Field | Description |
|------|------------|
| `type` | Block type |
| `layer_id` | 3-digit layer identifier |
| `layer_key` | Stable layer key |
| `document_id` | Globally unique identifier |
| `document_type` | Logical type of document |
| `module_scope` | root or module |
| `status` | lifecycle state |

---

## 4. Optional Metadata Fields

- title
- references
- tags
- version
- owner
- priority

---

## 5. Block Types

### 5.1 Core Types

| Type | Description |
|------|------------|
| layer_document | Main document block |
| entity_definition | Entity description |
| rule_definition | Rules |
| constraint_definition | Constraints |
| assumption_definition | Assumptions |
| prompt | Execution prompt |
| schema_reference | JSON/YAML/BPMN reference |

---

### 5.2 Specialized Types (optional)

- value_model
- economics_model
- event_definition
- control_definition

---

## 6. Layer Identification

### Format

```
001–999
```

---

## 7. Module Scope

```
root
module:<module_id>
```

---

## 8. Document Identity

Must be:
- globally unique
- stable across versions

Example:

```
product.definition.context
value.streams.main
critical.path.primary
```

---

## 9. Referencing

### Cross-layer References

```
references:
  - entity.workflow
  - event.execution_completed
  - control.assign_task
```

### External Files

```
references:
  - file:vss.schema.json
  - file:pnl.schema.json
  - file:process.bpmn
```

---

## 10. Supported File Formats

| Format | Use |
|------|-----|
| .md | default |
| .json | schema/data |
| .yaml | configs |
| .bpmn | processes |
| .xml | structured models |
| .fig | UI models |

---

## 11. Folder Structure (Optional)

AISMM does NOT depend on folder structure but supports it.

### Root Structure

```
.aismm/
  bundles/
    b0/
      001/
      002/
```

---

### Module Structure

```
.smm/
  modules/
    billing/
      bundles/
        b0/
```

---

### Hybrid

```
repo/
  README.md
  docs/
  .aismm/
  .smm/
```

---

## 12. Granularity Rules

AISMM allows:

- Single file
- Layer files
- Multi-doc
- Modular

---

## 13. Entity Identification

Each block must define entities using stable identifiers:

```
entity.*
event.*
control.*
value.*
metric.*
```

---

## 14. Parsing Rules

AISMM Studio must:

1. Scan all files
2. Extract blocks
3. Validate structure
4. Build graph model
5. Resolve references

---

## 15. Validation Rules

### Mandatory

- BEGIN / META_END / END present
- required fields exist
- valid layer_id
- unique document_id

---

### Recommended

- valid references
- consistent module_scope
- schema compliance

---

## 16. Layer Content Rules

Each layer:

- has a limited set of allowed document types
- has preferred formats (defined separately)

See:

```
aismm-layers-prefs.md
```

---

## 17. Layer Specifications

Each layer must have its own specification describing:

- what to include
- structure of content
- required sections

See:

```
/bundles/<bundle>/<layer>/spec.md
```

---

## 18. Composition

```
blocks → layers → bundles → aismm.json
```

---

## 19. Decomposition

```
aismm.json → bundles → layers → blocks → files
```

---

## 20. Core Principle

> AISMM is a semantic graph embedded in Markdown.

---

## 21. Summary

AISMM provides:

- machine-readable blocks
- human-readable documentation
- flexible file structure
- strong typing via metadata
- cross-layer graph linking

---

## Final Definition

AISMM is a block-based, layer-aware, graph-structured meta-model embedded in Markdown, designed for AI-native software development and orchestration.
