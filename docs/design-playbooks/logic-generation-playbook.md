---
title: "Logic Generation Playbook"
page_type: playbook
page_status: stub
template: knowledgebase_wikijs:templates/playbook-template.md
---

# Logic Generation Playbook

<!--
STUB. Master playbook: this page encodes the engineering workflow.
Absorbs the former pages Overview, Engineering-Workflow, Logic-Generation-Workflow
and Canonical-Model-Checkpoint (the checkpoint becomes a Decision Gate here).
To be written from the playbook template.
-->

Workflow to be expanded (from CLAUDE.md):

1. Interpret source engineering documentation.
2. Build the canonical system model.
3. Validate the model against source documents and applicable knowledge.
4. Identify conflicts, omissions and unresolved engineering decisions.
   - **Decision gate — Canonical Model Checkpoint:** the model is complete and every unresolved requirement is either answered or carried as an RFI → proceed; otherwise stop.
5. Build the software / control architecture.
6. Select and fill code archetypes.
7. Validate the resulting logic.
