---
title: "Feature"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: takeoff
---

# Feature

## 0. Purpose

**What this element represents:** A declared capability or variant flag on a system — economizer present, static pressure reset used, fan drive type — set once during Stage 2 (System Identification) and used to select the right variant within the system's playbook.

**Why the model needs it:** Features are declared facts ("this unit has an economizer"), not behavior. What the economizer *does* is narrative; that it *exists* is takeoff. Keeping them separate is what lets Stage 3 raise an open item when a playbook-expected feature isn't backed by a source.

---

## 1. Definition

A feature attaches to a `system` and states one flag with a value and a source.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | project-unique | — |
| `name` | enum | yes | drawn from the controlled feature-name list for the system's `type` (maintained on that system's playbook page) | e.g., `economizer`, `static_pressure_reset`, `fan_drive_type`, `reheat` |
| `value` | boolean \| enum | yes | boolean for a yes/no flag; enum for a multi-valued one (e.g., `fan_drive_type: vfd \| constant_volume \| multi_speed`) | — |
| `attaches_to` | reference | yes | a `system` id | — |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | — |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| attaches_to | `system` | 1 |

---

## 4. Source Traceability

Every feature's value must be traceable to a schedule, diagram, or SOO statement — never inferred because "units like this usually have one." An inference belongs on the system playbook as a baseline expectation, which then produces an open item if the documents don't confirm it (see Stage 3).

---

## 5. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| `name` is on the controlled list for the system's `type` | consistency | Use the extension rule (canonical-model-vocabulary.md §3) |
| A feature the system playbook baseline expects has no corresponding element | completeness | Raise an open item, don't silently add one |

---

## 6. Example

```yaml
id: FEAT-003
name: fan_drive_type
value: vfd
attaches_to: AHU-1
source_ref:
  - {kind: schedule, doc_id: M-601, location: "AHU-1 row, motor data column", revision: "Rev 2"}
```

---

## 7. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
