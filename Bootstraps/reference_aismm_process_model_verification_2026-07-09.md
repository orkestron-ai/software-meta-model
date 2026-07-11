# AISMM Canonical Process Model Verification Bootstrap

## Purpose

This bootstrap defines the **standard pass for verifying a business/process model against the codebase and other sources**, and for recording the verified result into the metamodel.

Use it when:
- A process tree or business-process model exists (in any form) and needs to be reconciled with the metamodel
- A process or process tree needs to be checked for whether it still has a real implementation
- Documentation and task-tracker signals need to be correlated with processes and code
- Metamodel layers need enrichment from verified process data

This bootstrap is agnostic to which specific process-modeling tool, documentation system, or task tracker a product uses. Concrete tool names below (e.g. Confluence, Jira) are **examples only** — substitute whatever the product actually uses.

---

## Mandatory Preflight

Before starting, the agent must:

1. Confirm the target process (or process tree root) is identified — by ID, code, name, or however the source process model identifies it.
2. Locate existing metamodel files for this process/domain to avoid duplication.
3. Determine which process source is available, in this order of preference:
   - **A dedicated process-modeling system reachable via MCP or another tool integration** (e.g. a BPMN/process-modeling platform) — use it as described below.
   - **A dedicated process-modeling system without tool integration** — read it through whatever manual/export mechanism is available (exports, screenshots, direct access).
   - **No dedicated process source** — reconstruct the process shape from whatever is available: code, documentation sources (e.g. Confluence-like wikis), task-tracker items (e.g. Jira-like trackers), or direct stakeholder input.
4. Regardless of which source tier applies, **code verification (Phase 2) and cross-source validation (Phase 4) are mandatory** — a process model may never be recorded as verified from a single source alone.

---

## Canonical State Flow

> **CRITICAL ORDER — do not skip or reorder phases:**
> 1. **Phase 1: Process Analysis & Source Extraction** — traverse the full process tree (or reconstruct it from available sources), collect all references to external documentation/tickets, gather functional objects, performers, diagrams
> 2. **Phase 2: Code Verification** — for every process from Phase 1, search the codebase. Confirm which processes have real implementations and which do not
> 3. **Phase 3: Mark Unverified** — for every process that Phase 2 could NOT confirm with code, mark it as unverified/unused in the source process model if that model supports annotation, and record the finding in the metamodel regardless
> 4. **Phase 4: External Source Correlation** — read documentation and task-tracker sources, correlate with processes and code, persist details into the metamodel, create tasks for mismatches
> 5. **Phase 5: Metamodel Writing** — write/update AISMM metamodel files based on verified data and available external content

**You must complete Phase 1 entirely before starting Phase 2.**
**You must complete Phase 2 entirely before starting Phase 3.**
**You must complete Phase 3 entirely before starting Phase 4.**
**Do NOT mark a process as unverified/unused until you have searched the codebase for it.**
**Phase 4 produces ONE task per process tree covering all correlated external references — not one task per reference.**

---

## Phase 1: Process Analysis & Source Extraction

**Goal:** Build a complete inventory of all processes in scope and extract all references to external sources. Do NOT look at code yet.

### 1.1 Get the root process

If a process-modeling tool is available (ideally via MCP), fetch the root process/entity by its identifier. Otherwise, start from whatever artifact names the process (a document, a ticket, a stakeholder statement).

Extract, when available: identifier, name, code, parent reference, short/long description, external links, classification.

### 1.2 Get the full process tree

If a process-modeling tool is available, fetch the tree to full depth. If a full-depth fetch times out or is unsupported, fetch shallow and iteratively expand each child.

If no dedicated process-modeling tool is available, reconstruct the tree structure from documentation, code structure, or task-tracker epics/sub-tasks, and record explicitly that the tree is reconstructed rather than sourced from a dedicated tool.

For each process in the tree, collect: name/code, identifier, parent reference, description fields, expected result, external links, depth level.

### 1.3 Extract external-source references from ALL processes at ALL depths (MANDATORY)

For each process in the tree (root through deepest leaf), scan every description/result/link field for references to:
- **Documentation sources** (e.g. Confluence, a wiki, a knowledge base, a shared drive of specs) — page IDs, URLs, or plain-text mentions
- **Task trackers** (e.g. Jira, GitLab/GitHub issues, a ticketing system) — ticket IDs, URLs, or plain-text mentions
- Any other external reference the source system embeds (design documents, diagrams, related systems)

Record all findings. This data goes into the traceability layer (b9.902).

**Anti-pattern (DO NOT DO):**
- Check only the root and depth-1 processes
- Write "no external references found" without having checked every process
- Skip description/result fields because they look like metadata — external references are often embedded there

### 1.4 Determine process notation (if applicable)

If the source tool models process notation (e.g. BPMN vs. a legacy/other notation), determine which notation each process uses before extracting diagrams — different notations may require different extraction handling.

### 1.5 Get functional objects, performers, diagrams (if applicable)

Collect, when the source model exposes them: functional/data objects touched by the process with their operations (create/read/update/delete), the org units/roles/actors performing the process, and any diagrams. For diagram content, fetch full diagram content when the tool supports it.

---

## Phase 2: Code Verification

**Goal:** For every process identified in Phase 1, determine whether code exists that implements it.

**Prerequisite:** Phase 1 must be fully complete.

### 2.1 Search for code implementation

For each process (or group of related processes) from Phase 1, search the codebase for the handlers, services, entities, event listeners, or commands that would implement it.

For each process, record one of:
- `verified` — code implementation found (note file paths)
- `unverified` — no code implementation found

### 2.2 Identify contradictions

