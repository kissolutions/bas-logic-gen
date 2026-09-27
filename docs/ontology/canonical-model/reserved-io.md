---
title: "Reserved I/O"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: architecture
---

# Reserved I/O

## 0. Purpose

**What this element represents:** A specific, named point that is anticipated but not yet committed — flagged in a review or sidebar conversation as something the project may need, without a confirmed device or source document behind it yet.

**Why the model needs it:** Reserved I/O is not the same thing as generic spare capacity. Spare capacity is unallocated headroom with no identity ("20% more points, in case"). A reserved item is specific ("two DP sensors for OR corridors, discussed in review, currently declined but likely"). Keeping them distinct means a reserved item can be promoted to a real `point` later without disturbing the generic spare calculation, and lets the model show *why* extra rack space, power or wireway was planned in a location, rather than the reason living only in someone's memory of a meeting.

**What this page does NOT do:** define the spare-capacity percentage or module chunk sizes — those are a platform policy (see `platforms/<platform>/architecture-constraints.md`) applied during Stage 8. It also doesn't cover the other two shapes "not committed yet" can take: pure hardware headroom with no content decided (an entire controller module sitting purchased-but-empty — see [Controller](controller.md) §4's `provisioning` attribute), or a fully-designed, multi-element assembly deferred as a block (a whole future component with its own devices and points — see [Provisional Scope](provisional.md)). This element is specifically for one named point.

---

## 1. Definition

A reserved I/O item lives in the **architecture layer**, added during Stage 8 (Controller/Panel I/O Assignment). It attaches to a `system` or `component` (whichever is known) and describes an anticipated point without requiring a device or takeoff source, because none exists yet.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Must come from source docs? |
|---|---|---|---|---|
| `id` | string | yes | project-unique | no |
| `anticipated_purpose` | text | yes | free text, e.g. "OR corridor DP monitoring" | no |
| `signal_type` | enum | yes | same allowed values as `point.signal_type` | no |
| `confidence_level` | enum | yes | `low` \| `medium` \| `high` | no |
| `origin` | text | yes | who/what raised it — e.g., "review meeting 2026-09-12, owner rep" | no |
| `status` | enum | yes | `reserved` \| `promoted` \| `dropped` | no |
| `provisioning` | enum | yes | `none` \| `space_only` \| `space_and_power` \| `space_power_and_wireway` \| `purchased` | no |
| `linked_point` | reference | if status=promoted | id of the resulting `point` | — |
| `linked_controller` | reference | yes | the controller/panel whose capacity this draws against | no |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| attaches_to | `system` or `component` | 1 |
| draws_capacity_from | `controller` or `panel` | 1 |
| promotes_to | `point` (once status = promoted) | 0–1 |

---

## 4. Source Traceability

A reserved item's `origin` is itself the source — a meeting, a review comment, a sidebar conversation — even though it is not a design document. It is recorded the same way a field change is: who said it and when, so the reservation is traceable even though nothing was drawn yet.

---

## 5. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| Every reserved item names a `linked_controller` with enough capacity for its `provisioning` level | consistency | Flag at Stage 8 finalize |
| A `promoted` item has a `linked_point` | completeness | Block promotion |
| `provisioning: purchased` items count against the controller's purchased spare, not just physical spare | consistency | Reconcile at Stage 8 |

---

## 6. Interaction with Spare Capacity

Generic spare capacity (a percentage or fixed count, from the project spec or the KIS default policy) is calculated **on top of** reserved I/O, not instead of it. Order of application at Stage 8 finalize:

1. Base point count (from the approved model).
2. Named reserved I/O items (this element) — each consumes capacity at its stated `provisioning` level.
3. Generic spare capacity policy applied to the total of 1 + 2.
4. Round the result up to the nearest module chunk size for the chosen controller/platform.
5. For the resulting spare chunks, decide purchase-now vs. provision-only per chunk (a cost decision, not an engineering one) — see [Controller/Panel I/O Assignment, Stage 8](../../design-playbooks/controls-engineering-playbook.md).

---

## 7. Example

```yaml
id: RES-002
anticipated_purpose: "OR corridor differential pressure monitoring (2 points)"
signal_type: AI
confidence_level: high
origin: "Design review 2026-09-12; owner initially declined, now requested"
status: reserved
provisioning: space_and_power
linked_controller: PNL-OR-1
```

---

## 8. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
