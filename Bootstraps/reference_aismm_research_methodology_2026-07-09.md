# AISMM Canonical Research Methodology Bootstrap

## Purpose

This bootstrap defines the **mandatory research and modeling methodology** for AISMM.

It governs:
- how metamodel artifacts are preserved (never deleted)
- how change management entities are structured (change → workitem → task)
- how research proceeds through process, scenario, and behavior layers

Use it whenever:
- creating, updating, or retiring metamodel artifacts
- formalizing changes, work items, or tasks
- analyzing processes, scenarios, or behavioral requirements
- reconciling upstream research with the product model

---

## Rule 1: Never Delete Metamodel Artifacts

### Principle

Metamodel artifacts are **permanent records** of product knowledge. Once created, they are never deleted automatically or without explicit double confirmation from the human user.

### What This Means

- An LLM agent must **never** delete a metamodel file on its own initiative
- If a file is outdated, unused, misplaced, or otherwise no longer valid — the agent must:
  1. Update the file's status to `deprecated`, `inactive`, `superseded`, or `misplaced`
  2. Record the reason for the status change in the file's metadata and body
  3. Link to the replacement artifact if one exists
  4. Flag the file for human review — only a human may authorize deletion

### Status Values

| Status | When to Use |
|--------|-------------|
| `active` | Current, valid artifact |
| `deprecated` | Superseded by a newer artifact; content preserved for reference |
| `inactive` | No longer used but may be reactivated; content preserved |
| `superseded` | Replaced by a different approach or model; link to replacement |
| `misplaced` | Created in the wrong location or layer; link to correct location |
| `pending_review` | Flagged for human review before any further action |

### Metadata for Retired Artifacts

When changing status from `active` to any non-active state, add:

```yaml
<!-- AISMM:BEGIN -->
status: deprecated
deprecated_date: "2026-07-09"
deprecated_reason: "Superseded by new process model after a source restructure"
superseded_by: "process-26061000002-registration-v2.md"
reviewed_by: null
<!-- AISMM:META_END -->
```

### Exception — Human-Authorized Deletion

The only path to deletion:
1. Agent flags file with `pending_review` status and documents reason
2. Human user reviews and explicitly confirms deletion (first confirmation)
3. Agent repeats the request for deletion with the file's content summary
4. Human user explicitly confirms deletion a second time (second confirmation)
5. Only then may the agent delete the file

**Without both confirmations, the file stays.**

---

## Rule 2: Change Management Entity Separation

### Principle

Change management uses three distinct entity types, each stored as a **separate artifact file** in `b8.801` (work-items-and-change-requests). The entities are linked to each other, forming a hierarchy.

### Entity Types

#### Change

A change represents a **wave of modifications** — what we want to change and how.

- One change = one file
- Contains: intent, scope, motivation, affected layers, high-level approach
- May contain multiple work items
- Status: `proposed`, `approved`, `in_progress`, `completed`, `rejected`, `deferred`

**File naming:** `change-<id>-<slug>.md`

**Required links:**
- `Related Work Items` — list of workitem files belonging to this change
- `Source Provenance` — link to b9.903 artifact
- `Signal Origin` — link to intake evidence from U1

#### Work Item

A work item represents an **epic-level unit** within a change — a consolidated group of tasks that delivers a specific result.

- One work item = one file
- Contains: description of the result, acceptance criteria, owning change
- May contain multiple tasks
- Status: `open`, `in_progress`, `completed`, `blocked`

**File naming:** `workitem-<id>-<slug>.md`

**Required links:**
- `Owning Change` — link to the change file
- `Related Tasks` — list of task files belonging to this work item
- `Related Artifacts` — links to affected metamodel layers (b1, b4, b5, etc.)

#### Task

A task represents a **concrete unit of work** — executing it contributes to closing the parent work item.

- One task = one file
- Contains: specific action, executor, dependencies, completion criteria, request/ticket identifier
- Status: `open`, `in_progress`, `completed`, `blocked`

**File naming:** `task-<id>-<slug>.md`

**Required links:**
- `Owning Work Item` — link to the workitem file
- `Owning Change` — link to the change file (transitive)
- `Request ID` — external ticket or request identifier (if applicable)
- `Result Artifact` — link to produced artifact upon completion

### Hierarchy

