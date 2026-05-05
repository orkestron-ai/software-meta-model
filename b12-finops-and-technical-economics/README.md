# Bundle 12 — FinOps and Technical Economics

## Overview

Bundle 12 defines **how technical costs, token economics, cloud commitments, quality costs, unit economics, and sustainability metrics are modeled as first-class AISMM entities**.

It is the **financial observability layer of AISMM**, making the economic dimension of technical decisions visible and governable.

---

## Why b12 is Separate from b0 Product Economics

### Boundary Statement

```text
b0 = product-level economics: value model, revenue streams, product P&L, value/cost expectations
b12 = technical economics: infrastructure cost, service cost, tenant/request cost, token/inference cost, carbon/capacity economics
```

Bundle 0 (`006-economics-model.md`) models **product-level economics**: business model, revenue streams, unit economics at the product level, pricing strategy, and value realization expectations.

Bundle 12 models **technical-layer economics** — the costs that emerge from running and operating the software system:

- cost per infrastructure service
- cost per tenant or request
- cost per AI token or inference call
- cloud commitment management
- cost-of-quality (defects, rework)
- cost-of-incident (downtime, remediation)
- carbon and sustainability accounting

### Cross-Bundle Economic Flow

```text
b0 → constrains → b12     (value targets constrain acceptable cost thresholds)
b12 → feeds → b0          (actual technical costs inform product P&L and economics model)
b6 → produces runtime cost signals → b12
b10 → produces AI/model/inference cost signals → b12
b11 → owns cost responsibility and allocation → b12
```

---

## Layers Overview

### 1201 — Cost Allocation and Showback

Tracks cost centers, allocation rules, and showback/chargeback reporting.

Core entities: `cost_center.*`, `cost_allocation.*`, `showback_report.*`, `chargeback_rule.*`

```text
Where does the cost go, and who sees it?
```

---

### 1202 — Capacity and Commitment Management

Models cloud commitments, reserved capacity, and capacity forecasts.

Core entities: `capacity_commitment.*`, `reserved_capacity.*`, `cloud_commitment.*`, `capacity_forecast.*`

```text
What are we committed to paying, and what capacity does that buy?
```

---

### 1203 — Token and Inference Economics

Models AI-specific costs: token budgets, inference costs, model provider costs, context window costs, and agent execution costs.

Core entities: `token_budget.*`, `inference_cost.*`, `model_provider_cost.*`, `context_window_cost.*`, `agent_execution_cost.*`

```text
What does it cost to run our AI features and agents?
```

---

### 1204 — Cost of Quality and Incident

Tracks the financial impact of defects, downtime, and rework.

Core entities: `cost_of_quality.*`, `cost_of_incident.*`, `defect_cost.*`, `downtime_cost.*`, `rework_cost.*`

```text
What does poor quality cost us in real money?
```

---

### 1205 — Unit Economics by Service, Tenant and Request

Models granular unit costs for services, tenants, and individual requests.

Core entities: `service_unit_cost.*`, `tenant_unit_cost.*`, `request_unit_cost.*`, `cost_driver.*`, `margin_signal.*`

```text
What does it cost to serve one customer, one request, one tenant?
```

---

### 1206 — Carbon and Sustainability Accounting

Models carbon metrics, energy usage, CO2e estimates, and sustainability policies.

Core entities: `carbon_metric.*`, `energy_usage.*`, `co2e_estimate.*`, `sustainability_policy.*`

```text
What is the environmental cost of running the system?
```

---

## Key Principles

1. **Technical costs are not product costs.** b12 models operational infrastructure costs; b0 models product value and P&L.
2. **Token and inference economics are first-class.** For AI-native systems, LLM API costs are a primary operational variable and must be explicitly tracked.
3. **Cost signals flow upward.** b12 metrics inform b0 product economics, not replace them.
4. **Ownership is in b11.** Who is responsible for which cost center is declared in b11.1102, projected into b12 cost allocation.
5. **Incident costs are cross-bundle.** `cost_of_incident` links to b6.603 incident records for full auditability.

---

## Typical Use Cases

- Tracking per-service and per-tenant infrastructure costs
- Modeling token/inference budgets for AI features
- Calculating cost-of-incident and cost-of-quality
- Cloud commitment optimization
- Carbon accounting for sustainability reporting
- Feeding technical cost data into product P&L (b0)
- Assigning cost responsibility to teams (b11)

---

## Relationship with Other Bundles

| Bundle | Relationship |
|--------|-------------|
| b0.006 | Product economics — b12 feeds technical cost inputs into product margin |
| b6.601 | Runtime instances are the primary source of infrastructure cost signals |
| b6.603 | Incident records link to `cost_of_incident` in b12.1204 |
| b7.702 | Financial risks are tracked in b7; b12 records their realized cost |
| b8.804 | Releases may change cost profile; release records link to cost changes |
| b10.1004 | Model inference endpoints are the source of token/inference cost signals |
| b11.1102 | Team ownership determines cost responsibility and allocation targets |

---

## Schema

Machine-readable schema:

```text
b12.schema.json
```

Defines entity types and required fields for all six b12 layers.

---

## Summary

Bundle 12 makes technical economics observable, governable, and traceable within AISMM — especially critical for AI-native systems where inference and token costs are significant operational variables.

```text
b12 = the financial lens on the technical system
```
