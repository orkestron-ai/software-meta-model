<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 001
layer_key: product_definition_context
document_id: spec.product.definition.context
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Product Definition & Context Layer Specification
<!-- AISMM:META_END -->

# 001 — Product Definition & Context

## 1. Purpose of the Layer

The **Product Definition & Context** layer defines what the product is, where its boundaries are, and in what environment it exists.

This layer is the primary entry point for understanding the product as a distinct system.

It answers the following questions:

- What is the product?
- What is included in the product?
- What is explicitly excluded?
- What external systems, platforms, actors, environments, and organizations surround it?
- What is owned by the product and what is outside its responsibility?
- Which contextual entities must be referenced by other AISMM layers?

---

## 2. Layer Role in AISMM

This layer provides the contextual foundation for all other layers.

Other AISMM layers rely on this layer to understand:

- product identity
- product scope
- product boundaries
- module boundaries
- external systems
- external dependencies
- ownership zones
- contextual assumptions
- environmental constraints

This layer does not define detailed requirements, processes, architecture, UX, code, or economic calculations.  
It defines the **context in which those layers exist**.

---

## 3. Main Output of the Layer

The main output of this layer is a structured product definition containing:

- product identity
- product scope
- out-of-scope boundaries
- contextual entities
- ownership and responsibility boundaries
- assumptions
- constraints
- stable identifiers for cross-layer references

---

## 4. Core Concepts

### 4.1 Product

The product is the main object being described by AISMM.

A product may be:

- a user-facing application
- a platform
- an internal system
- an API product
- a marketplace
- a service
- a product family
- a subsystem treated as an independent product

The product must have a stable identifier.

Example identifier:

```text
product.orkestron_ai
```

---

### 4.2 Product Boundary

A product boundary defines what belongs to the product and what does not.

Boundaries may be:

- functional
- technical
- organizational
- operational
- data-related
- legal or compliance-related
- economic

Boundaries must be explicit enough to prevent ambiguity in later layers.

---

### 4.3 Product Context

Product context describes the environment around the product.

It may include:

- external systems
- adjacent products
- shared platforms
- third-party services
- organizational units
- regulatory environment
- deployment environment
- users and non-user context participants

---

### 4.4 Product Module

A module is a meaningful internal part of the product.

A module may represent:

- business domain
- system component
- bounded context
- service
- UI area
- workflow area
- integration area

Modules defined here provide high-level context only.  
Detailed architecture is described in later layers.

Example identifier:

```text
module.billing
```

---

### 4.5 External System

An external system is outside the product boundary but interacts with, supports, constrains, or affects the product.

Example identifiers:

```text
external.identity_provider
external.sms_gateway
external.payment_processor
```

---

### 4.6 Channel

A channel is a way through which the product is accessed, consumed, or integrated.

Examples:

```text
channel.web_app
channel.public_api
channel.admin_panel
channel.mobile_app
```

---

### 4.7 Environment

An environment is a runtime or organizational context in which the product exists.

Examples:

```text
environment.production
environment.staging
environment.customer_cloud
```

---

### 4.8 Ownership Zone

An ownership zone defines who is responsible for a part of the product context.

Examples:

```text
ownership.product_team
ownership.platform_team
ownership.shared_services
ownership.external_provider
```

---

## 5. Identifiable Entities

This layer should define stable identifiers for the following entity types.

| Entity Type         | Identifier Prefix  | Description                        |
| ------------------- | ------------------ | ---------------------------------- |
| Product             | `product.*`        | Main product entity                |
| Product Module      | `module.*`         | Product-internal module or domain  |
| External System     | `external.*`       | System outside product boundary    |
| Channel             | `channel.*`        | Product access or delivery channel |
| Environment         | `environment.*`    | Runtime or deployment environment  |
| Organization / Team | `org.*` / `team.*` | Organizational context             |
| Ownership Zone      | `ownership.*`      | Responsibility boundary            |
| Boundary Rule       | `boundary.*`       | Inclusion or exclusion rule        |
| Assumption          | `assumption.*`     | Contextual assumption              |
| Constraint          | `constraint.*`     | Contextual limitation              |
| Context Domain      | `context.*`        | Named contextual area              |

