---
title: "Canonical Model Vocabulary"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
schema_file: schemas/ (to be created)
---

# Canonical Model Vocabulary

The canonical model is an XML document built from a **standard vocabulary** of elements. Using standard elements keeps models consistent across projects, lets tools validate them, and lets generated views (device list, I/O list) work without per-project changes.

This page lists the vocabulary. Each element has its own page defining attributes, validation rules and RFI triggers. The formal schema will live in `schemas/`.

---

## 1. Element Hierarchy

```
project
└── system            (one per unit, or one per typical with instances)
    ├── feature       (declared capability, e.g., economizer = true)
    ├── component     (fan, damper, coil, filter, valve, ...)
    │   └── device    (sensor, switch, actuator, VFD, ...)
    │       ├── point (one I/O signal; may cite external_system via
    │       │          external_source, if its wire originates outside
    │       │          this project's design scope)
    │       └── selection   (selection layer — stage 4)
    ├── reserved_io   (anticipated, not-yet-committed I/O — architecture layer)
    ├── dependency    (serves / served-by / shared signal — between two
    │                  systems both present in THIS model)
    └── open_item     (conflict, missing information, RFI)

external_system        (minimal reference to equipment outside this
                         project's design scope — takeoff layer; see
                         external-system.md. Never gets its own
                         component/device/point breakdown.)

panel                  (enclosure; contains controllers — architecture layer)
└── controller         (controller/remote-I/O hardware; hosts points and
                         reserved_io up to its capacity — architecture layer.
                         A controller can itself be provisioned-but-not-yet-
                         purchased, or purchased with zero points assigned —
                         real spare capacity at whole-module granularity,
                         distinct from a named reserved_io point.)

source_ref            (attached to any element or attribute)
```

| Element | Page | Layer |
|---|---|---|
| system | [System](system.md) | takeoff |
| feature | [Feature](feature.md) | takeoff |
| component | [Component](component.md) | takeoff |
| device | [Device](device.md) | takeoff |
| point | [Point](point.md) | takeoff |
| selection | [Device Selection](selection.md) | selection |
| dependency | [Dependency](dependency.md) | takeoff |
| external_system | [External System Reference](external-system.md) | takeoff |
| open_item | [Open Item](open-item.md) | any |
| reserved_io | [Reserved I/O](reserved-io.md) | architecture |
| panel | [Panel](panel.md) | architecture |
| controller | [Controller](controller.md) | architecture |
| source_ref | [Source Reference](source-reference.md) | any |

Behavior elements (function, setpoint, alarm, safety, operating mode, control relationship) are **not** part of the canonical model. They belong to the narrative stage and live in `ontology/narrative-model/`.

---

## 2. Standard Type Lists

Each element's `type` attribute draws from a controlled list maintained on its element page (e.g., component types: supply_fan, return_fan, outside_air_damper, ...; device types: current_switch, dp_switch, averaging_temp_sensor, ...).

<!-- Type lists to be built from existing projects and system playbooks. -->

---

## 3. Extension Rule

When something in the documents has no standard element or type:

1. Use the closest standard element with `type="custom"`.
2. Add a `custom_type` attribute with a proposed name and a `note` describing what it is.
3. Raise an open item of kind `vocabulary` so the item is reviewed at the model checkpoint.
4. If the same custom type recurs across projects, add it to the standard type list.

Never invent a new element name outside this vocabulary.

---

## 4. Status and Stewardship

- **Status:** Draft — hierarchy agreed; attributes and type lists pending
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
