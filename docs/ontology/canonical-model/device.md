---
title: "Device"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: takeoff
---

# Device

## 0. Purpose

**What this device element represents:** A field device attached to a component — a sensor, switch, actuator, or VFD — as shown on the controls diagram. Purely a takeoff fact: a device is *present*; which specific product and range get chosen is the [Selection](selection.md) layer, added in Stage 4.

**Why the model needs it:** The controls diagram is drawn device-by-device (an averaging sensor here, a DP switch there); this element mirrors that so the takeoff can be checked directly against the diagram.

---

## 1. Definition

A device is one field device instance, attached to a component, with a controlled `type`.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | project-unique, or the drawing's device tag if given | — |
| `type` | enum | yes | starter list: `current_switch`, `dp_switch`, `dp_sensor`, `averaging_temp_sensor`, `immersion_temp_sensor`, `duct_temp_sensor`, `humidity_sensor`, `co2_sensor`, `damper_actuator`, `valve_actuator`, `vfd`, `freezestat`, `smoke_detector` — extend per the vocabulary's extension rule | as drawn on the controls diagram, e.g., "averaging sensor" vs. "single-point sensor" vs. "DP-type airflow station" |
| `attaches_to` | reference | yes | a `component` id | — |
| `description` | text | no | free text | — |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | — |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| attaches_to | `component` | 1 |
| has | `point` | 1–n |
| has | `selection` (selection layer, Stage 4) | 0–1 |

---

## 4. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| `type` is on the controlled list, or uses the extension rule | consistency | Flag |
| The device type shown on the controls diagram matches the device type on the I/O list | consistency | Model-checkpoint validation check (see `validation/`) |
| A device the system playbook baseline expects (e.g., a freezestat on an AHU with a heating coil) has no corresponding element | completeness | Raise an open item, not a silent addition |

---

## 5. Example

```yaml
id: AHU1-SF-STAT
type: current_switch
attaches_to: SF-1
source_ref:
  - {kind: controls_diagram, doc_id: M-701, location: "AHU-1 controls diagram", revision: "Rev 2"}
```

---

## 6. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
