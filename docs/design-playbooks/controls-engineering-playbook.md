---
title: "Controls Engineering Playbook"
page_type: playbook
playbook_subtype: process
page_status: draft
confidence_level: medium
template: knowledgebase_wikijs:templates/playbook-template.md

domain_primary: bas_controls_engineering
ai_role: workflow_router
ai_priority: high

invocation_triggers:
  programmatic_signals:
    - "new controls project received"
    - "request to produce device list, I/O list, controls narrative or control logic"

decision_axes:
  - which stage the project is in
  - whether a checkpoint may be passed
  - where to loop back when a later stage exposes a gap

related_pages:
  upstream:
    - governance-and-doctrine/source-documents-and-authority.md
    - ontology/canonical-model/canonical-model-vocabulary.md
  downstream:
    - design-playbooks/systems/
---

# Controls Engineering Playbook

<!--
MASTER PROCESS PLAYBOOK.
playbook_subtype "process" is not yet in the framework's playbook template;
the section layout below adapts it. Candidate framework change: a process-playbook template.
-->

## 1. Purpose and Use

This playbook defines **how KIS Solutions takes a controls project from mechanical design documents to validated control logic**. It is the entry point for every project and the router to every other page in this repository.

It covers the whole controls engineering workflow. Producing control logic is its second half; the first half produces the canonical model, the device list, the I/O list and the controls narrative that logic depends on.

---

## 2. Design Intent

- **One source of truth.** The canonical model is the project record. The device list, I/O list, controls narrative skeleton and code point tables are generated views of it, never edited separately.
- **Facts before decisions.** What the documents say is recorded separately from what we decide.
- **Every fact is traceable.** Every model attribute carries the source document, sheet or section, and revision it came from. Conflicts become RFIs that cite both sources.
- **Checkpoints are frozen.** Once a checkpoint is passed, downstream stages add to the model but never edit what was approved. Changing approved content reopens the checkpoint.
- **Behavior is not takeoff.** The model records what exists; the narrative describes what it does; logic implements the narrative.

---

## 3. The Canonical Model

The canonical model is an XML document built from a standard element vocabulary (see [Canonical Model Vocabulary](../ontology/canonical-model/canonical-model-vocabulary.md)). It is organized by physical makeup: project → system → component → device → point, with features, dependencies, open items and source references attached where they apply.

It is built in **layers**. Each stage adds its own layer and does not edit earlier ones.

| Layer | Added by stage | Contains |
|---|---|---|
| Takeoff | 3 | What the documents show: systems, components, devices, points, features, dependencies — with sources |
| Selection | 4 | Engineering decisions on devices: device chosen, range, sizing basis |
| Architecture | 8 | Controller assignment, I/O terminals, addressing |

**Generated views**

| View | Available after | Used for |
|---|---|---|
| Draft device list, draft I/O list | Stage 4 | Team takeoff comparison at the model checkpoint |
| Open item / RFI log | Any stage | Tracking conflicts and missing information |
| Final I/O list | Stage 8 | Construction documents, programming |

---

## 4. Stage Map

| # | Stage | Output | Gate |
|---|---|---|---|
| 1 | Source Intake | Source inventory | — |
| 2 | System Identification | System inventory | — |
| 3 | Takeoff | Model: takeoff layer | — |
| 4 | Device Selection | Model: selection layer | — |
| 5 | **Model Checkpoint** | Approved model, draft lists, RFI log | Team agreement, RFIs resolved |
| 6 | Controls Narrative | Controls narrative | — |
| 7 | **Narrative Checkpoint** | Approved narrative | Approver: *pending decision* |
| 8 | Logic Architecture | Model: architecture layer; final I/O list | — |
| 9 | Pattern Selection | Pattern list with parameters | — |
| 10 | Archetype Binding | Archetypes, variants, tokens filled | — |
| 11 | Assembly | Controller code | — |
| 12 | Validation | Validation report | Logic and field checks |

Stages 1–5 are specified below. Stages 6–12 are outlined and will be specified in later passes.

---

## 5. Stage Specifications

Each stage states: entry criteria, inputs, knowledge to consult, decisions made (engineering or programming), output, exit criteria, and loop-back rules.

### Stage 1 — Source Intake

- **Entry:** project documents received.
- **Inputs:** everything available. See [Source Documents and Authority](../governance-and-doctrine/source-documents-and-authority.md) for the document types and what each governs.
- **Actions:**
  - Inventory every document with its identifier, revision and date.
  - Identify whether **approved equipment submittals** exist for any equipment. If they do, those units are designed around the submitted equipment (see Decision Gate G1).
  - Identify which specification governs each topic: Division 23 09 00 first, then other project spec sections, then client general specs.
- **Decisions:** none; this stage records.
- **Output:** source inventory.
- **Exit:** every document has an identifier and revision; submittal status known per equipment item.

### Stage 2 — System Identification

