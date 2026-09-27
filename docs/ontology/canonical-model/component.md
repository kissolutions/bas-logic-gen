---
title: "Component"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: takeoff
---

# Component

## 0. Purpose

**What this element represents:** A physical part of a system — a fan, a damper, a coil, a filter, a valve — that field devices attach to. Sits between `system` (the whole unit) and `device` (the sensor/switch/actuator on it).

**Why the model needs it:** Devices don't float freely on a system; they belong to a specific mechanical part, and that part is what a controls diagram is usually organized around (one row per fan, per damper, etc.).

---

## 1. Definition

A component is one physical part of a system, holding a controlled `type` and the devices attached to it.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | project-unique, or the equipment tag if the document gives one (e.g., `SF-1`) | — |
| `type` | enum | yes | starter list: `supply_fan`, `return_fan`, `exhaust_fan`, `outside_air_damper`, `return_air_damper`, `exhaust_damper`, `mixing_box`, `cooling_coil`, `heating_coil`, `filter`, `sound_attenuator`, `humidifier`, `pump`, `valve` — extend per the vocabulary's extension rule | — |
| `attaches_to` | reference | yes | a `system` id | — |
| `quantity` | integer | no | — | use when several identical components share one entry rather than repeating elements (e.g., "2 return fans") — otherwise give each its own `id` |
| `description` | text | no | free text | — |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | — |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| attaches_to | `system` | 1 |
| has | `device` | 0–n |

---

## 4. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| `type` is on the controlled list, or uses the extension rule | consistency | Flag |
| A component the system playbook baseline expects (e.g., an AHU should have a supply fan) has no corresponding element | completeness | Raise an open item |

---

## 5. Example

```yaml
id: SF-1
type: supply_fan
attaches_to: AHU-1
source_ref:
  - {kind: controls_diagram, doc_id: M-701, location: "AHU-1 controls diagram", revision: "Rev 2"}
```

---

## 6. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
