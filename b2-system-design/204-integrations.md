<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 204
layer_key: integrations
document_id: spec.integrations
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Integrations Layer Specification
<!-- AISMM:META_END -->

# 204 — Integrations

## 1. Purpose of the Layer

The **Integrations** layer defines the **actual connections and communication between systems using APIs, protocols, and transport mechanisms**.

It describes:
- which systems are connected
- how APIs are used in real interactions
- communication protocols and channels
- interaction patterns (synchronous / asynchronous)
- data exchange flows

It answers:
- Which systems communicate with each other?
- How are APIs actually used in practice?
- Through what protocols and channels does communication happen?

This layer defines **real-world interaction and communication between systems**.

---

## 2. Layer Role in AISMM

This layer connects:

```text
APIs → External / Distributed Systems
```

It extends internal interfaces into **real-world communication**.

Other layers rely on it to:

- execute cross-system processes
- enable external functionality
- support data exchange
- define system boundaries in practice

This layer does NOT define:

- API structure (see 203)
- internal system decomposition
- data modeling
- infrastructure deployment

It defines **interaction between systems**.

---

## 3. Main Output of the Layer

The output is a structured model of:

- integrations
- external systems
- communication channels
- protocols
- messages and events
- interaction patterns

---

## 4. Core Concepts

### 4.1 Integration

An integration defines a connection between systems.

Examples:

```text
integration.payment_provider
integration.crm_sync
```

---

### 4.2 External System

Represents an external or third-party system.

---

### 4.3 Communication Pattern

Defines how systems interact:

- synchronous (request/response)
- asynchronous (events, queues)
- batch processing

---

### 4.4 Protocol

Defines how communication happens:

- HTTP / REST
- gRPC
- messaging (Kafka, RabbitMQ)
- file transfer
- webhooks

---

### 4.5 Message / Event

Defines the payload exchanged between systems.

---

### 4.6 Channel

Defines transport medium:

- API endpoint
- queue
- topic
- webhook
- file exchange

---

## 5. Identifiable Entities

| Entity Type     | Identifier Prefix   |
| :-------------- | :------------------ |
| Integration     | `integration.*`     |
| External System | `external_system.*` |
| Message         | `message.*`         |
| Event           | `event.*`           |
| Protocol        | `protocol.*`        |
| Channel         | `channel.*`         |

---

## 6. Required Content Structure

---

### 6.1 Integration Definitions

Each integration must include:

- identifier
- description
- connected systems
- purpose

---

### 6.2 External Systems

Define:

- system description
- ownership (internal/external)
- responsibilities

---

### 6.3 Communication Patterns

Define:

- sync / async
- request-response or event-driven
- reliability expectations

---

### 6.4 Protocols

Define:

- communication protocol
- constraints

---

### 6.5 Messages / Events

Define:

- payload structure
- meaning
- source and destination

---

### 6.6 Channels

Define:

- communication medium
- endpoints / topics / queues

---

## 7. Preferred Representation

The semantic content of this layer is independent of any specific representation format.

This layer defines **system interaction patterns**, not how they must be stored.

Recommended representations:

- AsyncAPI for event-driven integrations  
- OpenAPI for synchronous integrations  
- Markdown (`.md`) for documentation  
- Diagrams for integration flows  

Implementations:

- SHOULD use AsyncAPI for messaging  
- SHOULD use OpenAPI for HTTP integrations  
- MAY use diagrams  
- MUST NOT depend on vendor-specific formats  

---

## 8. Relationships Inside the Layer

```text
integration → connects → system
integration → uses → protocol
integration → exchanges → message
integration → uses → channel
event → transported_via → channel
```

---

## 9. Relationships With Other AISMM Layers

### API Layer

```text
integration → uses → api.*
```

---

### System Architecture

```text
integration → connects → system.*
```

---

### Processes

```text
integration → triggered_by → process.*
integration → triggers → process.*
```

---

### Data Architecture

```text
message → contains → entity.*
```

---

### Economics

```text
integration → generates → cost.*
integration → affects → revenue.*
```

---

## 10. Layer Boundaries

This layer must not include:

- API internal structure
- system decomposition
- data modeling
- infrastructure deployment

---

## 11. Recommended Block Types

- layer_document
- integration_definition
- message_definition
- event_definition

---

## 12. Minimal Valid Content

Must define:

- at least one integration
- at least one external system

---

## 13. Completeness Criteria

A mature model includes:

- all integrations
- clear communication patterns
- defined protocols
- mapped data exchange
- reliability considerations

---

## 14. Example Identifiers

```text
integration.payment_gateway
external_system.stripe
event.payment_completed
message.payment_request
channel.webhook_payment
```

---

## 15. Summary

This layer defines **how systems connect and communicate across boundaries**.

It completes the system design by extending internal logic into real-world integrations.

<!-- AISMM:END -->
