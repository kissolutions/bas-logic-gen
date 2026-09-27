---
title: "System"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: takeoff
---

# System

## 0. Purpose

**What this element represents:** One piece of controlled equipment (an AHU, a VAV box, a pump system, etc.) — the top level of the model under `project`. Either a single unit or a **typical**: one definition standing in for many identical instances.

**Why the model needs it:** Stage 2 (System Identification) produces this level first, before any I/O is taken off, so every later element has a system to attach to and a system-type playbook to be checked against.

---

## 1. Definition

A system is one tagged piece of equipment with a system type. Its type selects which system playbook (`design-playbooks/systems/`) supplies the baseline used to catch missing points and features during takeoff.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | equipment tag, e.g. `AHU-1` | as used on drawings |
| `type` | enum | yes | matches a `design-playbooks/systems/` page (`ahu`, `vav`, `rtu`, `mau`, `exhaust_fan`, `pump_system`, `boiler_plant`, `chilled_water_system`, `heating_water_system`, `cooling_tower`) | selects the baseline; see Gate G2 in the Controls Engineering Playbook if none matches |
| `typical` | boolean | yes | — | true if this element stands in for a group of identical units |
| `instances` | list of strings | if `typical: true` | equipment tags | e.g., `[VAV-1-01 .. VAV-1-40]` |
| `description` | text | no | free text | plain description as used in conversation |
| `location` | string | no | e.g., "Mechanical Room, 2nd floor" | may later cross-reference the Floor Plan Drawing (Stage 9) |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | where this system was identified from |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| has | `feature` | 0–n |
| has | `component` | 0–n |
| has | `dependency` | 0–n |
| has | `open_item` | 0–n |

---

## 4. Source Traceability

A system's existence traces to whichever schedule or diagram first named it (Stage 1/2). Its `type` classification is an engineering decision and gets its own source reference if it required judgment (e.g., the schedule doesn't label the unit type explicitly).

---

## 5. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| `type` matches an existing system playbook | completeness | Gate G2 — escalate |
| Every scheduled/diagrammed unit has a corresponding `system` element | completeness | Flag at Stage 2 exit |
| `typical: true` requires a non-empty `instances` list | completeness | Flag |

---

## 6. RFI Triggers

A unit whose type cannot be determined from the documents (ambiguous or missing schedule data) becomes an open item of kind `missing`, not a guess.

---

## 7. Example

```yaml
id: AHU-1
type: ahu
typical: false
description: "Primary AHU serving 2nd floor"
source_ref:
  - {kind: schedule, doc_id: M-601, location: "sheet 4, AHU schedule", revision: "Rev 2"}
```

---

## 8. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
