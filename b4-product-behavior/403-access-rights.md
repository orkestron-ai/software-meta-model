<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 403
layer_key: access_rights
document_id: spec.access.rights
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Access Rights Layer Specification
<!-- AISMM:META_END -->

# 403 — Access Rights

## 1. Purpose of the Layer

The **Access Rights** layer defines **who is allowed to perform which actions under which conditions**.

It describes:

- actors and roles  
- permissions  
- access policies  
- conditions and constraints  
- scopes of access  

It answers:

- Who can do what?
- Under which conditions is access granted or denied?
- What data and operations are accessible?
- How is access controlled and enforced?

---

## 2. Layer Role in AISMM

This layer connects:

Domain Logic → Allowed Behavior → System Enforcement

It transforms:

- domain rules → access constraints  
- actors → roles  
- operations → permissions  

Other layers rely on it to:

- enforce security and control  
- restrict system behavior  
- validate access in processes (103)  
- protect data (202)  
- control API usage (203)  

---

## 3. Main Output

- actors  
- roles  
- permissions  
- policies  
- access conditions  
- scope definitions  

---

## 4. Core Concepts

### 4.1 Actor

Represents a participant interacting with the system:

- user  
- system  
- service  
- external party  

---

### 4.2 Role

Defines a set of permissions assigned to actors.

---

### 4.3 Permission

Defines an allowed action:

- read  
- write  
- execute  
- approve  
- manage  

---

### 4.4 Access Policy (ENHANCED)

Defines rules for granting access:

- role-based (RBAC)  
- attribute-based (ABAC)  
- context-based  

---

### 4.5 Access Condition (NEW)

Defines dynamic constraints:

- time-based  
- location-based  
- state-based  
- ownership-based  

---

### 4.6 Scope (NEW)

Defines boundaries of access:

- entity-level  
- field-level  
- operation-level  
- environment-level  

---

### 4.7 Delegation (NEW)

Defines transfer of permissions between actors.

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Actor | actor.* |
| Role | role.* |
| Permission | permission.* |
| Policy | policy.* |
| Condition | condition.* |
| Scope | scope.* |

---

## 6. Required Structure

### 6.1 Actors
- id  
- type  
- attributes  

---

### 6.2 Roles
- id  
- assigned permissions  

---

### 6.3 Permissions
- action  
- resource  

---

### 6.4 Policies
- rule  
- applicable roles  
- conditions  

---

### 6.5 Conditions
- logic  
- parameters  

---

### 6.6 Scope
- boundaries  
- limitations  

---

## 7. Preferred Representation

- Markdown  
- RBAC/ABAC matrices  
- JSON/YAML  

---

## 8. Relationships

actor → assigned_to → role  
role → grants → permission  
policy → applies_to → role  
permission → applies_to → resource  
condition → restricts → permission  

---

## 9. Cross-Layer Links

Requirements:
permission → enforces → requirement.*

Domain:
policy → constrained_by → business_rule.*

Processes:
step → requires → permission.*

Data:
permission → applies_to → entity.*

API:
operation → requires → permission.*

---

## 10. Boundaries

Does NOT include:

- authentication mechanisms  
- infrastructure security configs  
- encryption details  

---

## 11. Minimal Content

- 1 role  
- 1 permission  

---

## 12. Completeness

- full role model  
- permissions defined  
- policies defined  
- conditions applied  

---

## 13. Summary

Defines **who can do what and under which conditions**, ensuring controlled and secure system behavior.

<!-- AISMM:END -->
