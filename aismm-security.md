# AISMM Security Specification

## 1. Overview

This document defines the **security model for AISMM (AI-driven Software Meta-Model)**.

AISMM is a distributed, file-based, block-structured meta-model stored in Git repositories.  
Therefore, security must be addressed at:

- repository level
- file level
- block level
- runtime composition level

---

## 2. Core Principle

> Git does NOT support secure per-file access control.

Therefore:

> AISMM security MUST be implemented outside of Git core mechanics.

---

## 3. Security Layers

### 3.1 Repository-Level Security

The primary security boundary.

**Rule:**
- Sensitive data MUST be stored in separate repositories.

Example:

```
product-repo/
aismm-public/
aismm-sensitive/
aismm-finance/
aismm-security/
```

---

### 3.2 Module-Level Isolation (.smm)

AISMM supports modularization:

```
.smm/
  modules/
    billing/
    security/
```

Sensitive modules SHOULD be:

- isolated
- stored in private repositories
- connected via submodules

---

### 3.3 Submodule Strategy

Sensitive AISMM components SHOULD be included via Git submodules:

```
main-repo/
  .aismm/
    public/
    sensitive/ -> git submodule (private repo)
```

---

### 3.4 Encryption

If separation is not possible, encryption MUST be used.

Recommended tools:

- git-crypt
- Mozilla SOPS
- age / GPG

Example:

```
billing-costs.md (encrypted)
```

---

### 3.5 Access Control via Platform

Use Git platform features:

- GitHub / GitLab permissions
- repository access control
- branch protection

---

### 3.6 CODEOWNERS (Optional)

Used for governance, NOT security.

Provides:
- review enforcement
- ownership mapping

Does NOT provide:
- read access restriction

---

## 4. Context Aggregator Responsibilities

Any AISMM editor or context aggregator MUST:

- support multiple repositories
- merge AISMM graphs from different sources
- respect access boundaries
- fail gracefully if data is missing

---

## 5. Composition Model

AISMM supports secure composition:

```
public AISMM
+ private AISMM modules
→ full internal model
```

Different users see:

| User     | Visible Model     |
| -------- | ----------------- |
| Public   | public only       |
| Internal | public + internal |
| Finance  | + finance         |
| Security | + security        |

---

## 6. Data Classification

Each AISMM block SHOULD be classified:

- public
- internal
- confidential
- restricted

Example:

```
classification: confidential
```

---

## 7. Threat Model

### Risks

- unauthorized access to economic models (e.g., product P&L), which constitute commercial secrets and are restricted to specific personnel
- leakage of product strategy or roadmap
- exposure of security architecture

### Mitigations

- repository separation
- encryption
- access control
- audit logs

---

## 8. Recommendations

### MUST

- separate sensitive AISMM data into private repos
- enforce access control at repository level

### SHOULD

- use submodules for composition
- classify AISMM blocks

### MAY

- encrypt specific files
- use runtime filtering

---

## 9. Non-Goals

AISMM does NOT aim to:

- replace Git security model
- provide file-level encryption natively
- enforce runtime authorization

---

## 10. Final Principle

> AISMM security is achieved by architecture, not by Git.

---

## Summary

AISMM security is based on:

- repository isolation
- modular decomposition
- controlled composition
- optional encryption

---

## End of Document
