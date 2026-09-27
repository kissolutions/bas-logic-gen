---
title: "Dependency"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: takeoff
---

# Dependency

## 0. Purpose

**What this element represents:** A relationship between two systems — one serves another, or two share a signal — recorded during Stage 2 (System Identification).

**Why the model needs it:** Cross-system relationships (a static pressure reset strategy needing VAV damper positions, an AHU serving forty VAV boxes) are exactly the kind of fact that gets lost if it only lives in someone's mental picture of the project. They also drive later narrative and logic work: a function on one system can require a point from another.

---

## 1. Definition

A dependency names two systems and the nature of the relationship between them.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | project-unique | — |
| `type` | enum | yes | `serves` \| `served_by` \| `shares_signal_with` | `serves`/`served_by` are inverses of each other |
| `from_system` | reference | yes | a `system` id | — |
| `to_system` | reference | yes | a `system` id (or a typical's instance list) | — |
| `description` | text | no | free text | e.g., "static pressure reset uses VAV-1-* damper positions" |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | — |

---

## 3. Relationships

A dependency connects exactly two `system` elements; it has no children of its own.

---

## 4. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| Both `from_system` and `to_system` reference existing systems | consistency | Flag |
| `serves` on one system has a matching `served_by` on the other (or is generated automatically as the inverse) | consistency | Reconcile at Stage 2 exit |

---

## 5. Example

```yaml
id: DEP-002
type: serves
from_system: AHU-1
to_system: VAV-1-*
description: "AHU-1 serves VAV-1-01 through VAV-1-40"
source_ref:
  - {kind: floor_plan, doc_id: M-201, location: "2nd floor plan", revision: "Rev 1"}
```

---

## 6. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
