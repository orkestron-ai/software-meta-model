# AI-driven Software Meta-Model (AISMM)

## Overview

**AISMM (AI-driven Software Meta-Model)** is a unified, machine-readable and human-navigable framework for describing, designing, building, and evolving software systems.

It provides:

- A **formal structure** for all aspects of a product
- A **graph-based model** connecting business, product, and technical layers
- A **repository-native format** compatible with Git workflows
- A foundation for **AI-driven software engineering**

AISMM is not just a documentation approach — it is a **meta-operating system for software development**.

---

## Core Principle

> A software system can be fully described as interactions between **Actors**, **Views**, **Actions**, and **Models**, producing **Outcomes** that are evaluated as **Value**.

---

## Core Interaction Model (Bundle 0)

### Model
Entities, attributes, relationships, states, rules.

### View
UI, projections, visibility, navigation.

### Action
Executable, interaction, and composite actions.

### Actors / Roles / Permissions
Who can do what.

### State Transitions
How system changes.

### Outcomes
Results of actions.

---

## Core Chain

Actor → View → Action → Model → State Transition → Outcome → Value

---

## AISMM Structure

### Bundle 0 — Core Interaction Model

### Bundle 1 — Value and Motivation View
Value streams, context, stakeholders, strategy, hypotheses, economics.

### Bundle 2 — Business Structure View
Business architecture, processes (BPMN), critical path.

### Bundle 3 — Product Behavior View
Requirements, rules, user scenarios.

### Bundle 4 — System Structure View
Architecture, API, integrations, data.

### Bundle 5 — Implementation and Technology View
Technology, code.

### Bundle 6 — Operations and Runtime View
Operations, observability.

### Bundle 7 — Quality and Risk View
Testing, risks.

### Bundle 8 — Change and Knowledge View
Debt, roadmap, tasks, knowledge, traceability.

### Bundle 9 — Policy & Constraints Overlay
Security, performance, NFR, compliance, access.

---

## Repository Structure Example

repo/
  .aismm/
  modules/
    module/.smm/

---

## Formats

- VSS (Value)
- BPMN (Processes)
- OpenAPI (API)
- JSON Schema (Data)
- Gherkin (Tests)
- Markdown (Fallback)

---

## Summary

AISMM = Core (Model/View/Action) + Views (Bundles) + Overlays (Constraints)
