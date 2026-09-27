---
title: "Source Reference"
page_type: model-element
parent_type: ontology
page_status: draft
template: templates/model-element-template.md
model_layer: any
---

# Source Reference

## 0. Purpose

**What this element represents:** A citation attached to any other element or attribute, saying exactly which document (or field agreement, or narrative decision) it came from. This is what makes the model traceable and lets Gate G3 (source conflict) work — two conflicting values are only a conflict if each one names where it came from.

**Why the model needs it:** Every fact in the canonical model must trace to something. Without a structured citation, "why does the model say this" always ends in someone's memory of a document instead of the document itself.

---

## 1. Definition

A source reference names one document (or non-document source — see `kind` below) and the specific place in it the fact came from.

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Notes |
|---|---|---|---|---|
| `doc_id` | string | yes, unless `kind` is non-document | matches an entry in the Stage 1 source inventory | — |
| `kind` | enum | yes | `soo` \| `schedule` \| `controls_diagram` \| `floor_plan` \| `elevation_riser` \| `spec` \| `submittal` \| `field_change` \| `rfi_response` \| `review_conversation` \| `engineering_judgment` | `review_conversation` and `field_change` don't need a `doc_id` — see attributes below |
| `location` | string | if `kind` is a document | sheet/section/keyed-note/page, as specific as the document allows | e.g., "M-601, sheet 4, keyed note 7" |
| `revision` | string | if `kind` is a document | the document's revision/date at time of citation | needed so a later revision doesn't silently change what was cited |
| `who` | string | if `kind` is `field_change` or `review_conversation` | name/role | who said it |
| `when` | date | if `kind` is `field_change` or `review_conversation` | — | when it was said |
| `note` | text | no | free text | extra context, e.g. quoting the exact keyed note text |

---

## 3. Relationships

A source reference has no children; it attaches *to* other elements (every model element and most of their attributes carry one or more `source_ref` values).

---

## 4. Validation Rules

| Rule | Type | On failure |
|---|---|---|
| `kind: soo \| schedule \| controls_diagram \| floor_plan \| elevation_riser \| spec \| submittal` requires `doc_id`, `location`, `revision` | completeness | Flag at takeoff |
| `kind: field_change \| review_conversation` requires `who` and `when` | completeness | Flag when the citing element is created |
| `doc_id` must exist in the Stage 1 source inventory | consistency | Flag — undeclared source |

---

## 5. Example

```yaml
kind: schedule
doc_id: M-601
location: "sheet 4, AHU schedule, AHU-1 row"
revision: "Rev 2, 2026-08-14"
```

```yaml
kind: review_conversation
who: "J. Carithers, design review 2026-09-12"
when: 2026-09-12
note: "Owner requested 2 additional OR corridor DP points, previously declined"
```

---

## 6. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