Identifiers must be stable and reused by other layers.

---

## 6. Required Content Structure

A Product Definition & Context block should contain the following sections.

---

### 6.1 Product Identity

Defines the product as a distinct entity.

Should include:

- product identifier
- product name
- product type
- short description
- mission or purpose
- product owner or owning organization
- lifecycle status
- repository or source location

Recommended fields:

```text
Product ID:
Name:
Type:
Status:
Owner:
Short Description:
Mission:
Primary Repository:
```

---

### 6.2 Product Scope

Defines what is included in the product.

Should include:

- core capabilities at high level
- included modules
- supported channels
- owned interfaces
- owned data domains
- owned operational responsibilities

This section must not contain detailed requirements.  
It defines inclusion boundaries only.

---

### 6.3 Out of Scope

Defines what is explicitly excluded from the product.

Should include:

- excluded capabilities
- external systems not owned by the product
- organizational responsibilities outside product control
- channels not currently supported
- data or process areas outside the product

Out-of-scope definitions are important because they prevent accidental expansion of the model.

---

### 6.4 Product Modules

Defines high-level product modules or domains.

Each module should include:

- module identifier
- name
- description
- role in product
- ownership
- whether it may have its own `.smm` model

Example:

```text
module.workflow_engine
module.agent_registry
module.billing
```

---

### 6.5 External Context

Defines external entities that surround the product.

Should include:

- external systems
- adjacent products
- shared platforms
- third-party services
- external organizations
- regulatory or market context

Each external entity must have a stable identifier.

---

### 6.6 Channels and Interfaces

Defines major channels through which the product is used or integrated.

Examples:

- web application
- admin interface
- public API
- internal API
- CLI
- integration channel

Detailed API contracts are not described here.  
Only contextual channel identity is defined.

---

### 6.7 Environments

Defines environments relevant for understanding product boundaries.

Examples:

- production
- staging
- development
- customer-hosted
- cloud region
- on-premise deployment

Detailed infrastructure is described in system design and operations layers.

---

### 6.8 Ownership and Responsibility Boundaries

Defines who owns what.

Should clarify:

- what is owned by the product team
- what is shared with other teams
- what is owned by platform teams
- what is owned by external providers
- what is customer responsibility

---

### 6.9 Boundary Rules

Defines explicit rules for deciding whether something belongs to the product.

Examples:

```text
boundary.api_owned_by_product
boundary.external_identity_provider_not_owned
boundary.mobile_app_out_of_scope
```

A boundary rule should define:

- rule identifier
- statement
- rationale
- affected entities
- consequences for other layers

---

### 6.10 Assumptions

Defines contextual assumptions that other layers may depend on.

Examples:

```text
assumption.shared_identity_provider_available
assumption.product_used_by_enterprise_clients
assumption.multi_repository_landscape
```

Assumptions may later be validated, invalidated, or linked to risks.

---

### 6.11 Constraints

Defines contextual constraints that limit product design or evolution.

Examples:

```text
constraint.eu_data_residency
constraint.no_native_mobile_client
constraint.must_use_shared_billing_platform
```

Detailed security, compliance, and performance constraints may be expanded in later layers.

---

## 7. Preferred Representation

The semantic content of this layer is independent of any specific representation format.

This layer defines **what must be described**, not how it must be stored or visualized.

However, for consistency, usability, and interoperability, the following representations are considered most suitable for this layer:

- Markdown (`.md`) for narrative structure and contextual descriptions
- Structured formats (JSON / YAML) for machine-readable product metadata
- Simple diagrams or tabular representations for visualizing boundaries, modules, and context relationships

These representations are **recommendations, not requirements**.

Implementations:

- SHOULD use the recommended formats for clarity and consistency
- MAY use alternative formats if they preserve semantic meaning
- MUST NOT encode semantics in format-specific constructs

The correctness of this layer is determined исключительно by the completeness and consistency of its **semantic content**, not by the chosen representation format.

---

## 8. Relationships Inside the Layer

This layer should describe relationships such as:

```text
product → contains → module
product → exposes → channel
product → depends_on → external_system
module → owned_by → ownership_zone
channel → uses → interface
environment → constrains → product
boundary_rule → applies_to → entity
assumption → supports → product_scope
constraint → limits → product_scope
```