Compare the process model with code:
- The process model describes a sub-process that doesn't exist in code
- Code implements behavior not reflected in the process model
- The process model describes different behavior than the code
- The process model references deprecated/removed functionality

### 2.3 Compile the unverified list

Before moving to Phase 3, compile a complete list of all `unverified` processes. Review each one — some may be conceptual (parent containers) and should not be marked unused. Mark as unused only processes that claim to implement specific functionality but have no code.

---

## Phase 3: Mark Unverified Processes

**Goal:** Record processes that have no code implementation.

**Prerequisite:** Phase 2 must be fully complete.

### 3.1 Mark each unverified process

If the source process-modeling tool supports annotation/editing, mark each unverified process from Phase 2.3 (e.g. with an `[UNUSED]`-style prefix and a dated, attributed note explaining the basis: "no code implementation found"). If the source tool does not support annotation, record the finding directly in the metamodel instead.

### 3.2 Verify marks

After marking, verify that:
- Every marked process genuinely has no code
- No verified processes were marked unused
- Parent processes are not marked unused if their children are active

---

## Phase 4: External Source Correlation & Metamodel Enrichment

**Goal:** Read documentation and task-tracker sources, correlate content with processes and code, persist details into the metamodel, and create tasks for any mismatches or contradictions.

**Prerequisite:** Phase 3 must be fully complete.

### 4.1 Compile external references from Phase 1

Using the documentation/tracker references extracted in Phase 1, create a list of unique external items to read.

### 4.2 Read each external item

For each item, read its content via the appropriate connector/MCP tool for that source system when available; fall back to a generic web fetch or manual review otherwise.

Record result: `ready` (content retrieved), `blocked` (authentication/permissions required), or `not_found` (item doesn't exist or reference is invalid).

### 4.3 Correlate external content with processes and code

For each accessible item, classify: `matched`, `code-mismatch`, `process-missing`, `unused-process`, `contradiction`, `blocked`, or `not_found`.

### 4.4 Persist details into the metamodel

For items with `ready` status, extract and persist relevant content:

| External content | Target layer |
|-------------------|--------------|
| Functional requirements, API specs, acceptance criteria | b4.401 (Requirements) |
| Business rules, constraints, validation logic | b4.402 (Domain rules) |
| User flows, edge cases, error handling | b5.501 (User scenarios) |
| Security requirements, compliance, PII | b7.703 (Security) |
| Integration details, component architecture | b2.201 (System architecture) |
| Entity descriptions, data flows | b2.202 (Data architecture) |

### 4.5 Create tasks for mismatches and contradictions

For any item classified as `code-mismatch`, `process-missing`, `unused-process`, or `contradiction`, create a tracking task in the traceability layer (b9.902).

---

## Phase 5: Metamodel Writing

### 5.1 Determine which layers to write

| Layer | Write when |
|-------|-----------|
| b0.002 | Value stream: channels, triggers, outcomes |
| b1.103 | Process: hierarchy, performers, functional objects |
| b2.201 | System architecture: components, integrations |
| b2.202 | Data architecture: entities, storage, flows |
| b4.401 | Requirements: API endpoints, business rules |
| b4.402 | Domain rules: validation, constraints, invariants |
| b4.405 | State machines: states, transitions, triggers |
| b5.501 | User scenarios: flows, edge cases, error paths |
| b7.703 | Security: auth, encryption, PII, compliance |
| b9.902 | Traceability: process-to-code mapping, contradictions, external references |
| b9.903 | Source provenance: file paths, line numbers, process source identifiers |

### 5.2 File naming convention

Follow `../aismm-layer-artifact-naming.md`: `<layer-prefix>-<id>-<subject>.md`, one artifact file per minimal semantic unit.

### 5.3 YAML header

Follow the standard AISMM record metadata block; include the source process identifier(s) under a `source_ref` (or equivalent) field so the binding back to the process source is traceable.

---

## Rules

- **Phase order is mandatory**: Phase 1 → Phase 2 → Phase 3 → Phase 4 → Phase 5. Never reorder.
- Never start Phase 2 (code search) until Phase 1 is fully complete.
- Never mark a process unverified/unused in Phase 3 until Phase 2 confirms no code exists.
- Always traverse to the deepest level — never stop at depth 1.
- Always extract external-source references from all processes at all depths (Phase 1).
- Always verify processes against code (Phase 2) — a process source alone, however authoritative, is never sufficient.
- Always correlate external content with both the process model and code (Phase 4).
- Always persist external details directly into metamodel layers when content matches.
- Always create discrepancy tasks for mismatches, unused-process references, and contradictions (Phase 4).
- Always record contradictions in the traceability layer (b9.902).
- Do NOT modify the source process model to hide contradictions — only mark/annotate findings.
- Do NOT create metamodel files for processes that are clearly unused (a sub-tree under an unused parent).
- Prefer updating existing metamodel files over creating new ones.

---

## Common Pitfalls

1. **Wrong phase order**: Starting code verification before completing process analysis and source extraction.
2. **Premature unused marks**: Marking a process unused before searching the codebase.
3. **Incomplete tree**: Stopping at depth 1 misses the majority of processes.
4. **Missing external references**: References may be in ANY process at ANY depth — not just root or depth 1.
5. **False "no references found"**: Writing "no references found" without having checked every process.
6. **Treating a single source as sufficient**: A process-modeling tool, a document, or a ticket alone is never enough — code verification is always mandatory.
7. **Duplicate metamodel files**: Always check if a file already exists before creating.
8. **Ignoring contradictions**: Code often diverges from the process model. Document, don't ignore.
9. **Unused sub-trees**: When a parent is unused, all children are unused. Mark the parent, skip children.
