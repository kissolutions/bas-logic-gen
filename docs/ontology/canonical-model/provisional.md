---
title: "Provisional Scope"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: any
---

# Provisional Scope

## 0. Purpose

**What this element represents:** A wrapper placed exactly where its contents would sit in the model if they weren't deferred — a whole, already-designed chunk (one or many `component`, `device`, `point`, `system`, `controller`, or other elements together, in their normal shape) that isn't committed yet, pending an owner decision, a budget approval, an alternate/allowance line item, or a future contract phase.

**Why the model needs it:** `reserved_io` covers one named anticipated point. `controller.provisioning` covers pure hardware headroom with no content decided yet. Neither covers a fully-specified assembly — say, a whole future economizer package (a component, its devices, and their points together) — that's ready to build the moment it's approved, and that belongs, structurally, exactly where its real counterpart would go. This element holds that case without duplicating the tree or inventing a second location for it to live.

**What this element does NOT do:** it doesn't decide anything itself. Whether and when its contents become real is tracked by a linked `open_item` — this element is purely structural, a location marker and a container.

---

## 1. Definition

A provisional scope is a container placed at the exact position in the model its contents would occupy if committed, holding one or more otherwise-normal elements unchanged in shape. When its linked decision resolves in favor of building it, its children are re-parented onto the wrapper's own parent — in place — and the wrapper is removed. Nothing about the children's own structure changes at that point.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | project-unique | — |
| `label` | text | yes | short description of the chunk as a whole, e.g., "Phase 2 economizer package" | — |
| `contains` | list of element | yes, 1+ | any element type(s) that would be valid at this position in the tree if not deferred | children keep their normal attributes, including their own `source_ref`; they are not simplified for being provisional |
| `status` | enum | yes | `deferred` \| `dropped` | there is no `approved` state — approval means extraction, which removes the wrapper entirely (see §4) |
| `linked_open_item` | reference | yes | an `open_item` id | carries the *why* and *who decides* — see §3 |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | e.g., a bid alternate, an allowance line item, a basis-of-design narrative, a review conversation |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| contains | any normally-valid element for this tree position | 1–n |
| tracked_by | `open_item` | 1 |

A `provisional` wrapper does not carry its own approval workflow — that's `open_item`'s job (`status`, `owner`, `resolve_by`, `answer`). This element only answers "what would go here, and is it still on hold."

---

## 4. Extraction Procedure

Extraction is the only way a `provisional` wrapper's contents become real, and it is the same operation regardless of what's inside — one point, or a whole assembly:

1. Confirm the `linked_open_item` is `answered`/`closed` with an approval, and note its answer's `source_ref` (a change order, a written owner approval, a board action) — the wrapper's own `status` is not what authorizes this.
2. Re-check every `id` inside the wrapper for uniqueness against the *current* project state, not just against what existed when the wrapper was authored — time may have passed, and something else may have claimed that id since.
3. Re-parent each direct child of the wrapper onto the wrapper's own parent, in place. No other structural change.
4. Delete the `provisional` element itself.
5. From this point on, the promoted elements participate normally in every generated view and every capacity/spare calculation — nothing marks them as having once been provisional.

---

## 5. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| Every element inside `contains` independently satisfies its own element type's validation rules | consistency | Flag — it should be ready to extract the moment it's approved, not discovered incomplete at that point |
| Contents of a `provisional` wrapper are excluded from all generated views (I/O list, device list) and from current-state capacity/spare-percentage math | — | Not a failure; this is the intended behavior |
| A `linked_open_item` exists and is not itself `closed` with a *rejection* while the wrapper's `status` is still `deferred` | consistency | Flag — a rejected item should move the wrapper to `status: dropped`, not sit inconsistent |
| `status: dropped` wrappers are retained (not deleted) for traceability, and stay excluded from every view | — | Not a failure |
| Nesting a `provisional` wrapper inside another is allowed, and represents an independently-decidable sub-contingency | — | Not a failure; extraction is still done one level at a time |

---

## 6. Example

```yaml
# anonymized instance
id: PROV-04
label: "Phase 2 economizer package for AHU-1 (contingent on owner budget approval)"
status: deferred
linked_open_item: OI-030
contains:
  - component:
      id: ECON-1
      type: economizer_damper
      attaches_to: AHU-1
      source_ref:
        - {kind: spec, doc_id: "23 09 00", location: "Alternate No. 3", revision: "Rev 1"}
      # ... devices and points under ECON-1, in their normal shape,
      # exactly as they'd be written if this were committed scope
source_ref:
  - {kind: spec, doc_id: "23 09 00", location: "Alternate No. 3 narrative", revision: "Rev 1"}
```

---

## 7. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
