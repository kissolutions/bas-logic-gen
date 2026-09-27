---
title: "Open Item"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
---

# Open Item

## 0. Purpose

**What this element represents:** A conflict, gap, or unresolved decision found while building the canonical model or a downstream deliverable. It replaces silently guessing.

**Why the model needs it:** Every conflict between source documents, or every place a system playbook's baseline expects something the documents don't show, must be visible and trackable rather than resolved by judgment and lost.

---

## 1. Definition

An open item records: what is missing or conflicting, which element(s) it attaches to, the competing values and their sources (if a conflict), and — critically — **which milestone it must be resolved before**.

Not every open item blocks the very next checkpoint. An item is only a blocker for the checkpoint named in its `resolve_by` attribute.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Must come from source docs? |
|---|---|---|---|---|
| `id` | string | yes | project-unique | no |
| `kind` | enum | yes | `conflict` \| `missing` \| `vocabulary` \| `rfi` \| `negotiation` | no |
| `attaches_to` | reference | yes | element id(s) in the model | no |
| `description` | text | yes | free text | no |
| `competing_values` | list | if kind=conflict | value + source_ref per option | yes (each value) |
| `resolve_by` | enum | yes | `model_checkpoint` \| `submittal_review` \| `commissioning` \| other named milestone | no |
| `status` | enum | yes | `open` \| `answered` \| `closed` | no |
| `owner` | string | no | person/role responsible | no |
| `answer` | text | if status≠open | the resolution, with its own source reference | when answered |

---

## 3. Relationships

| Relationship | Target element | Cardinality |
|---|---|---|
| attaches_to | any canonical-model element | 1–n |
| raised_by | source_ref (the document/comparison that surfaced it) | 1 |
| resolved_by | source_ref (the answer's source — RFI response, field agreement, owner correspondence) | 0–1 |

---

## 4. Source Traceability

Every open item records where it was raised (which document comparison or playbook-baseline check surfaced it) and, once answered, the source of the answer. An open item is never resolved without a recorded source for the answer — including a **field change**, whose source is the informal approval itself (who approved it, and when).

---

## 5. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| Every open item has a `resolve_by` | completeness | Cannot pass the model checkpoint with an item defaulted to "now" that was never actually assessed |
| An item cannot be `closed` without an `answer` and a source | consistency | Block closure |
| A `resolve_by: model_checkpoint` item is still `open` at the model checkpoint | completeness | Blocks the checkpoint (see Controls Engineering Playbook, Gate G4) |
| A `resolve_by` later than `model_checkpoint` (e.g., `commissioning`) is allowed to remain open past earlier checkpoints | — | Not a failure; this is the intended behavior |

---

## 6. RFI Triggers

An open item of kind `rfi` is one that needs someone outside the team to answer. It becomes a formal RFI when the team cannot resolve it from information already available.

---

## 7. Example

```yaml
# anonymized instance
id: OI-014
kind: negotiation
attaches_to: [system:AHU-1, device:AHU1-CTRL]
description: >
  Owner has an existing BACnet instance ID numbering scheme for this
  campus; not yet provided. Instance IDs assigned here are provisional.
resolve_by: commissioning
status: open
owner: controls_engineer
```

```yaml
id: OI-021
kind: conflict
attaches_to: [point: AHU1-FRZ]
description: >
  Freezestat shown on M-501 floor plan keyed note 7, not listed on the
  I/O schedule (sheet M-601 rev 2).
competing_values:
  - value: "present, hardwired safety"
    source_ref: "M-501, keyed note 7"
  - value: "not present"
    source_ref: "M-601 rev 2, I/O schedule"
resolve_by: model_checkpoint
status: open
```

---

## 8. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
