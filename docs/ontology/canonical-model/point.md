---
title: "Point"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: takeoff
---

# Point

## 0. Purpose

**What this element represents:** One I/O signal on a device, as it comes off the I/O list and controls diagram — its BAS I/O category and its raw electrical signal. The *engineered range* (e.g., "0–2 in. w.c.") is a Selection-layer decision (Stage 4), not recorded here.

**Why the model needs it:** This is the element the final I/O list, the device list, and controller/panel capacity accounting (Stage 8) are all generated from. It is also where the "used for control or visualization" flag lives — the thing that decides whether a range must be assigned at all.

---

## 1. Definition

A point is one I/O signal, attached to a device, with a BAS I/O category and a raw signal type.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | the point name, per the naming standard (`governance-and-doctrine/naming-standards.md`) | — |
| `signal_type` | enum | yes | `AI` \| `AO` \| `BI` \| `BO` | the BAS I/O category — this is what Stage 8 capacity accounting counts |
| `raw_signal` | enum | yes | starter list: `4-20mA`, `0-10VDC`, `dry_contact`, `rtd_1000ohm`, `thermistor_10k`, `pulse` — extend per the vocabulary's extension rule | as shown on the controls diagram or I/O list, before engineering units are assigned |
| `attaches_to` | reference | yes | a `device` id | — |
| `usage` | list of enum | yes | `control` \| `visualization` \| `alarm_only` \| `monitoring_only` | drives the Stage 4 exit rule: any point tagged `control` or `visualization` must get a range and sizing basis in the Selection layer |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | — |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| attaches_to | `device` | 1 |
| has | `selection` (Stage 4, for range/sizing) | 0–1 |
| assigned_to | `controller` (Stage 8, architecture layer) | 0–1 |

---

## 4. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| Every point on the I/O list has a corresponding `point` element, and vice versa | consistency | Model-checkpoint validation check (`validation/io-completeness.md`) |
| Every point tagged `usage: control` or `usage: visualization` has a Selection-layer range before the Model Checkpoint closes | completeness | Blocks Stage 5 exit |
| `signal_type` and `raw_signal` are a sane pairing (e.g., `BI` should not carry `4-20mA`) | consistency | Flag |

---

## 5. Example

```yaml
id: AHU1-SF-S
signal_type: BI
raw_signal: dry_contact
attaches_to: AHU1-SF-STAT
usage: [control, alarm_only]
source_ref:
  - {kind: schedule, doc_id: M-601, location: "AHU-1 I/O list, row 14", revision: "Rev 2"}
```

---

## 6. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
