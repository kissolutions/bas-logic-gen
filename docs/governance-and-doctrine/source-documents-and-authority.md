---
title: "Source Documents and Authority"
page_type: governance
page_status: draft
---

# Source Documents and Authority

Defines the documents a controls project is taken off from, what each one governs, and how conflicts between them are handled. Used at stages 1–5 of the [Controls Engineering Playbook](../design-playbooks/controls-engineering-playbook.md).

---

## 1. Document Types

| Document | Governs (primary use) | Does not govern | Typical defects |
|---|---|---|---|
| **Specification — Division 23 09 00** | Our home spec: controls scope, products, execution requirements | Equipment capacities | Boilerplate that contradicts drawings |
| Other project specification sections | Topics 23 09 00 references or omits | — | Scope split across divisions |
| **Client general specifications** | Gaps the project specs leave — especially networking, graphics, power | Anything the project specs cover | Out of date relative to project |
| Mechanical schedules | Equipment type, capacities, fan drive type, speeds, maximum static pressures | Control behavior | Revisions out of step with drawings |
| Controls diagrams | Points present and device types (e.g., averaging sensor, airflow station type, DP switch) | Point naming | Points not reflected elsewhere |
| Sequence of operations (SOO) | Design intent and required features | Precise behavior | Vague, contradictory, incomplete |
| Floor plans and keyed notes | Device locations and counts | Device types | Keyed notes disagree with schedules |
| Elevations and risers | Physical and network topology, mounting | Behavior | Incomplete |
| **Approved equipment submittals** | The actual equipment's I/O and networkable features | — | May differ from design documents |

---

## 2. Authority Rules

1. **Approved submittals supersede design documents** for the equipment's actual I/O and networkable features. Design pivots around the submitted equipment. The difference from the design documents is recorded with both sources.
2. **Specifications:** Division 23 09 00 first; other project sections where 23 09 00 is silent or refers out; client general specifications only where project specifications are silent.
3. **All other conflicts produce an open item.** Both values and both sources are recorded. No document silently outranks another unless a rule is added here.

<!-- Add precedence rules here only when they are standing KIS policy. -->

---

## 3. Source Reference Format

Every model element and attribute carries a source reference so conflicts can be traced and RFIs written with both citations.

<!-- Format to be defined with the canonical model vocabulary: document id, sheet/section, detail or keyed note, revision. -->

---

## 4. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
