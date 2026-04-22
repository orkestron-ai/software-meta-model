<!-- AISMM:BEGIN -->
type: layer_document
layer_id: 006
layer_key: product_pnl_model
document_id: economics.pnl.model
document_type: pnl_model
module_scope: root
status: active
title: Product P&L Model
references:
  - file:pnl.schema.json
<!-- AISMM:META_END -->

# Product P&L Model

## Overview

This document defines the **economic model of the product**.

It describes:
- how revenue is generated
- how costs are incurred
- how profit is calculated
- how economic elements are linked to system behavior

AISMM Studio uses this layer to:
- calculate unit economics
- link events to revenue and costs
- validate economic consistency

---

## 1. Revenue Streams

Define all sources of revenue.

For each revenue stream:

- **ID:**
- **Name:**
- **Type:** (subscription / usage / transaction / licensing / other)
- **Source Event (optional):**
- **Pricing Model:**
- **Unit Price:**
- **Description:**

---

## 2. Cost of Revenue (COGS)

Define costs directly linked to product usage.

For each cost:

- **ID:**
- **Name:**
- **Type:** (infrastructure / external_service / api_usage / other)
- **Source Event / Control / Resource:**
- **Unit Cost:**
- **Description:**

---

## 3. Operating Expenses (OPEX)

### 3.1 Development

- Team costs
- Development infrastructure
- Tools

### 3.2 Operations

- Operations team
- Monitoring / observability tools
- Support

### 3.3 Product Management

- Product managers
- Analysts
- Designers

### 3.4 Security & Compliance

- Security tools
- Audits
- Certifications

---

## 4. Other Costs

- Shared / allocated costs
- External services not tied to usage
- Overhead (optional)

---

## 5. Profit Calculation

Define formulas:

- **Gross Profit:** Revenue - COGS
- **Operating Profit:** Gross Profit - OPEX
- **Net Profit:** Operating Profit - Other Costs

---

## 6. Mapping to System Behavior

- Revenue → Events
- Cost → Events / Controls / Resources

Example:

```
event.workflow_executed → revenue.usage_fee
event.sms_sent → cost.sms
```

---

## 7. Mapping to Value Streams (VSS)

- VSS Flow → Revenue / Cost
- VSS Converter → Cost center
- Anti-value → Loss

---

## 8. Mapping to Critical Path

- Critical steps → cost drivers
- Final outcome → revenue realization

---

## 9. Assumptions

- 

---

## 10. Constraints

- 

---

## Notes

- This layer defines **how money is calculated**, not when (see P&L Plan)
- All items should have stable IDs
- Prefer event-based attribution where possible

<!-- AISMM:END -->
