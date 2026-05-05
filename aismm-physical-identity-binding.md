# AISMM Physical Identity Binding Model

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.physical-identity-binding
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Purpose

AISMM uses stable logical IDs for all entities. Physical identity binding defines how those logical IDs **connect to real physical artifacts** — files, code symbols, commits, container images, schemas, and runtime instances.

Without this binding, traceability breaks when files move, code is refactored, or infrastructure changes.

---

## 1. Binding Types

| Binding Type | Description |
|--------------|-------------|
| `identity_binding` | Base type — logical entity linked to physical artifact |
| `artifact_binding` | Links to a build or release artifact |
| `symbol_binding` | Links to a code symbol (class, function, module) |
| `commit_binding` | Links to a specific Git commit or commit range |
| `schema_binding` | Links to a database migration, OpenAPI file, or schema file |
| `runtime_binding` | Links to a running instance, deployment, or container |
| `digest_binding` | Links via content-addressable hash (OCI digest, SHA) |

---

## 2. Required Binding Fields

Every physical binding must include:

```json
{
  "id": "identity_binding.auth_service_to_repo_symbol",
  "logical_entity": "b2.201.component.auth_service",
  "binding_type": "code_symbol",
  "repository": "repo:main",
  "path": "src/Auth/AuthService.php",
  "symbol": "AuthService",
  "commit": "optional — pin to specific commit for audit",
  "digest": "optional — content hash for integrity verification",
  "status": "active"
}
```

### Field Definitions

| Field | Required | Description |
|-------|----------|-------------|
| `id` | yes | Stable AISMM ID for the binding record |
| `logical_entity` | yes | AISMM entity ID being bound |
| `binding_type` | yes | One of the canonical binding types |
| `repository` | yes | Repository reference (short name or URL) |
| `path` | yes | Relative path within the repository |
| `symbol` | conditional | Required for `symbol_binding` |
| `commit` | optional | Git SHA to pin the binding to a point in time |
| `digest` | optional | Content hash for integrity checks |
| `status` | yes | `active`, `stale`, `broken`, `archived` |

---

## 3. Standard Binding Patterns

### Component → Code Symbol

```json
{
  "id": "identity_binding.payment_service_to_class",
  "logical_entity": "b2.201.component.payment_service",
  "binding_type": "symbol_binding",
  "repository": "repo:backend",
  "path": "src/Payment/PaymentService.php",
  "symbol": "PaymentService",
  "status": "active"
}
```

### API Operation → OpenAPI Path

```json
{
  "id": "identity_binding.post_payment_to_openapi",
  "logical_entity": "b2.203.api_operation.post_payment",
  "binding_type": "schema_binding",
  "repository": "repo:api-contracts",
  "path": "openapi/payment.yaml",
  "symbol": "POST /payment/confirm",
  "status": "active"
}
```

### Data Entity → Schema File

```json
{
  "id": "identity_binding.user_profile_to_migration",
  "logical_entity": "b2.202.data_entity.user_profile",
  "binding_type": "schema_binding",
  "repository": "repo:backend",
  "path": "migrations/2026_01_10_create_user_profiles.sql",
  "status": "active"
}
```

### Release → Artifact Digest

```json
{
  "id": "identity_binding.release_v2_1_0_to_image",
  "logical_entity": "b8.804.release.v2_1_0",
  "binding_type": "digest_binding",
  "repository": "registry:ghcr.io/myorg/app",
  "path": "app:v2.1.0",
  "digest": "sha256:abc123def456...",
  "status": "active"
}
```

### Runtime Instance → Deployment Artifact

```json
{
  "id": "identity_binding.prod_auth_to_deployment",
  "logical_entity": "b6.601.runtime_instance.auth_prod",
  "binding_type": "runtime_binding",
  "repository": "repo:infra",
  "path": "terraform/prod/auth_service.tf",
  "status": "active"
}
```

---

## 4. Binding Status Lifecycle

```text
active → stale → broken → archived
```

| Status | Meaning |
|--------|---------|
| `active` | Binding is current and verified |
| `stale` | Physical artifact may have moved; needs re-verification |
| `broken` | Physical artifact no longer exists at specified path |
| `archived` | Historical binding kept for audit; no longer active |

---

## 5. Cross-Layer Coverage

Physical identity bindings link AISMM logical layers to real artifacts:

| Logical Layer | Binding Type | Physical Artifact |
|---------------|-------------|-------------------|
| b2.201 Component | `symbol_binding` | Source code class/module |
| b2.203 API Operation | `schema_binding` | OpenAPI path |
| b2.202 Data Entity | `schema_binding` | Migration file / schema |
| b3.302 Module | `symbol_binding` | Code directory or package |
| b3.303 Artifact | `artifact_binding` | Build artifact / container image |
| b6.601 Runtime Instance | `runtime_binding` | Infrastructure file |
| b8.804 Release | `digest_binding` | Container or package digest |

---

## Summary

| Concept | Definition |
|---------|-----------|
| Logical ID | Stable AISMM entity identifier |
| Physical binding | Record linking logical ID to physical artifact |
| Binding types | `identity`, `artifact`, `symbol`, `commit`, `schema`, `runtime`, `digest` |
| Binding status | `active` / `stale` / `broken` / `archived` |

Physical identity binding ensures AISMM traceability survives code refactoring, file moves, and infrastructure changes.
