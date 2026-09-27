---
title: "KMC Architecture Constraints"
page_type: constraint-synthesis
page_status: stub
template: knowledgebase_wikijs:templates/template-constraint-synthesis.md
---

# KMC Architecture Constraints

<!--
STUB. This page is authoritative for the SPARE-CAPACITY POLICY and MODULE
CHUNK SIZES referenced by Stage 8 (Controller/Panel I/O Assignment) in
design-playbooks/controls-engineering-playbook.md. Capacity numbers below
are placeholders — do not treat as real KMC hardware facts until filled in
against actual datasheets / platform product-class pages.
-->

## Spare Capacity Policy (precedence)

1. **Project specification**, if it states a spare-capacity requirement — governs.
2. **KIS default policy** — 20% spare, absent a spec requirement.
3. **Adjustments on top of the default:**
   - Named [Reserved I/O](../../ontology/canonical-model/reserved-io.md) items (flagged in review or sidebar conversation) are layered on top of the percentage, not counted as part of it.
   - Confidence level of specific I/O or narrative functions may justify carrying more or less margin than the flat default.

## Module Chunk Sizes

<!-- TBD: per KMC controller/remote-I/O module family, the point capacity by
type (AI/AO/BI/BO). Source from product-class pages
(platforms/kmc/controller-mappings/) or manufacturer datasheets, not memory. -->

## Purchase vs. Provision-Only

Spare and reserved capacity, once rounded up to a module chunk, is not automatically purchased. For each chunk, Stage 8 records whether it is:

- **Purchased now** — module bought and installed.
- **Provisioned only** — rack/gutter space, power budget and wireway capacity reserved on the panel and wiring schematics, module itself deferred until needed.

This avoids paying for remote I/O modules that sit unused, while keeping the physical/electrical accommodation real on the submitted drawings so a later addition doesn't require re-drawing panel layout or wiring schematics.

## Status and Stewardship

- **Status:** Stub — policy structure defined in the Controls Engineering Playbook; real KMC capacity numbers pending
- **Owner:** KIS Solutions
