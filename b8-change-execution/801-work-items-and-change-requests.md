<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 801
layer_key: work_items_and_change_requests
document_id: spec.work.items.change.requests
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Work Items and Change Requests Layer Specification
<!-- AISMM:META_END -->

# 801 — Work Items and Change Requests

## 1. Purpose of the Layer

The **Work Items and Change Requests** layer defines **how changes are initiated, described, justified, and tracked**.

It is the **entry point of SDLC execution**.

It describes:

- work items (tasks, bugs, improvements)  
- change requests and intent  
- acceptance scope  
- contextual reasoning and decisions  

It answers:

- What are we changing?
- Why are we changing it?
- What problem are we solving?
- What alternatives were considered?

---

## 2. Layer Role in AISMM

This layer connects:

Intent → Change → Execution

It transforms:

- ideas → structured work  
- problems → actionable tasks  
- discussions → decisions  

Other layers rely on it to:

- plan delivery (802)  
- execute testing (803)  
- build releases (804)  
- preserve knowledge (805–806)  

---

## 3. Main Output

- work items  
- change requests  
- acceptance definitions  
- decision context  
- reasoning summaries  

---

## 4. Core Concepts

### 4.1 Work Item (CRITICAL)

Atomic unit of work:

- task  
- bug  
- improvement  
- spike  

---

### 4.2 Change Request (CRITICAL)

Formalized request for change:

- feature  
- fix  
- refactor  
- optimization  

---

### 4.3 Change Intent (CRITICAL)

High-level purpose:

- problem  
- opportunity  
- hypothesis  

---

### 4.4 Acceptance Scope

Defines completion criteria:

- requirements  
- expected behavior  
- constraints  

---

### 4.5 Work Context (CRITICAL)

Context required to understand the change:

- links to requirements  
- links to system components  
- related risks  

---

### 4.6 Decision Reason (CRITICAL)

Explains why a solution was chosen:

- alternatives considered  
- trade-offs  
- constraints  

---

### 4.7 Discussion Summary (NEW)

Aggregated form of comments:

- key points  
- decisions  
- disagreements  

---

### 4.8 Rejected Alternatives (NEW)

Explicitly captured rejected options.

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Work Item | work_item.* |
| Change Request | change_request.* |
| Change Intent | change_intent.* |
| Decision | decision.* |

---

## 6. Required Structure

### 6.1 Work Items
- id  
- type  
- status  

---

### 6.2 Change Requests
- intent  
- scope  
- priority  

---

### 6.3 Context
- linked requirements  
- linked components  

---

### 6.4 Decision
- chosen solution  
- reasoning  

---

### 6.5 Summary
- discussion summary  
- alternatives  

---

## 7. Preferred Representation

- Markdown  
- task tracking systems (Jira, etc.)  
- JSON/YAML  

---

## 8. Relationships

change_request → implemented_as → work_item  
work_item → affects → requirement  
work_item → affects → component  
decision → justifies → work_item  
change_intent → drives → change_request  

---

## 9. Cross-Layer Links

Behavior:
work_item → relates_to → requirement.*

System:
work_item → affects → component.*

Risk:
change_request → impacts → risk.*

Testing:
work_item → validated_by → test_case.*

---

## 10. Boundaries

Does NOT include:

- planning logic  
- release management  
- runtime monitoring  

---

## 11. Minimal Content

- 1 work item  
- 1 change request  

---

## 12. Completeness

- intent defined  
- scope clear  
- reasoning captured  
- links to AISMM entities established  

---

## 13. Summary

Defines how changes are **initiated, justified, and structured into executable work with preserved context and reasoning**.

<!-- AISMM:END -->
