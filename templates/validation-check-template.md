---
# ==============================
# KIS KNOWLEDGE MODEL METADATA
# ==============================
page_type: validation-check
parent_type: playbook
page_status: stub | draft | validated | deprecated
confidence_level: low | medium | high

domain_primary: bas_logic
check_id: <VC-xxx>
stage: model | architecture | logic | field
severity: blocking | major | advisory
automatable: yes | partial | no

# ------------------------------
# AI INVOCATION & REASONING
# ------------------------------
ai_role: verification
ai_priority: high | medium

invocation_triggers:
  stage_signals:
    - "<example: canonical model complete, before logic architecture>"

related_pages:
  upstream:
    - <logic patterns / model elements this check protects>
  downstream:
    - <tools/ script that runs it, when it exists>
---

# <VC-xxx> — <Check Name>

<!--
VALIDATION CHECK TEMPLATE (extension type; parent: playbook)
Purpose: One prescriptive check that catches one class of defect.
-->

## 0. What This Check Detects

## 1. Why It Matters

<!-- Consequence in the field if the defect ships. -->

## 2. Applies To

- **Stage:** model / architecture / logic / field
- **Systems / patterns:** {{list}}

## 3. Inputs and Preconditions

## 4. Procedure

1. {{Step}}

## 5. Pass Criteria

## 6. On Failure

| Finding | Classification | Action |
|---|---|---|
| {{finding}} | engineering decision / programming defect / RFI | |

## 7. Automation

<!-- Whether and how a tool in tools/ can run this check; what it needs from the model. -->

## 8. Status and Stewardship

- **Status:** Stub / Draft / Validated / Deprecated
- **Last Review:** YYYY-MM
- **Owner:**
