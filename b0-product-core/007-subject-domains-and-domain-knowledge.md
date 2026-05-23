<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 007
layer_key: subject_domains_and_domain_knowledge
document_id: spec.subject.domains.domain.knowledge
document_type: layer_specification
module_scope: root
status: stable
spec_version: 3.0.0
title: Subject Domains and Domain Knowledge Layer Specification
kind_class: descriptive
<!-- AISMM:META_END -->

# 007 — Subject Domains and Domain Knowledge

## 1. Purpose of the Layer

The **Subject Domains and Domain Knowledge** layer describes the **real-world problem domains the product automates**, independent of how the system is built.

It captures the *subject matter* — the field, its concepts, its processes as they exist in reality, its terminology, and the authoritative external sources that define it.

It answers:

- Which real-world domains does this product automate or operate in?
- What are the core concepts and terminology of each domain, as the domain itself defines them?
- What processes, rules and constraints exist in the domain independently of our system?
- Where is the authoritative external knowledge about this domain (standards, regulations, literature, references)?
- Who are the subject-matter experts (SMEs) for each domain?

This layer is the **problem-space** counterpart of the **solution-space** layers `b2.207` (bounded contexts and domain architecture) and `b4.402` (domain model and business rules).

> Critical distinction (V3): `b0.007` describes the domain **as it exists in reality**. `b2.207` describes **how our system partitions** that reality. `b4.402` describes the **rules our product implements**. `b9.906` is the **canonical term/relationship registry**. These four must not be merged.

---

## 1A. Product Model Representation

In a product repository, this layer is represented by the directory `aismm/b0-product-core/007-subject-domains-and-domain-knowledge/`.

The questions in this specification must be answered through one or more record files inside that directory, not through a single layer file.

Each new semantic unit for this layer must create a new record file whose identity is preserved after creation.

Record naming, file IDs and preferred record kinds for layer `007` are governed by [`../aismm-layer-artifact-naming.md`](../aismm-layer-artifact-naming.md).

Preferred record kinds: `domain`, `term`, `proc`.

---

## 2. Layer Role in AISMM

This layer connects:

Reality (subject domain) → Vocabulary → Solution partitioning → Implemented rules

It is the **semantic anchor** of the product: it explains the world the product acts upon so that humans and AI agents do not have to infer domain meaning from code or UI.

Other layers use it to:

- ground requirements (`b4.401`) and rules (`b4.402`) in real domain meaning
- justify bounded-context boundaries (`b2.207`)
- supply canonical terms to the ontology (`b9.906`)
- give RAG retrieval (`b9.905`) authoritative domain definitions instead of guessed ones
- onboard humans and agents into unfamiliar problem spaces

---

## 3. Main Output

- subject domain definitions
- domain concept models (real-world, not system entities)
- domain-specific terminology (glossary, feeding `b9.906`)
- real-world domain processes and constraints
- authoritative external references (standards, regulations, literature)
- SME ownership pointers (into `b11`)
- `automates` links from domain → system surfaces

---

## 4. Core Concepts

### 4.1 Subject Domain

A bounded area of real-world activity the product automates or supports (for example: live event broadcasting, content moderation, developer-competition operations, payments).

A subject domain is **discovered**, not designed. It would exist even if the product did not.

### 4.2 Domain Concept

A real-world concept inside a domain (for example: "broadcast", "speaker", "stream key", "moderation verdict") described with the domain's own meaning, not the system's table or class.

### 4.3 Domain Terminology

The vocabulary used by domain practitioners. Each term is a candidate canonical term registered in `b9.906`. This layer owns the **meaning in the domain**; `b9.906` owns the **canonical registration**.

### 4.4 Domain Process (real-world)

A process as it happens in reality, independent of our implementation (for example: how a live event is produced end-to-end in the industry). Distinct from `b1.103` (our business processes) and `b8` (our delivery process).

### 4.5 Domain Rule / Domain Constraint (real-world)

A rule that holds in the domain regardless of our system (for example: regulatory broadcast delay, age rating rules). Distinct from `b4.402` business rules, which are the rules *we chose to implement*.

### 4.6 Authoritative Reference

A link to an external source of truth for the domain: standard, regulation, specification, textbook, recognized body. Carries `authorship_class: external_reference` (see `aismm-context-security.md`).

### 4.7 Subject-Matter Expert (SME)

A human authority for the domain. Modeled as a pointer into `b11.1102` (ownership) / `b11.1104` (skills). SMEs are the natural validators for derived domain records (see `aismm-owner-validation-and-attestation.md`).

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Subject domain | subject_domain.* |
| Domain concept | domain_concept.* |
| Domain term | domain_term.* |
| Domain process | domain_process.* |
| Domain rule | domain_rule.* |
| Authoritative reference | authoritative_reference.* |
| SME reference | sme_reference.* |

---

## 6. Required Structure

### 6.1 Subject Domain
- id, name, scope, why it matters to the product
- maturity / stability of domain knowledge
- SME owner reference

### 6.2 Domain Concepts and Terms
- id, name, real-world definition
- synonyms / disambiguation
- canonical term link to `b9.906`

### 6.3 Domain Processes and Rules
- id, description, real-world source
- whether the product automates it (and where)

### 6.4 Authoritative References
- id, title, source, url/file reference
- authority class and freshness

### 6.5 Automation Mapping
- `automates` links to `b2.207` bounded contexts and `b2.201` components
- `realizes_domain_rule` links to `b4.402`

---

## 7. Preferred Representation

- Markdown for narrative domain knowledge
- Glossary tables for terminology
- Concept maps (Mermaid / diagram references) for concept relationships
- `url:` / `file:` references for external authoritative sources

---

## 8. Relationships

subject_domain → contains → domain_concept
domain_concept → named_by → domain_term
domain_term → registered_as → b9.906.term
subject_domain → defines → domain_process
subject_domain → constrained_by → domain_rule
subject_domain → documented_by → authoritative_reference
subject_domain → owned_by → sme_reference (b11)
subject_domain → automated_by → b2.207.bounded_context
domain_rule → realized_by → b4.402.rule

(`automates` / `realizes_domain` / `registered_as` are added to the canonical taxonomy in `b9.906`.)

---

## 9. Cross-Layer Links

- `b9.906` — canonical terms and relationship taxonomy (this layer feeds it)
- `b2.207` — bounded contexts that automate domains
- `b4.402` — implemented business rules realizing domain rules
- `b4.401` — requirements grounded in domain concepts
- `b11.1102` / `b11.1104` — SME ownership and skills
- `b9.905` — RAG uses this layer as the authoritative domain source

---

## 10. Boundaries

Does NOT include:

- system entities, tables, classes (that is `b2.202` / `b4.402`)
- our internal business processes (that is `b1.103`)
- system partitioning (that is `b2.207`)
- the canonical term registry mechanics (that is `b9.906`)

This layer is **knowledge about the world**, not knowledge about the system.

---

## 11. Minimal Valid Content

- at least one subject domain with scope
- core terminology for that domain
- at least one authoritative reference or an explicit gap note
- one `automated_by` link to a system surface

---

## 12. Completeness Criteria

- all domains the product automates are listed
- each domain has terminology linked into `b9.906`
- each domain has authoritative references or declared knowledge gaps
- automation mapping connects every domain to at least one solution-space surface
- SME owner assigned per domain (enables owner validation)

---

## 13. Summary

This layer answers **"what real-world domains does this product automate, and what do they actually mean?"** It separates problem-space domain knowledge from solution-space architecture, gives agents authoritative domain grounding, and feeds canonical terminology into the knowledge bundle.

<!-- AISMM:END -->