- **Entry:** source inventory complete.
- **Inputs:** mechanical schedules, controls diagrams, floor plans, risers, approved submittals.
- **Consult:** system playbooks in `design-playbooks/systems/` to classify each system.
- **Actions:**
  - List every piece of controlled equipment by tag.
  - Assign each a system type; this selects its system playbook.
  - Set feature flags (e.g., reheat, economizer, static pressure reset, fan drive type) with a source reference for each.
  - Group identical units into typicals with an instance list.
  - Record dependencies between systems (serves / served-by, shared signals).
- **Decisions:** system type and typical grouping — engineering.
- **Output:** system inventory (the top level of the model).
- **Exit:** every scheduled or diagrammed unit is accounted for.
- **Loop-back / escalate:** equipment type with no system playbook → stop and escalate.

### Stage 3 — Takeoff

- **Entry:** system inventory complete.
- **Inputs:** controls diagrams, schedules, SOO, floor plans and keyed notes, elevations, risers, specifications, approved submittals.
- **Consult:** canonical model vocabulary; system playbook baseline for the system type (what a unit of this type normally has).
- **Actions:**
  - Build the takeoff layer for each system: components, devices, points, features, dependencies.
  - Record a source reference on every element and attribute.
  - Where two sources disagree, record both values with both sources and raise an open item. Do not pick one.
  - Where the system playbook baseline expects something the documents do not show, raise an open item rather than adding it.
  - Use standard vocabulary elements; anything outside the vocabulary follows the extension rule on the vocabulary page.
- **Decisions:** none. The takeoff layer contains facts only.
- **Output:** model, takeoff layer.
- **Exit:** every source document has been taken off; every element has a source.

### Stage 4 — Device Selection

- **Entry:** takeoff layer complete.
- **Inputs:** takeoff layer; schedules (capacities, maximum static pressures, etc.); specifications; approved submittals.
- **Consult:** device pages in knowledgebase_wikijs (`Devices/`).
- **Actions:** for each device, record in the selection layer the device chosen, its range, and the sizing basis (the scheduled value and source it was sized from).
- **Decisions:** device and range — engineering.
- **Output:** model, selection layer.
- **Exit:** every point used for control or visualization has a range and sizing basis.

### Stage 5 — Model Checkpoint

- **Entry:** takeoff and selection layers complete.
- **Actions:**
  - Run model-stage validation checks (`validation/`, stage = model).
  - Generate the draft device list, draft I/O list and open item log.
  - The team compares the draft lists against their own takeoffs of the source documents.
  - Issue RFIs for open items that need someone outside the team; record answers in the model with the answer as the source.
- **Exit:** team agrees the model matches the documents; every open item is resolved or explicitly carried with an owner.
- **After exit:** takeoff and selection layers are frozen.

### Stages 6–12 — Outline

- **6 Controls Narrative:** built function by function from the approved model using system playbooks and logic patterns. Where the narrative departs from the mechanical SOO, the departure is recorded as an engineering decision.
- **7 Narrative Checkpoint:** approval of the narrative.
- **8 Logic Architecture:** controller assignment, program segmentation, interfaces; adds the architecture layer; produces the final I/O list.
- **9 Pattern Selection:** choose logic patterns (behavior) for each narrative function.
- **10 Archetype Binding:** choose platform code archetypes and variants; fill tokens from the model.
- **11 Assembly:** produce controller code.
- **12 Validation:** logic-stage and field-stage checks.

---

## 6. Decision Gates

- **G1 — Approved submittals exist for this equipment?**
  - **Yes** → take off I/O and networkable features from the submittal; design around the actual equipment. Where the submittal differs from the design documents, record the difference and its source.
  - **No** → take off from design documents.
- **G2 — Equipment type has a system playbook?**
  - **Yes** → continue.
  - **No** → stop; escalate for a new system playbook or a project-specific approach.
- **G3 — Sources disagree?**
  - **Yes** → record both, raise an open item. Never resolve by silent precedence unless a rule on the source authority page covers it.
- **G4 — Model checkpoint passed?**
  - **Yes** → continue to the narrative.
  - **No** → loop back to the stage that owns the defect (takeoff or selection).
- **G5 — A later stage needs to change approved takeoff or selection content?**
  - **Yes** → reopen the model checkpoint for that item.

---

## 7. Common Failure Modes

- Adding "obviously needed" devices during takeoff instead of raising an open item.
- Resolving document conflicts by judgment and losing the source trail.
- Editing a generated list instead of the model.
- Starting behavior description before the model checkpoint.

---

## 8. Open Decisions

- Who approves the controls narrative at stage 7 (internal only, or engineer of record / owner).
- Precedence rules between specific document types, beyond submittals (see source authority page).

---

## 9. Status and Stewardship

- **Status:** Draft
- **Last Review:** 2026-09
- **Owner:** KIS Solutions
