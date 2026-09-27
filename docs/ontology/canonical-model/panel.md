---
title: "Panel"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: architecture
---

# Panel

## 0. Purpose

**What this element represents:** The physical enclosure added during Stage 8 (Controller/Panel I/O Assignment) — it contains one or more `controller` elements and carries whatever physical accommodation for future growth (rack/gutter space, power budget, wireway capacity) the project has set aside, whether or not that headroom is tied to any specific controller yet.

**Why the model needs it:** A panel is frequently sized, laid out, and purchased to hold more than the currently-committed controller count — one enclosure serving several AHUs, with an extra module bay built into the backpan layout from day one. That headroom belongs to the panel as a whole until (or unless) a specific `controller` element is added to claim it.

---

## 1. Definition

A panel is one physical enclosure, containing one or more `controller` elements, with its own rack/power/wireway budget.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | project-unique, or the drawing's panel designation | — |
| `contains` | list of reference | yes, 1+ | `controller` ids | every controller physically in this enclosure, including ones with `provisioning` less than `purchased` |
| `rack_space_reserved` | text | no | free text or a count, per the platform's module chunk sizing | headroom not yet claimed by any specific `controller` element — e.g., "one open module bay, unassigned" |
| `power_budget_reserved` | text | no | free text | — |
| `wireway_capacity_reserved` | text | no | free text | — |
| `location` | reference | no | a location on the floor plan drawing (Stage 9) | — |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | the backpan layout / panel GA drawing |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| contains | `controller` | 1–n |

---

## 4. Panel-Level Headroom vs. Controller-Level Provisioning

Reserved capacity can live at either level, and a model should show which:

- If the backpan layout already draws a specific controller module in its bay — even one not yet purchased — model it as a `controller` element with `provisioning` less than `purchased` (see [Controller](controller.md) §4). This is the more common case: the module's *identity* (which platform, which capacity) is already decided, only the purchase timing is deferred.
- If the panel simply has *unassigned* rack/power/wireway margin with no specific module identity decided yet (e.g., "we left one extra bay, we don't know what will go there"), record it on the panel's own `rack_space_reserved` / `power_budget_reserved` / `wireway_capacity_reserved` attributes instead, with no corresponding `controller` element until a specific one is decided.

---

## 5. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| Every id in `contains` is a real `controller` element | consistency | Flag |
| The sum of controllers' physical footprint (per their `device_ref` hardware page) plus `rack_space_reserved` does not exceed the panel's physical dimensions | consistency | Flag at Stage 8 finalize |

---

## 6. Example

```yaml
# anonymized instance
id: PNL-AHU-1-2-3
contains: [KMC1, KMC2, KMC3, KMC4]
rack_space_reserved: null
power_budget_reserved: null
wireway_capacity_reserved: null
location: "Level 5, Wing A mechanical room"
source_ref:
  - {kind: submittal, doc_id: "backpan layout", location: "DDC-AHU1,2,3 panel GA", revision: "as-built"}
```

---

## 7. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
