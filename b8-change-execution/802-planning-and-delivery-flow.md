<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 802
layer_key: planning_and_delivery_flow
document_id: spec.planning.delivery.flow
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Planning and Delivery Flow Layer Specification
<!-- AISMM:META_END -->

# 802 — Planning and Delivery Flow

## 1. Purpose of the Layer

The **Planning and Delivery Flow** layer defines **how work items move through time, resources, and delivery processes**.

It describes:

- plans and timelines  
- sprints and milestones  
- delivery streams  
- priorities and dependencies  
- capacity and workload distribution  

It answers:

- What are we doing now, next, and later?
- How is work prioritized?
- How do tasks move through delivery stages?
- What are the constraints (time, people, dependencies)?

---

## 2. Layer Role in AISMM

This layer connects:

Work → Plan → Execution Flow

It transforms:

- work items → scheduled work  
- priorities → execution order  
- dependencies → delivery constraints  

Other layers rely on it to:

- execute changes (801)  
- coordinate testing (803)  
- build releases (804)  
- track progress and predict delivery  

---

## 3. Main Output

- plans  
- sprints / iterations  
- milestones  
- delivery pipelines  
- dependencies  
- priorities  

---

## 4. Core Concepts

### 4.1 Plan (CRITICAL)

Structured representation of future work.

---

### 4.2 Sprint / Iteration

Time-boxed delivery cycle.

---

### 4.3 Milestone

Significant delivery checkpoint.

---

### 4.4 Delivery Stream (CRITICAL)

Flow of work through stages:

- backlog → in progress → review → done  

---

### 4.5 Priority (CRITICAL)

Relative importance of work items.

---

### 4.6 Dependency (CRITICAL)

Relationship between work items:

- blocking  
- sequential  
- parallel  

---

### 4.7 Capacity

Available resources:

- team capacity  
- time allocation  

---

### 4.8 Lead Time / Cycle Time (NEW)

Time metrics for delivery performance.

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Plan | plan.* |
| Sprint | sprint.* |
| Milestone | milestone.* |
| Delivery Stream | delivery_stream.* |
| Dependency | dependency.* |

---

## 6. Required Structure

### 6.1 Plans
- scope  
- timeline  

---

### 6.2 Iterations
- duration  
- goals  

---

### 6.3 Milestones
- target date  
- deliverables  

---

### 6.4 Flow
- stages  
- transitions  

---

### 6.5 Dependencies
- source  
- target  

---

### 6.6 Capacity
- allocation  
- limits  

---

## 7. Preferred Representation

- Markdown  
- project management tools (Jira, etc.)  
- Gantt charts  
- Kanban boards  
- JSON/YAML  

---

## 8. Relationships

plan → contains → work_item  
sprint → includes → work_item  
delivery_stream → moves → work_item  
dependency → links → work_item  
milestone → achieved_by → work_item  

---

## 9. Cross-Layer Links

Change:
plan → schedules → work_item.*

Testing:
delivery_stream → triggers → test_run.*

Release:
milestone → relates_to → release.*

Risk:
dependency → may_create → risk.*

---

## 10. Boundaries

Does NOT include:

- implementation details  
- runtime monitoring  
- compliance logic  

---

## 11. Minimal Content

- 1 plan  
- 1 work item  

---

## 12. Completeness

- priorities defined  
- dependencies mapped  
- delivery flow clear  
- capacity considered  

---

## 13. Summary

Defines how work is **planned, prioritized, and executed through structured delivery flows over time**.

<!-- AISMM:END -->
