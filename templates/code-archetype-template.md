---
# ==============================
# KIS KNOWLEDGE MODEL METADATA
# ==============================
page_type: code-archetype
parent_type: playbook
page_status: stub | draft | validated | deprecated
confidence_level: low | medium | high

domain_primary: bas_logic
platform: <kmc | ...>
controllers:
  - <BAC-5901 | BAC-9300 | ...>
implements:
  - <logic-pattern page(s)>

variants:
  - id: <V1>
    name: <short name>

placeholder_convention: "{{TOKEN}}"   # every project-specific value in a code block is a named token

# ------------------------------
# AI INVOCATION & REASONING
# ------------------------------
ai_role: generation_source
ai_priority: high

invocation_triggers:
  model_signals:
    - condition: "<example: implements pattern X on platform KMC>"

related_pages:
  upstream:
    - <logic-pattern page>
    - <platform product-class page>
  lateral:
    - <archetypes this one exchanges data with>
  downstream:
    - <validation-check pages>
provenance:
  - <historical example this was abstracted from>
---

# <Archetype Name> — <Platform>

<!--
CODE ARCHETYPE TEMPLATE (extension type; parent: playbook)
Purpose: The standard, copy-ready implementation of one or more logic patterns on one platform.
This is a MENU: pick a variant, fill the parameters, paste the block.
Rules:
- Behavior is defined on the logic-pattern page. This page implements it; it does not redefine it.
- Every project-specific value in code is a {{TOKEN}} listed in Section 3.
- Code here must have been run on real equipment or a test bench before status = validated.
-->

## 0. What This Implements

**Logic pattern(s):** {{links}}  
**Platform / controllers:** {{list}}

---

## 1. Selection Menu

| Variant | Select when | Key difference |
|---|---|---|
| V1 — {{name}} | {{condition}} | baseline |
| V2 — {{name}} | {{condition}} | {{difference}} |

---

## 2. Preconditions

- Required points / objects exist (Section 4)
- Required upstream modules present: {{list}}
- Controller resources needed: {{program slots, variables, PID loops, etc.}}

---

## 3. Parameters and Tokens

| Token | Meaning | Default | Range | Decision class | Source in canonical model |
|---|---|---|---|---|---|
| {{TOKEN}} | | | | engineering / programming | |

---

## 4. Points and Objects

| Canonical role | Object type | Naming pattern | Notes |
|---|---|---|---|
| {{role}} | {{e.g., BI / BV / AV}} | {{pattern}} | |

---

## 5. Interfaces

| Direction | Signal / variable | From / to | Meaning |
|---|---|---|---|

---

## 6. Code

### V1 — {{name}}

```text
{{copy-ready code block}}
```

### V2 — {{name}}

```text
{{copy-ready code block}}
```

---

## 7. Integration Steps

<!-- Where the block goes in the program segmentation, execution order, and what else must be edited. -->

1. {{Step}}

---

## 8. Behavior Under Abnormal Conditions

<!-- Confirm how this code meets each abnormal-condition response on the logic-pattern page. Note any gap. -->

| Condition (from pattern) | How this code handles it | Gap? |
|---|---|---|

---

## 9. Verification

| Check | Method | Expected result |
|---|---|---|
| {{validation-check link}} | bench / field / review | |

---

## 10. Platform Limitations and Known Issues

---

## 11. Provenance

- **Abstracted from:** {{historical example(s)}}
- **Validated on:** {{project / bench, date}}

---

## 12. Status and Stewardship

- **Status:** Stub / Draft / Validated / Deprecated
- **Last Review:** YYYY-MM
- **Owner:**
