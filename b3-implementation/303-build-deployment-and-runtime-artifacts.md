<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 303
layer_key: build_deployment_runtime_artifacts
document_id: spec.build.deployment.runtime
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Build, Deployment and Runtime Artifacts Layer Specification
<!-- AISMM:META_END -->

# 303 — Build, Deployment and Runtime Artifacts

## 1. Purpose of the Layer

The **Build, Deployment and Runtime Artifacts** layer defines how code is transformed into **runnable artifacts** and how those artifacts are delivered into execution environments.

It describes:

- build processes
- artifact generation
- packaging formats
- deployment units
- runtime instances
- CI/CD pipelines (as structure, not operations)

It answers:

- How does code become a runnable system?
- What artifacts are produced?
- How are they packaged?
- How are they deployed into environments?
- What exactly runs in production?

---

## 2. Layer Role in AISMM

This layer connects:

Code → Artifact → Deployment → Runtime

It transforms:

- code → build artifacts  
- artifacts → deployment units  
- deployment → runtime instances  

Other layers rely on it to:

- understand delivery pipeline  
- map runtime to architecture (201)  
- connect to infrastructure (301)  

IMPORTANT:

This layer does NOT describe:
- monitoring
- operations
- SLA / SLO
(these belong to operational bundles)

---

## 3. Main Output

- build definitions  
- artifacts  
- packaging formats  
- deployment units  
- runtime instances  
- pipeline structure  

---

## 4. Core Concepts

### 4.1 Build Process
Defines how code is compiled or assembled.

---

### 4.2 Artifact
Output of build:

- binary  
- container image  
- bundle  

---

### 4.3 Packaging
Defines how artifacts are structured:

- Docker image  
- archive  
- executable  

---

### 4.4 Deployment Unit
Deployable entity:

- container  
- service instance  
- job  

---

### 4.5 Runtime Instance
Actual running instance.

---

### 4.6 Pipeline (NEW)
Defines build + deploy flow:

- stages  
- triggers  
- dependencies  

---

### 4.7 Environment (NEW)
Defines where system runs:

- dev  
- staging  
- production  

---

### 4.8 Versioning (NEW)
Defines artifact lifecycle:

- versions  
- tags  
- releases  

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Build | build.* |
| Artifact | artifact.* |
| Package | package.* |
| Deployment Unit | deployment_unit.* |
| Runtime Instance | runtime_instance.* |
| Pipeline | pipeline.* |
| Environment | env.* |

---

## 6. Required Structure

### 6.1 Build
- id  
- input code  
- output artifact  

---

### 6.2 Artifacts
- id  
- type  
- version  

---

### 6.3 Packaging
- format  
- structure  

---

### 6.4 Deployment
- unit  
- target environment  

---

### 6.5 Runtime
- instances  
- scaling model  

---

### 6.6 Pipeline
- stages  
- triggers  
- dependencies  

---

### 6.7 Environments
- name  
- purpose  

---

## 7. Preferred Representation

- CI/CD configs (GitHub Actions, GitLab CI, etc.)  
- Dockerfiles  
- Kubernetes manifests  
- Terraform references  
- Markdown for overview  

---

## 8. Relationships

build → produces → artifact  
artifact → packaged_as → package  
package → deployed_as → deployment_unit  
deployment_unit → runs_as → runtime_instance  
pipeline → executes → build  

---

## 9. Cross-Layer Links

Code:
artifact → built_from → module.*

Technology:
deployment_unit → runs_on → runtime.*

System Architecture:
deployment_unit → hosts → component.*

Integrations:
runtime_instance → communicates_via → integration.*

---

## 10. Boundaries

Does NOT include:

- business logic  
- system design  
- monitoring and operations  

---

## 11. Minimal Content

- 1 artifact  
- 1 deployment unit  

---

## 12. Completeness

- full build chain  
- artifact versions  
- deployment mapping  
- runtime definition  

---

## 13. Summary

Defines how code becomes a **running system in real environments**.

<!-- AISMM:END -->
