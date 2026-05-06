# AISMM Model Registry

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.model-registry
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

AISMM can be **distributed across multiple repositories, modules, restricted folders, submodules, and external sources**.

The Model Registry defines where the complete AISMM model lives and what it expects to contain. Without it, tools and AI agents cannot reliably determine whether they have loaded the full model or only a partial view.

```text
The registry is the source of truth for discovering all AISMM sources.
The registry does not contain all model data; it tells where model data lives.
A parser must start from aismm.registry.json when available.
```

---

## 1. Registry File

The canonical machine-readable registry is:

```text
aismm.registry.json
```

This file should live at the **root of the primary repository** that contains the AISMM model.

If `aismm.registry.json` exists, tools and parsers **must** use it as the model discovery root. If it does not exist, completeness status is `unknown`.

---

## 2. Required Registry Fields

```json
{
  "model_instance_id": "<uuid>",
  "product_id": "<uuid>",
  "product_key": "example_product",
  "product_name": "Example Product",
  "aismm_version": "2.0.0",
  "registry_version": "1.0.0",
  "primary_repository": "https://github.com/org/product-repo",
  "sources": [],
  "expected_bundles": [],
  "expected_layers": [],
  "declared_empty_layers": [],
  "restricted_sources": [],
  "optional_sources": [],
  "last_verified_at": null,
  "completeness_status": "unknown"
}
```

### Field Definitions

| Field | Required | Description |
|-------|----------|-------------|
| `model_instance_id` | yes | Stable UUID for this AISMM model instance |
| `product_id` | yes | Stable UUID for the product |
| `product_key` | recommended | Short slug identifier (e.g. `payment_platform`) |
| `product_name` | optional | Human-readable product name |
| `aismm_version` | yes | AISMM spec version this registry targets |
| `registry_version` | yes | Version of the registry schema |
| `primary_repository` | yes | URL of the primary repository |
| `sources` | yes | Array of all model source descriptors |
| `expected_bundles` | recommended | List of bundle IDs expected in this model |
| `expected_layers` | recommended | List of layer IDs expected in this model |
| `declared_empty_layers` | recommended | Layer IDs explicitly declared as empty |
| `restricted_sources` | optional | Sources that require special access |
| `optional_sources` | optional | Sources that may not exist |
| `last_verified_at` | optional | ISO datetime of last completeness check |
| `completeness_status` | yes | One of the canonical completeness status values |

---

## 3. Source Types

Each entry in `sources` must declare a `source_type`:

| Source Type | Description |
|-------------|-------------|
| `local_path` | A path within the same repository |
| `git_repository` | An external Git repository |
| `git_submodule` | A Git submodule within the primary repo |
| `external_url` | A URL-accessible resource |
| `artifact_registry` | An artifact registry (e.g. OCI, npm, Maven) |
| `restricted_repository` | A repository requiring special access/auth |
| `encrypted_source` | An encrypted source (e.g. AISMM restricted layer) |
| `module_repository` | A module-level AISMM sub-repository |

### Source Object Format

```json
{
  "source_id": "src.main_repo",
  "source_type": "local_path",
  "path": ".",
  "status": "available",
  "bundles": ["b0", "b1", "b2", "b3", "b4", "b5", "b6", "b7", "b8", "b9"],
  "required": true
}
```

```json
{
  "source_id": "src.security_restricted",
  "source_type": "restricted_repository",
  "url": "https://github.com/org/product-restricted",
  "status": "restricted",
  "bundles": ["b7"],
  "layers": ["703"],
  "required": false,
  "access_note": "Requires security team credentials"
}
```

---

## 4. Source Status Values

| Status | Meaning |
|--------|---------|
| `available` | Source is accessible and loaded |
| `unavailable` | Source exists but cannot be reached |
| `restricted` | Source requires special access; may not be loaded |
| `encrypted` | Source is encrypted; requires decryption |
| `optional` | Source may legitimately not exist |
| `deprecated` | Source is no longer maintained |
| `missing` | Source was expected but not found |
| `unknown` | Source status has not been verified |

