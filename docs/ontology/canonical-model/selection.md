---
title: "Device Selection"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: selection
---

# Device Selection

*Not to be confused with the Controller/Panel I/O Assignment stage (Stage 8) — this element is about field devices, added in Stage 4.*

## 0. Purpose

**What this element represents:** The engineering decision made about a takeoff-layer `device`: which actual product was chosen, its engineered range, and the sizing basis behind that range. This is what turns "a DP switch exists here" (a takeoff fact) into "a 0–2 in. w.c. DP switch, sized from the 1.5 in. w.c. scheduled maximum static" (an engineering decision).

**Why the model needs it:** Keeping this separate from `device` is what lets the range trace to its sizing basis instead of being a bare number someone has to trust.

---

## 1. Definition

A selection attaches to a takeoff-layer `device` and records the chosen product, range, and sizing basis.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | project-unique | — |
| `device_ref` | reference | yes | the takeoff `device` id this selects for | — |
| `selected_product` | string | yes | manufacturer + model, or a link to a knowledgebase_wikijs field-device hardware page if one exists | physical device facts live on the wiki page, same authority rule as controller hardware |
| `range` | string | required if the point's `usage` includes `control` or `visualization` | engineering units, e.g., "0–2 in. w.c." | — |
| `sizing_basis` | text | required with `range` | e.g., "sized from 1.5 in. w.c. scheduled max static" | — |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | the schedule/spec value the sizing was based on |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| attaches_to | `device` | 1 |
| cites | knowledgebase_wikijs field-device hardware page | 0–1 |

---

## 4. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| Every `device` whose points include `usage: control` or `usage: visualization` has a corresponding `selection` | completeness | Blocks Model Checkpoint (Stage 5) exit |
| `range` has a `sizing_basis` | completeness | Flag |

---

## 5. Example

```yaml
id: SEL-009
device_ref: AHU1-FILT-DP
selected_product: "generic DP switch, 0-5 in. w.c. adjustable"
range: "0-2 in. w.c."
sizing_basis: "sized from 1.5 in. w.c. scheduled max static, M-601"
source_ref:
  - {kind: schedule, doc_id: M-601, location: "AHU-1 row, static pressure column", revision: "Rev 2"}
```

---

## 6. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
