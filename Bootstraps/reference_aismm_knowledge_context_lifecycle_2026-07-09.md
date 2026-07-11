# AISMM Canonical Knowledge Context Lifecycle Bootstrap

## Purpose

This bootstrap defines the **standard pass for retrieving context before a task, persisting findings after a task, and periodically maintaining the resulting knowledge** — expressed entirely in terms of the AISMM registry and bundle/layer model.

It replaces any ad hoc, project-local "docs" knowledge base convention. Per the Bootstraps README's Structural Rules, product knowledge lives under `aismm/` as bundle/layer artifact files, discoverable through `aismm.registry.json` — never in a separate, unregistered folder tree.

Use it:
- **Before** starting any implementation, debugging, research, migration, or architectural change (retrieval)
- **After** completing a task, investigation, research, or architectural decision (persistence)
- **Periodically**, after a series of tasks or before a major reconciliation cycle (maintenance)

---

## Part 1: Retrieve Context (Pre-Task)

Goals: reuse existing decisions, prevent duplication, maintain product-model consistency, detect constraints and risks early.

### 1.1 Start from the registry

Follow the registry-first discovery pipeline in `../aismm-model-registry.md`: load `aismm.registry.json`, resolve declared sources, and note `completeness_status` before relying on any layer as exhaustive.

### 1.2 Extract task signals

From the task description, extract: component/service names, APIs, data entities, integrations, infrastructure components, protocols, and any external ticket/reference identifiers.

### 1.3 Search relevant layers

Use these entry points, in order:
- `b9.901` (knowledge index and navigation) — for a map of what exists and where
- `b9.902` (traceability graph) — for how the signals connect to requirements, scenarios, components
- `b9.903` (source provenance and confidence) — for how trustworthy existing records are
- The bundle/layer directly implicated by the task (e.g. `b2.201` for a component question, `b4.401` for a requirements question)
- `b8.805` (change history and decision log) — for prior directed decisions that constrain the task
- `b7` (quality, risk, compliance) — for existing constraints, risks, or security/compliance requirements

### 1.4 Determine existing constraints

From what is found, determine: architectural constraints already decided, adopted vs. forbidden approaches, performance/operational caveats, known risks, and past incidents or reconciliation notes.

### 1.5 Output format

Produce a concise summary, not a dump of full records:

```markdown
### Related Records
- [record](relative/path.md) — one-line purpose

### Existing Decisions
- constraint / adopted approach / forbidden approach

### Risks And Constraints
- scaling, migration, performance, security notes

### Related Entities
- components, data entities, integrations
```

Rules: use concise summaries; do not paste entire records; do not repeat the same information twice; prioritize records with higher confidence/provenance; explicitly warn if a candidate approach conflicts with an existing decision.

---

## Part 2: Save Context (Post-Task)

Goals: make findings from this task discoverable and reusable by the next task or agent, without creating unregistered knowledge.

### 2.1 When to save

Save after: completing a feature or bugfix, finishing research or an investigation, discovering a new pattern/constraint/integration detail, resolving a non-obvious problem, or learning something about the product that isn't obvious from code alone.

### 2.2 What to save

Key findings and decisions, non-obvious patterns and conventions, integration details and constraints, performance characteristics, troubleshooting insights, and newly discovered components/entities/modules.

Do not save: trivial or self-evident facts, temporary debugging notes, information already well documented elsewhere in the model, or raw chat transcripts.

### 2.3 Where to save it — map the finding to a layer, not a folder

| Kind of finding | Target |
|---|---|
| A new or changed fact about a component/integration/data entity | The owning bundle/layer artifact file (e.g. `b2.201`, `b2.204`, `b2.202`) — update existing artifact if one already covers this unit, else create one per `../aismm-layer-artifact-naming.md` |
| A decision made during the task | `b8.805` (change history and decision log) |
| A relationship between existing records, or a new cross-reference | `b9.902` (traceability graph) |
| Where a fact came from and how much to trust it | `b9.903` (source provenance and confidence) |
| A troubleshooting insight with no natural layer home yet | The closest-fitting layer, with an explicit note in **Known Gaps** if the fit is imperfect — do not invent a new unregistered file location |

### 2.4 Required shape

Follow the standard AISMM artifact metadata block and `## Related Objects` conventions already used across the model. Do not introduce a free-form format (title/date/summary/findings/references) that bypasses the standard record shape.

---

## Part 3: Normalize And Maintain (Periodic)

Goals: reduce duplication, keep cross-links correct, keep terminology consistent, keep the model retrieval-friendly.

This part is deliberately thin — the heavy lifting is already canonical:

- Run `../aismm-consistency-checks.md` for structural validation (naming, layer placement, required sections).
- Run `../aismm-health.md` for model health reporting after meaningful updates.
- Use `../aismm-consistency-rules.md` for conflict resolution when two records disagree.

In addition to running the above, this pass should:

### 3.1 Deduplicate content (not just structure)

Detect overlapping records describing the same component, decision, or process at different times, and merge them — preserving the artifact identity of the record that remains source-of-truth, per `../aismm-repository-workflow.md` (status changes over deletion; see also the research-methodology bootstrap's never-delete rule).

### 3.2 Fix cross-links

Check `## Related Objects` sections and inline links for: broken relative paths, missing backlinks (A links to B but B doesn't link back), and links that now point to a superseded record.

### 3.3 Normalize terminology

Standardize how the model refers to the same component/integration/concept across records (service names, external-system names, data-entity names). Prefer the term already dominant in the model; do not invent new terminology mid-normalization.

### 3.4 Compress without losing signal

Reduce repeated explanations, verbose descriptions, and temporary implementation detail that has since been superseded. Preserve architectural reasoning, operational caveats, performance findings, and integration constraints — these are exactly what Part 1 retrieval depends on.

---

## Rules

- Never create a knowledge store outside the `aismm/` bundle/layer model (no ad hoc `docs/` tree, no unregistered notes folder).
- Always start retrieval from the registry, not from a manual folder scan.
- Always map a "save" to a specific layer/bundle target — if no target fits, say so explicitly as a gap rather than inventing a new location.
- Prefer merge/update over duplication; prefer status change over deletion.
- Keep retrieval summaries concise — this bootstrap's output feeds directly into agent context windows.
