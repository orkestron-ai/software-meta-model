# AISMM Agent Contract References

```yaml
aismm_meta:
  document_type: governance
  document_id: aismm.agent-contract-references
  spec_version: 2.0.0
  status: accepted
  created: "2025-01-01"
  updated: "2025-01-01"
```

---

## Boundary Statement

> **AISMM does not define agent contracts.**
>
> Agent contracts live in separate repositories.
>
> AISMM references agent contracts and records their interaction with the product model.

This separation exists because:

- AISMM is a product knowledge model — it describes what the system is
- Agent contracts define how AI agents work with that knowledge — capabilities, result schemas, execution interfaces
- Mixing them would make AISMM an agent orchestration framework, which is outside its scope

```text
AISMM = product/system knowledge
Agent contracts = how agents work with that knowledge
```

---

## 1. Reference Concepts

AISMM may declare the following reference entities to bridge to external agent contract repositories:

| Entity | Description |
|--------|-------------|
| `agent_contract_reference` | A pointer to an agent contract in an external repository |
| `agent_capability_reference` | A pointer to a specific agent capability declared in an external contract |
| `agent_result_contract_reference` | A pointer to a result/output schema in an external contract |
| `agent_execution_trace_reference` | A pointer to a recorded agent execution trace |
| `agent_task_result_reference` | A pointer to a specific task result produced by an agent |
| `agent_context_package` | A context package assembled by AISMM for agent consumption |

---

## 2. Reference Format

```json
{
  "id": "agent_contract_reference.requirements_reviewer",
  "repository": "https://github.com/orkestron-ai/software-agents-contracts",
  "contract_id": "contract.requirements.review",
  "version": "1.2.0",
  "used_by": ["b8.801.work_item.review_payment_requirements"],
  "context_packages": ["agent_context_package.requirements_review_context"]
}
```

```json
{
  "id": "agent_context_package.requirements_review_context",
  "for_contract": "agent_contract_reference.requirements_reviewer",
  "includes_layers": ["b4.401", "b4.402", "b2.201", "b2.203"],
  "retrieval_strategy": "b9.905",
  "redaction_policy": "standard"
}
```

---

## 3. What AISMM Records About Agent Interactions

AISMM may record **effects** of agent operations on the product model:

- Which work items were created or updated by agents (b8.801, field `last_modified_by`)
- Which knowledge summaries were produced by agents (b8.806)
- Which trace links were added by agents (b9.902, with `authorship_class: agent_authored`)
- Which context packages were assembled and consumed (b9.905)

AISMM does **not** record:

- Agent capability schemas
- Agent result schemas
- Agent execution contracts
- Agent tool definitions

---

## 4. Authorship Tracking

When an agent modifies an AISMM entity, the block must declare:

```yaml
last_modified_by: agent:<contract_id>:<version>
authorship_class: agent_authored
agent_contract_reference: agent_contract_reference.<name>
```

This enables audit trails and confidence scoring for agent-produced content.

---

## Summary

AISMM provides a clean bridge to external agent contracts through lightweight reference entities. This keeps AISMM focused on product knowledge while enabling full traceability of agent interactions with the model.
