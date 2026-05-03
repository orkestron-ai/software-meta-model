# AISMM Layers Preferred Formats Specification

## Overview

This document defines **preferred representation formats for AISMM layers**.

The goal is:

- improve human readability
- standardize modeling approaches
- enable tooling compatibility
- keep AISMM machine-readable

---

## Principles

> - Each layer has a **preferred format**, but Markdown (`.md`) remains the default fallback.
> - Use the best format for humans, but ensure machine readability via AISMM blocks.

---

## 1. The `prefs` Directory

Every bundle directory may contain a `prefs` folder:

```text
bundles/
  b0/
    prefs/
      schema.json
      format-rules.md
    001/
    002/
```

---

## 2. Contents of `prefs`

The `prefs` folder stores:
- **Schemas**: JSON/YAML schemas defining the exact structure of layers.
- **Protocols**: Rules on how layers within the bundle should communicate or be referenced.
- **Formats**: Definitions of the preferred file formats (e.g., Markdown, BPMN, Figma) for specific layers.

---

## 3. Referencing Preferences

AISMM blocks within the bundle layers may contain references to these preference files to enforce structural integrity and preferred representations. 

For example, a block might reference its schema like so:

```
references:
  - file:../prefs/schema.json
```
