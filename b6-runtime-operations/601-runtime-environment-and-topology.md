<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 601
layer_key: runtime_environment_and_topology
document_id: spec.runtime.environment.topology
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Runtime Environment and Topology Layer Specification
<!-- AISMM:META_END -->

# 601 — Runtime Environment and Topology

## 1. Purpose of the Layer

The **Runtime Environment and Topology** layer defines **how the system exists and operates in real environments**.

It describes:

- runtime instances  
- services and nodes  
- clusters and regions  
- deployment topology  
- network paths and connectivity  

It answers:

- Where is the system running?
- What instances are active?
- How are components distributed?
- How do parts of the system communicate in runtime?

---

## 2. Layer Role in AISMM

This layer connects:

Deployment → Runtime → Real Execution

It transforms:

- deployment units → runtime instances  
- infrastructure → topology  
- components → running services  

Other layers rely on it to:

- understand real system behavior  
- support monitoring (602)  
- support incident response (603)  
- validate SLA/SLO (604)  

---

## 3. Main Output

- runtime instances  
- service instances  
- nodes and clusters  
- regions and zones  
- deployment topology  
- network paths  

---

## 4. Core Concepts

### 4.1 Runtime Instance

Actual running unit of the system.

---

### 4.2 Service Instance

Running instance of a service or component.

---

### 4.3 Node

Execution host:

- VM  
- container host  
- server  

---

### 4.4 Cluster

Group of nodes working together.

---

### 4.5 Region

Geographical location of infrastructure.

---

### 4.6 Zone

Subdivision of region for redundancy.

---

### 4.7 Deployment Topology (CRITICAL)

Describes how system components are distributed across runtime.

---

### 4.8 Network Path (NEW)

Defines communication routes:

- service-to-service  
- external connections  

---

### 4.9 Scaling Model (NEW)

Defines scaling behavior:

- horizontal  
- vertical  
- auto-scaling  

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Runtime Instance | runtime_instance.* |
| Service Instance | service_instance.* |
| Node | node.* |
| Cluster | cluster.* |
| Region | region.* |
| Zone | zone.* |

---

## 6. Required Structure

### 6.1 Runtime Instances
- id  
- deployment unit  
- environment  

---

### 6.2 Services
- service instances  
- mapped components  

---

### 6.3 Nodes
- type  
- capacity  

---

### 6.4 Clusters
- nodes  
- orchestration  

---

### 6.5 Regions / Zones
- location  
- redundancy  

---

### 6.6 Topology
- distribution of services  
- dependencies  

---

### 6.7 Network
- connections  
- protocols  

---

## 7. Preferred Representation

- Markdown  
- infrastructure diagrams  
- Kubernetes manifests  
- Terraform references  

---

## 8. Relationships

runtime_instance → runs_on → node  
node → part_of → cluster  
cluster → located_in → region  
service_instance → deployed_as → runtime_instance  
service_instance → communicates_with → service_instance  

---

## 9. Cross-Layer Links

Deployment:
runtime_instance → created_from → deployment_unit.*

System:
service_instance → implements → component.*

Technology:
node → uses → technology.*

Monitoring:
runtime_instance → observed_by → metric.*

---

## 10. Boundaries

Does NOT include:

- monitoring logic  
- incident handling  
- SLA definitions  

---

## 11. Minimal Content

- 1 runtime instance  
- 1 service  

---

## 12. Completeness

- topology defined  
- runtime mapped  
- network paths described  
- scaling model defined  

---

## 13. Summary

Defines how the system exists and operates as a **real distributed runtime system**.

<!-- AISMM:END -->
