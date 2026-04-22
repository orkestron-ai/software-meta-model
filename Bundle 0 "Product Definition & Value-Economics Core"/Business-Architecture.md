<!-- AISMM:BEGIN -->
type: layer_document
layer_id: 004
layer_key: business_architecture_static
document_id: business.architecture.core
document_type: business_architecture
module_scope: root
status: active
title: Business Architecture (Static)
<!-- AISMM:META_END -->

# Business Architecture (Static)

## Overview

This document defines the **static business structure of the product**.

It describes:
- business domains
- business capabilities
- core business entities
- high-level relationships
- ownership zones

AISMM Studio uses this layer to:
- build capability maps
- understand product structure
- link value streams (VSS) to business logic

---

## 1. Business Domains

High-level functional areas of the product.

For each domain:

- **ID:**  
- **Name:**  
- **Description:**  
- **Owned Capabilities:**  

---

## 2. Business Capabilities

Defines what the product is able to do at a business level.

For each capability:

- **ID:**  
- **Name:**  
- **Description:**  
- **Owned by Domain:**  
- **Related Entities:**  

---

## 3. Core Business Entities

Key business-level entities (not technical models).

- **Entity ID:**  
- **Name:**  
- **Description:**  
- **Used in Domains:**  

---

## 4. Business Roles

Roles participating in business operations.

- **Role ID:**  
- **Name:**  
- **Description:**  
- **Related Capabilities:**  

---

## 5. High-Level Relationships

Describes dependencies between domains and capabilities.

- Domain → Domain dependencies  
- Capability → Capability dependencies  
- Entity usage relationships  

---

## 6. Ownership Zones

Defines responsibility boundaries.

- Domain ownership  
- Capability ownership  
- Cross-domain ownership  

---

## 7. Mapping to Value Streams (VSS)

- Domain → VSS converters  
- Capability → value transformations  
- Entity → value nodes  

---

## 8. Mapping to System Design

- Domain → system components  
- Capability → services / modules  
- Entity → data models  

---

## 9. Assumptions

- 

---

## 10. Constraints

- 

---

## Notes

- This layer is **static** and changes rarely
- It must not include process sequences (see Process Layer)
- It must not include technical implementation details
- IDs must remain stable across versions

<!-- AISMM:END -->