---

## 5. Completeness Status Values

| Status | Meaning |
|--------|---------|
| `complete` | All expected required sources are available and loaded |
| `partial` | Some required sources were unavailable or missing |
| `partial_authorized` | Some sources are restricted but known; authorized subset loaded |
| `unknown` | No registry exists or registry has not been verified |
| `invalid` | A required source is missing and no empty declaration exists |

---

## 6. Expected Bundles and Layers

The registry declares which bundles and layers are expected:

```json
{
  "expected_bundles": ["b0", "b1", "b2", "b3", "b4", "b5", "b6", "b7", "b8", "b9", "b10", "b11", "b12"],
  "expected_layers": ["001", "002", "003", "004", "005", "006", "101", "102", "103"]
}
```

Every expected layer must be satisfied by either:

1. At least one real AISMM block for that layer, **or**
2. An explicit empty layer block (see [`aismm-empty-layer-standard.md`](./aismm-empty-layer-standard.md)), **or**
3. A declaration in `declared_empty_layers`.

```json
{
  "declared_empty_layers": ["1004", "1005", "1006"]
}
```

---

## 7. Model Completeness Rules

1. If `aismm.registry.json` exists, parsers **must** use it as the discovery root.
2. If no registry exists, completeness status must be `unknown`.
3. If an expected required source is unavailable, status must be `partial` or `invalid`.
4. If a source is restricted but known, the model may be `partial_authorized`.
5. A parser must report which bundles/layers were loaded and which were unavailable.
6. AI context packages must include `model_completeness_status`.
7. Strict mode must fail if required layers are missing and not declared empty.

---

## 8. Registry-First Parsing Pipeline

When `aismm.registry.json` exists, the parsing pipeline must be:

```text
1.  Locate aismm.registry.json
2.  Load model_instance_id and product_id
3.  Resolve all declared sources
4.  Authenticate to restricted sources if possible
5.  Fetch local and remote repositories
6.  Decrypt encrypted sources if authorized
7.  Scan all declared source paths
8.  Extract AISMM blocks
9.  Validate product_id/model_instance_id consistency across blocks
10. Validate expected layers against loaded blocks
11. Load real blocks or empty layer blocks
12. Build graph
13. Build indexes and retrieval units
14. Report completeness status
```

If no registry exists, fall back to local file scanning but report `completeness_status: unknown`.

---

## 9. Core Concepts

```text
model_registry.*        — the registry document itself
model_source.*          — a declared source location
repository_source.*     — a Git repository source
bundle_source.*         — a bundle-level source declaration
layer_source.*          — a layer-level source declaration
module_source.*         — a module-level sub-repository
restricted_source.*     — a source requiring special access
expected_layer.*        — a layer expected to exist in the model
source_status.*         — current availability status of a source
completeness_status.*   — overall model completeness assessment
```

---

## 10. Example Registry

See [`examples/aismm.registry.example.json`](./examples/aismm.registry.example.json) for a complete worked example.

---

## 11. CI Integration

The registry can be validated in CI:

```bash
aismm-check --registry aismm.registry.json --verify-sources
```

This produces a completeness report that can gate deployment or agent context assembly.

---

## Summary

| Concept | Definition |
|---------|-----------|
| Registry file | `aismm.registry.json` at repository root |
| Discovery rule | Parsers must start from registry if it exists |
| Source types | 8 canonical types (local, git, submodule, url, artifact, restricted, encrypted, module) |
| Source status | 8 values: available, unavailable, restricted, encrypted, optional, deprecated, missing, unknown |
| Completeness status | 5 values: complete, partial, partial_authorized, unknown, invalid |

The model registry makes distributed AISMM discoverable, verifiable, and safe for AI agent consumption.
