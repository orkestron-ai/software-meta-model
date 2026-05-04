<!-- AISMM:BEGIN -->
type: layer_specification
layer_id: 905
layer_key: context_retrieval_and_rag
document_id: spec.context.retrieval.rag
document_type: layer_specification
module_scope: root
status: stable
spec_version: 1.0.0
title: Context Retrieval and RAG Layer Specification
<!-- AISMM:META_END -->

# 905 — Context Retrieval and RAG

## 1. Purpose of the Layer

The **Context Retrieval and RAG** layer defines **how AISMM knowledge is prepared, retrieved, ranked, assembled, and delivered as context for humans and AI agents**.

It complements 901.

- 901 defines how knowledge is indexed and navigated.
- 905 defines how relevant context is retrieved and assembled for reasoning.

It answers:

- What units of knowledge are retrievable?
- How is content chunked and embedded?
- How is relevant context selected?
- How is retrieval quality measured?
- How do agents receive safe and sufficient context?

---

## 2. Layer Role in AISMM

This layer connects:

Knowledge Index → Retrieval → Context Assembly → Reasoning

It transforms:

- documents and entities → retrieval units
- retrieval units → ranked context
- ranked context → agent-ready context package

Other layers rely on it to:

- support AI agents
- enable RAG workflows
- reduce hallucination risk
- improve context completeness
- make AISMM usable at scale

---

## 3. Main Output

- retrieval units
- chunking strategies
- embedding indexes
- retrieval profiles
- context packages
- relevance signals
- retrieval quality metrics

---

## 4. Core Concepts

### 4.1 Retrieval Unit (CRITICAL)

Atomic retrievable unit of knowledge.

It may represent:

- document section
- entity definition
- decision summary
- trace path
- requirement
- code reference
- release summary

A retrieval unit MUST preserve enough metadata to reconnect retrieved text to AISMM entities.

---

### 4.2 Chunking Strategy

Defines how source content is split into retrieval units.

Chunking may be:

- document-based
- section-based
- entity-based
- graph-neighborhood-based
- summary-based

---

### 4.3 Embedding Record

Vector representation of a retrieval unit.

It MUST reference:

- source retrieval unit
- embedding model
- vector store
- index version

---

### 4.4 Retrieval Profile

Configuration for a retrieval scenario.

Examples:

- agent task execution
- impact analysis
- onboarding
- debugging
- audit
- release preparation

---

### 4.5 Retrieval Query

A structured request for context.

It may include:

- natural language query
- entity references
- bundle/layer scope
- confidence threshold
- freshness requirement
- max context size

---

### 4.6 Context Package (CRITICAL)

Final assembled context delivered to an agent or human.

It includes:

- selected retrieval units
- relevance ranking
- source references
- confidence indicators
- omitted context notes
- unresolved gaps

---

### 4.7 Relevance Signal

Evidence that a retrieval unit is relevant.

Examples:

- semantic similarity
- graph distance
- entity match
- source confidence
- business value
- recency of related change
- retention score

---

### 4.8 Context Budget

Limit on context size.

It defines:

- max tokens
- max retrieval units
- priority rules
- compression strategy

---

### 4.9 Retrieval Quality Metric

Measures quality of retrieval.

Examples:

- precision
- recall
- coverage
- source confidence
- answer supportability
- gap rate

---

## 5. Identifiable Entities

| Entity | Prefix |
|--------|--------|
| Retrieval Unit | retrieval_unit.* |
| Chunking Strategy | chunking_strategy.* |
| Embedding Record | embedding_record.* |
| Retrieval Profile | retrieval_profile.* |
| Retrieval Query | retrieval_query.* |
| Context Package | context_package.* |
| Relevance Signal | relevance_signal.* |
| Context Budget | context_budget.* |
| Retrieval Metric | retrieval_metric.* |

---

## 6. Required Structure

### 6.1 Retrieval Units

Each retrieval unit SHOULD include:

- id
- source reference
- source entity references
- text or compact summary
- semantic scope
- bundle/layer references
- provenance reference
- confidence reference
- retention score reference

---

### 6.2 Chunking Strategies

Each strategy SHOULD include:

- id
- source type
- chunking method
- max size
- overlap rule
- semantic boundary rule

---

### 6.3 Embedding Records

Each embedding record SHOULD include:

- retrieval unit
- embedding model
- vector store
- index version
- created_at
- refresh rule

---

### 6.4 Retrieval Profiles

Each profile SHOULD include:

- scenario
- allowed sources
- ranking rules
- filters
- context budget
- required confidence level

---

### 6.5 Context Packages

Each context package SHOULD include:

- query
- selected retrieval units
- ranking explanation
- source references
- confidence summary
- missing context
- consistency warnings

---

## 7. Preferred Representation

PRIMARY:

- JSON/YAML retrieval registry
- vector database index metadata
- Markdown for retrieval policy

OPTIONAL:

- Chroma / Qdrant / LanceDB metadata
- graph database references
- search engine index metadata

---

## 8. Relationships

retrieval_unit → derived_from → source  
retrieval_unit → represents → entity  
embedding_record → embeds → retrieval_unit  
retrieval_profile → configures → retrieval_query  
retrieval_query → produces → context_package  
context_package → contains → retrieval_unit  
relevance_signal → ranks → retrieval_unit  

---

## 9. Cross-Layer Links

Knowledge Index:
retrieval_unit → indexed_by → knowledge_index.*

Traceability:
retrieval_unit → linked_to → trace_node.*
retrieval_query → may_expand_via → trace_path.*

Provenance:
retrieval_unit → derived_from → source.*
context_package → includes → evidence.*

Coverage:
context_package → reports → missing_context.*
context_package → warns_about → inconsistency.*

Change Execution:
retrieval_unit → may_summarize → work_item.*
retrieval_unit → may_summarize → decision.*

Value:
relevance_signal → may_use → value.*
relevance_signal → may_use → retention_score.*

---

## 10. Boundaries

Does NOT include:

- creation of original AISMM content
- business logic
- agent execution workflow
- model provider configuration

---

## 11. Minimal Content

- 1 retrieval unit
- 1 retrieval profile
- 1 context package format

---

## 12. Completeness

The layer is complete when:

- all important AISMM knowledge can be retrieved
- retrieval units are linked to source entities
- confidence and provenance are available
- context packages expose missing context and uncertainty
- retrieval quality can be measured

---

## 13. Summary

Defines how AISMM knowledge becomes **retrievable, rankable, and usable as trusted context for AI agents and humans**.

<!-- AISMM:END -->
