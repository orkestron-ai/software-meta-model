<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 204
layer_key: integrations
document_id: spec.integrations
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.1.0
title: Integrations Layer Specification
<!-- AISMM:META_END -->

# 204 — Integrations

## 1. Purpose of the Layer

The **Integrations** layer defines how systems are **actually connected and communicate in real-world environments**.

It describes:

- system-to-system connections
- external and internal integrations
- protocols and transport mechanisms
- communication channels
- real message and event exchange
- reliability and delivery guarantees

It answers:

- Which systems communicate?
- How do they communicate in practice?
- Through which protocols and channels?
- What guarantees exist (delivery, retries, failures)?
- How do integrations behave under real conditions?

This layer defines **runtime interaction between systems**, not just contracts.

---

## 2. Layer Role in AISMM

This layer connects:

API Contracts → Real Communication → Distributed Execution

IMPORTANT:

API (203) ≠ Integration (204)

- API = what can be done  
- Integration = how it is actually connected and executed  

This layer extends:

- APIs → into real network interactions  
- events → into real transport channels  
- processes → into cross-system execution  

Other layers rely on it to:

- execute distributed processes
- connect external systems
- move data across boundaries
- implement real system topology

---

## 3. Main Output

- integrations
- connected systems
- protocols
- channels
- messages and events
- communication patterns
- reliability rules
- retry / failure handling

---

## 4. Core Concepts

### 4.1 Integration
Connection between systems.

---

### 4.2 External System
Third-party or separate system.

---

### 4.3 Communication Pattern
Defines execution style:

- synchronous (request/response)
- asynchronous (event-driven)
- streaming
- batch

---

### 4.4 Protocol
Defines communication mechanism:

- HTTP / REST
- gRPC
- messaging (Kafka, RabbitMQ)
- file transfer
- webhook

---

### 4.5 Channel (ENHANCED)
Transport medium:

- endpoint
- queue
- topic
- webhook
- file exchange

---

### 4.6 Message / Event (ENHANCED)
Actual payload exchanged.

Includes:

- structure
- meaning
- lifecycle

---

### 4.7 Reliability Model (NEW)
Defines:

- delivery guarantees (at-most-once, at-least-once, exactly-once)
- retry policies
- timeout behavior
- fallback strategies

---

### 4.8 Integration Topology (NEW)
Defines:

- point-to-point
- hub-and-spoke
- event bus
- mesh

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Integration | integration.* |
| External System | external_system.* |
| Message | message.* |
| Event | event.* |
| Protocol | protocol.* |
| Channel | channel.* |

---

## 6. Required Structure

### 6.1 Integrations
- id
- purpose
- connected systems
- topology

---

### 6.2 Systems
- description
- ownership
- role

---

### 6.3 Communication Patterns
- type
- interaction style

---

### 6.4 Protocols
- protocol
- constraints

---

### 6.5 Channels
- type
- endpoints / topics / queues

---

### 6.6 Messages / Events
- payload
- source
- destination

---

### 6.7 Reliability
- delivery guarantees
- retry policy
- failure handling

---

## 7. Preferred Representation

PRIMARY:

- AsyncAPI (events)
- OpenAPI (sync)

ADDITIONAL:

- diagrams (topology)
- Markdown

---

## 8. Relationships

integration → connects → system  
integration → uses → protocol  
integration → uses → channel  
integration → exchanges → message  
event → transported_via → channel  

---

## 9. Cross-Layer Links

API:
integration → uses → api.*

System Architecture:
integration → connects → component.*

Processes:
integration → executes → process.*

Data:
message → contains → entity.*

Critical Path:
integration → affects → step.*

Economics:
integration → generates → cost.*
integration → impacts → revenue.*

---

## 10. Boundaries

Does NOT include:

- API contracts (203)
- internal system structure (201)
- data modeling (202)
- infra configuration (301)

---

## 11. Minimal Content

- 1 integration
- 1 external system

---

## 12. Completeness

- full integration map
- protocols defined
- channels defined
- reliability defined
- topology defined

---

## 13. Summary

Defines how systems are **actually connected and communicate in distributed environments**.

<!-- AISMM:END -->