These relationships may be written in human-readable text, structured tables, or extracted into graph.

---

## 9. Relationships With Other AISMM Layers

### 9.1 Value Streams Layer

Product modules and external systems may be mapped to value converters or value context nodes.

```text
module.* → VSS.converter
external.* → VSS.external_converter
```

---

### 9.2 Stakeholders and Motivation Layer

Stakeholders refer to product context entities, channels, ownership zones, and external participants.

```text
stakeholder.* → product.*
stakeholder.* → channel.*
stakeholder.* → external.*
```

---

### 9.3 Business Architecture Layer

Business domains and capabilities are grounded in product modules and boundaries defined here.

```text
business_domain.* → module.*
capability.* → module.*
```

---

### 9.4 Critical Path Layer

Critical path steps should use product modules, channels, and external systems defined here.

```text
critical_step.* → module.*
critical_step.* → external.*
```

---

### 9.5 System Design Layer

Applications, services, data stores, APIs, and integrations must map back to product modules and external systems.

```text
application.* → module.*
integration.* → external.*
api.* → channel.*
```

---

### 9.6 Product Behavior Layer

Requirements and controls must reference product scope, modules, channels, and boundary rules.

```text
requirement.* → module.*
control.* → module.*
control.* → boundary.*
```

---

### 9.7 Operations and Runtime Layer

Runtime environments, ownership zones, and operational responsibilities originate here and are refined later.

```text
runtime_environment.* → environment.*
operational_responsibility.* → ownership.*
```

---

### 9.8 Knowledge and Traceability Layer

All identifiers defined here become valid targets for cross-layer traceability.

---

## 10. Layer Boundaries

This layer should not include:

- detailed business processes
- detailed requirements
- detailed UI flows
- API specifications
- database models
- implementation details
- release plans
- runtime procedures
- financial formulas

Those belong to other layers.

This layer may reference them only at a high level.

---

## 11. Recommended Block Types

The layer may contain the following AISMM block types:

- `layer_document`
- `entity_definition`
- `boundary_rule`
- `assumption_definition`
- `constraint_definition`
- `schema_reference`

The default representation is Markdown.  
Structured JSON/YAML may be referenced if needed, but the layer is defined by its semantic content, not by a specific file format.

---

## 12. Minimal Valid Content

A minimal valid Product Definition & Context layer must define:

- one `product.*` entity
- product name
- product type
- short description
- in-scope summary
- out-of-scope summary
- at least one ownership statement
- stable identifiers for referenced modules or external systems if mentioned

---

## 13. Recommended Completeness Criteria

A mature version of this layer should define:

- product identity
- product modules
- external systems
- channels
- environments
- ownership zones
- boundary rules
- assumptions
- constraints
- cross-layer references

---

## 14. Example Entity Identifiers

```text
product.orkestron_ai
module.workflow_engine
module.agent_registry
module.billing
external.identity_provider
external.sms_gateway
channel.web_app
channel.public_api
environment.production
ownership.product_team
boundary.mobile_app_out_of_scope
assumption.shared_identity_provider_available
constraint.eu_data_residency
```

---

## 15. Example AISMM Block

```md
<!-- AISMM:BEGIN -->
type: layer_document
layer_id: 001
layer_key: product_definition_context
document_id: product.definition.context
document_type: product_definition
module_scope: root
status: draft
title: Product Definition & Context
<!-- AISMM:META_END -->

# Product Definition & Context

## Product Identity

Product ID: product.example  
Name: Example Product  
Type: SaaS Platform  
Status: draft  

## Product Scope

The product includes workflow orchestration, user management, and reporting.

## Out of Scope

Native mobile applications are outside the current product boundary.

## Product Modules

- module.workflow_engine
- module.reporting

## External Systems

- external.identity_provider

## Boundary Rules

- boundary.mobile_app_out_of_scope

<!-- AISMM:END -->
```

---

## 16. Summary

The Product Definition & Context layer defines the product as a bounded system.

It establishes the stable contextual entities that other AISMM layers use to describe value, behavior, architecture, operations, risks, and change.

<!-- AISMM:END -->
