---
title: "BAS Logic Knowledge Base — Home"
page_type: governance
page_status: draft
---

# BAS Logic Knowledge Base

Domain extension of the KIS knowledge framework (knowledgebase_wikijs) for producing BAS control logic. Start with the [Page Type Registry](governance-and-doctrine/page-type-registry.md).

| Folder | Holds | Page type |
|---|---|---|
| `governance-and-doctrine/` | Registry, BAS definitions, source authority, naming standards | governance |
| `ontology/canonical-model/` | Canonical model vocabulary: the takeoff (what exists, with sources) | model-element |
| `ontology/narrative-model/` | Behavior elements used by the controls narrative (function, setpoint, alarm, ...) | model-element |
| `ontology/logic-patterns/` | Vendor-neutral control behaviors | logic-pattern |
| `concepts/logic-architecture/` | How control software behaves | concept |
| `design-playbooks/` | The controls engineering workflow and per-system strategies | playbook |
| `platforms/<platform>/` | What a controller platform implies; conventions; controller mappings | product-class, playbook, constraint-synthesis |
| `code-archetypes/<platform>/` | Copy-ready implementations: variant menu, tokens, code | code-archetype |
| `reference-implementations/` | Validated complete systems | reference |
| `historical-examples/` | Code from real jobs — precedent only | design |
| `validation/` | Checks that catch defects at model, logic and field stages | validation-check |

Physical device pages live in knowledgebase_wikijs under `Devices/`, not here.