```
change-<id>-<slug>.md
  └── workitem-<id>-<slug>.md
        └── task-<id>-<slug>.md
        └── task-<id>-<slug>.md
  └── workitem-<id>-<slug>.md
        └── task-<id>-<slug>.md
```

### Cross-Linking Rules

- Every work item MUST link to its owning change
- Every task MUST link to its owning work item AND its owning change
- Changes MUST list all contained work items
- Work items MUST list all contained tasks
- Links use relative markdown paths: `[[Work Item: Registration Redesign | ../workitem-001-registration-redesign.md]]`

### When to Decompose

Decompose a work item into tasks when:
- The work depends on one named executor, one team queue, or one external service request
- Separate request tracking, completion evidence, or result artifacts are needed downstream
- The work is addressed to infrastructure, platform, security, legal, procurement, or another external executor

When a task is addressed to an external executor, phrase it as a **direct execution request** with explicit actions and required outputs — not as a discovery prompt.

---

## Rule 3: Research Order — Process, Scenario, Behavior

### Principle

Research and modeling must follow a deterministic order:

**Process → Scenario → Behavior**

Each level builds on the previous one. Files at each level must be linked to files at the levels above and below them.

### Level 1: Processes

Processes describe **what participants do** and **how the activity works**.

- One process = one file
- A process may decompose into sub-processes
- Each sub-process is a separate file
- Files live in `b1.103` (processes)

**Structure:**
```
process-<id>-<root-name>.md
  └── process-<id>.1-<subprocess>.md
        └── process-<id>.1.1-<sub-subprocess>.md
```

**Required links:**
- `Parent Process` — link to parent process file (if sub-process)
- `Child Processes` — list of sub-process files
- `Related Scenarios` — list of scenario files that traverse this process
- `Related Artifacts` — links to functional objects, performers, diagrams

### Level 2: Scenarios

Scenarios describe **possible paths through a process** — how a user or system moves from start to finish along specific conditions.

- One scenario = one file
- A scenario MUST be linked to at least one process and/or process step
- Files live in `b5.501` (user-scenarios-and-ux-logic)

**Required links:**
- `Owning Process` — link to the process file the scenario traverses
- `Process Steps` — links to specific process step files (if applicable)
- `Related Behaviors` — links to behavioral requirement files
- `Related Artifacts` — links to requirements, rules, state machines

### Level 3: Behavior

Behavior describes **how the system responds** — requirements, rules, state transitions triggered by process steps or scenario paths.

- Behavior artifacts MUST be linked to at least one process and/or scenario
- Files live in `b4.401` (requirements), `b4.402` (domain rules), `b4.405` (state machines)

**Required links:**
- `Related Process` — link to the process file
- `Related Scenario` — link to the scenario file (if applicable)
- `Related Artifacts` — links to related rules, states, or other behaviors

### Cross-Level Linking

```
process-26061000001-registration.md
  ├── process-26061000001.2-email-verification.md
  │     └── (linked to)
  │
  ├── (linked to)
  │     usc-26061000001-one-click-registration.md  (scenario)
  │     usc-26061000001-email-flow.md  (scenario)
  │
  └── (linked to)
        req-26061000001-registration-api.md  (behavior)
        rule-26061000001-email-validation.md  (behavior)
```

### Summary Lists vs Graph Nodes

**Summary lists are allowed only as artifacts** — they provide indexes, tables of contents, or overviews. They are NOT nodes in the connected graph.

**Graph nodes are individual files** — each process, scenario, and behavior file is a first-class node with bidirectional links to related files.

A summary might look like:
```markdown
## Process Index

| Code | Name | File | Status |
|------|------|------|--------|
| Go-1 | Registration | [[Registration | process-26061000001-registration.md]] | active |
| Go-1.2 | Email Verification | [[Email Verification | process-26061000001.2-email-verification.md]] | active |
```

But the actual connected knowledge lives in the individual files with their cross-links.

---

## Enforcement

- Agents must follow this research order when populating the metamodel
- Agents must never skip a level — do not create scenarios without processes, do not create behavior without processes or scenarios
- Agents must always create cross-links between levels when creating new artifacts
- Agents must never delete artifacts — use status changes instead
- Agents must create separate files for change, workitem, and task entities — never combine them into one file
