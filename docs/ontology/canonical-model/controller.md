---
title: "Controller"
page_type: model-element
parent_type: ontology
page_status: stub
template: templates/model-element-template.md
model_layer: architecture
---

# Controller

<!--
STUB. Added by Stage 8 (Controller/Panel I/O Assignment) — see
design-playbooks/controls-engineering-playbook.md.

Represents one piece of controller/remote-I/O hardware (not a field device —
see device.md — and not the enclosure — see panel.md). Hosts points and
reserved_io items up to its per-type capacity.

Attributes to define: platform, model/family, device_ref, point_capacity by
type, points_assigned (refs), reserved_io_assigned (refs),
spare_capacity_purchased, spare_capacity_provisioned_only, parent panel.

AUTHORITY: capacity numbers (point count by type, module chunk sizes, form
factor) are NOT invented or duplicated here. They are physical device facts,
authoritative in knowledgebase_wikijs under Devices/Controllers/<platform>/
(hardware-template), the same as any field device (KMC controller pages, PLC
pages, or other controller hardware pages). This element's `device_ref`
attribute points to that wiki page; a project instance cites it rather than
restating it. bas-logic-gen's platforms/<platform>/controller-mappings/
pages describe how KIS USES a given model (selection guidance, our
conventions) and also cite the wiki page for facts — they do not hold the
numbers either.
-->
