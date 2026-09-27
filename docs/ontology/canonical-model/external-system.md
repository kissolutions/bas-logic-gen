---
title: "External System Reference"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: takeoff
---

# External System Reference

## 0. Purpose

**What this element represents:** Equipment or a system outside this project's design scope that one of our systems exchanges I/O with — most often a downstream unit whose status feeds back into our permissive/interlock logic, or a signal we hand off outward. We are not taking off its components, devices, or points; we only need enough to wire, configure, and document our side of the interface.

**Why the model needs it:** Without this element, the only options are (a) model the other system fully, as if we were designing it — wrong, because we aren't and shouldn't invent its internals — or (b) leave the connection undocumented, which loses exactly the kind of fact an RFI or a commissioning tech needs (what is this wire, where does it come from, has anyone confirmed it). An external system reference is deliberately minimal: a name, what it provides or receives, and whether that's been field-verified — nothing more.

**What this element does NOT do:** it never gets its own `component`/`device`/`point` breakdown. If a project later takes on design responsibility for that system, it stops being an `external_system` reference and becomes a real `system` with its own takeoff.

---

## 1. Definition

An external system reference is a minimal, takeoff-layer citation to equipment outside this project's design scope, sufficient to document an inbound or outbound I/O connection without modeling the equipment itself.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `id` | string | yes | the tag as shown on our drawings/schedules | if the source tag itself looks inconsistent or mislabeled, record it as-is and raise an `open_item` rather than silently correcting it |
| `name` | text | yes | best-available plain-language description of what it is | mark as unconfirmed if the source documents don't clearly identify it |
| `io_provided` | text | yes | free text, e.g., "1x dry contact, N.O., indicates unit running" | describes the signal(s) crossing the boundary, not the far-side equipment's internals |
| `direction` | enum | yes | `inbound` \| `outbound` \| `bidirectional` | relative to our system |
| `verification_status` | enum | yes | `unverified` \| `field_verified` | mirrors a source document's own "verify terminations" style callouts — do not default to `field_verified` |
| `source_ref` | list of [Source Reference](source-reference.md) | yes, 1+ | — | — |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| referenced_by | `point` (via its optional `external_source` attribute — see [Point](point.md) §2) | 0–n |

An `external_system` is never the target of `dependency` — `dependency` connects two systems both present in this project's own model. Use this element instead when the other side is out of scope.

---

## 4. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| `verification_status: unverified` items are still open at the Model Checkpoint | — | Not a failure by itself, but pair it with an `open_item` (`kind: missing` or `rfi`, `resolve_by: commissioning`) so it's tracked, not forgotten |
| Every `point.external_source` reference resolves to a real `external_system` id | consistency | Flag |

---

## 5. Example

```yaml
# anonymized instance
id: EXT-01
name: "Downstream dishwasher exhaust unit — running status feedback (tag on source drawing appears mislabeled; see OI-0xx)"
io_provided: "1x dry contact, N.O., closes on unit running"
direction: inbound
verification_status: unverified
source_ref:
  - {kind: controls_diagram, doc_id: "AHU-1,2,3 instrumentation diagram", location: "dry contact interlocks note", revision: "as-built"}
```

---

## 6. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
