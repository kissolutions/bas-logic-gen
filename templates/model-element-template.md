---
# ==============================
# KIS KNOWLEDGE MODEL METADATA
# ==============================
page_type: model-element
parent_type: ontology
page_status: stub | draft | validated | deprecated
confidence_level: low | medium | high

domain_primary: bas_logic
model_layer: canonical-system-model
schema_file: <schemas/... when it exists>

# ------------------------------
# AI INVOCATION & REASONING
# ------------------------------
ai_role: model_definition
ai_priority: high

invocation_triggers:
  source_signals:
    - "<example: sequence of operations mentions a setpoint>"
  uncertainty_flags:
    - "<example: element referenced but attributes missing>"

rfi_generation_enabled: true

related_pages:
  upstream:
    - <governance / definitions>
  lateral:
    - <related model elements>
  downstream:
    - <logic patterns that consume this element>
---

# <Model Element Name>

<!--
MODEL ELEMENT TEMPLATE (extension type; parent: ontology)
Purpose: Define one kind of thing the canonical system model contains
(e.g., an I/O point, a setpoint, an alarm, an operating mode).
The canonical model is vendor-neutral. Nothing on this page names a platform object.
-->

## 0. Purpose

**What this element represents:**  
**Why the model needs it:**

---

## 1. Definition

<!-- One precise definition. Link the BAS definitions page for any term used. -->

---

## 2. Attributes

| Attribute | Type | Required? | Allowed values | Must come from source docs? |
|---|---|---|---|---|
| {{name}} | | yes / no | | yes / no |

---

## 3. Relationships

<!-- Which other model elements this one references or is referenced by, with cardinality. -->

| Relationship | Target element | Cardinality |
|---|---|---|

---

## 4. Source Traceability

<!-- What a model instance must record about where each attribute came from (document, sheet, section, revision). -->

---

## 5. Validation Rules

| Rule | Type (completeness / consistency / range) | On failure |
|---|---|---|

---

## 6. RFI Triggers

<!-- Missing or conflicting information that must become an RFI rather than a default. -->

| Missing / conflicting | RFI question | Decision class |
|---|---|---|

---

## 7. Example

```yaml
# anonymized instance
```

---

## 8. Status and Stewardship

- **Status:** Stub / Draft / Validated / Deprecated
- **Last Review:** YYYY-MM
- **Owner:**
