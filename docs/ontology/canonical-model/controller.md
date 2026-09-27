---
title: "Controller"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: architecture
---

# Controller

## 0. Purpose

**What this element represents:** One piece of controller or remote-I/O hardware — a KMC BAC-59xx, a CAN-59xx expansion module, a PLC rack, or similar — added during Stage 8 (Controller/Panel I/O Assignment). Not a field device (see [Device](device.md)) and not the enclosure that holds it (see [Panel](panel.md)).

**Why the model needs it:** Once device selection gives us the real I/O count, the controller/module choice is itself an engineering decision — and one with a cost lever on top of it: a module can be fully populated, partially populated, or sitting in the panel already purchased and installed with zero points currently assigned to it (real spare capacity at whole-module granularity, distinct from a *named* anticipated point — see [Reserved I/O](reserved-io.md)).

---

## 1. Definition

A controller is one controller/remote-I/O hardware instance, hosting `point` and `reserved_io` elements up to its per-type capacity, contained in a `panel`.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | project-unique, or the drawing's designation (e.g., `KMC1`) | — |
| `device_ref` | reference | yes | a knowledgebase_wikijs `Devices/Controllers/<platform>/` hardware page | **AUTHORITY:** point capacity by type, module chunk sizes, form factor, and power draw are physical device facts. They are never invented or restated here — they live only on the wiki hardware page, exactly like a field device. This element cites it. |
| `parent_panel` | reference | yes | a `panel` id | — |
| `provisioning` | enum | yes | `space_only` \| `space_and_power` \| `space_power_and_wireway` \| `purchased` | describes *this controller's own* installation state — see §4. Not the same field as a `reserved_io` item's `provisioning`, which describes a named point's claim on capacity that may or may not yet exist |
| `points_assigned` | list of reference | no | `point` ids | must be empty unless `provisioning: purchased` — see Validation Rules |
| `reserved_io_assigned` | list of reference | no | `reserved_io` ids | named anticipated points drawing against this controller's remaining capacity |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | the panel schematic/backpan layout showing this controller |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| contained_in | `panel` | 1 |
| hosts | `point` | 0–n |
| hosts | `reserved_io` | 0–n |
| cites | knowledgebase_wikijs controller hardware page | 1 |

---

## 4. Provisioning vs. Reserved I/O — Two Different Kinds of "Not Built Yet"

These answer different questions and must not be collapsed into one:

- **`reserved_io`** (see that page): *a specific, named point* is anticipated, with no committed device or source document behind it yet. It draws capacity from whatever controller ends up hosting it.
- **`controller.provisioning`** (this attribute): *an entire controller/module* exists in the model as a growth increment with no named points behind it at all — the panel has rack space, and possibly power and wireway, set aside (or the module itself is already bought and sitting in the enclosure) purely as headroom, because a future need is expected in kind but not yet in specifics. `provisioning: purchased` with an empty `points_assigned` is a legitimate, common state: the hardware is in the panel, powered, and addressable, and simply has nothing wired to it yet.

A controller only advances from `space_and_power` (or lower) to `purchased` when someone actually buys and installs the module — a cost decision, not an engineering one (see Controls Engineering Playbook, Stage 8, Gate G6).

---

## 5. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| `points_assigned` is empty unless `provisioning: purchased` | consistency | A point cannot be wired to hardware that isn't installed yet — flag |
| Every `id` in `points_assigned` is a real `point` element | consistency | Flag |
| `device_ref` resolves to an existing knowledgebase_wikijs hardware page | consistency | Flag; if the page doesn't exist yet, scaffold it (PENDING datasheet fields), never invent the numbers here |
| A controller with `provisioning: purchased` and empty `points_assigned` and empty `reserved_io_assigned` is valid | — | Not a failure — this is real spare capacity, already bought, awaiting a future need |

---

## 6. Example

```yaml
# anonymized instance — a fourth controller module physically installed
# in a multi-AHU panel, powered and networked, with no field points
# currently assigned: real spare capacity at whole-module granularity.
id: KMC4
device_ref: "knowledgebase_wikijs/Devices/Controllers/KMC/CAN-5902.md"
parent_panel: PNL-AHU-1-2-3
provisioning: purchased
points_assigned: []
reserved_io_assigned: []
source_ref:
  - {kind: submittal, doc_id: "backpan layout", location: "controller panel GA drawing", revision: "as-built"}
```

---

## 7. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
