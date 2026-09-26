---
# ==============================
# KIS KNOWLEDGE MODEL METADATA
# ==============================
page_type: logic-pattern
parent_type: functional-construct
page_status: stub | draft | validated | deprecated
confidence_level: low | medium | high

domain_primary: bas_logic
pattern_family: <equipment-command | alarm-handling | safety-handling | setpoint-handling | sequencing | control-loop | primitive>
vendor_neutral: true

# ------------------------------
# AI INVOCATION & REASONING
# ------------------------------
ai_role: behavior_definition
ai_priority: high | medium | low

invocation_triggers:
  model_signals:
    - condition: "<example: canonical model contains a commanded fan with a status point>"
  uncertainty_flags:
    - "<example: proof method not specified>"

decision_axes:
  - "<example: proof method>"
  - "<example: failure response>"

implementations:
  - <code-archetype page per platform>

related_pages:
  upstream:
    - <model-element or wiki physical entity pages>
  lateral:
    - <interacting logic patterns>
  downstream:
    - <code archetypes, system playbooks>
---

# <Logic Pattern Name>

<!--
LOGIC PATTERN TEMPLATE (extension type; parent: functional construct)
Purpose: Define a control BEHAVIOR independent of any platform.
Answers: "What must this logic do, with what inputs, under all conditions?"
Do NOT include:
- Platform code or object names (→ Code Archetype)
- Device specifications (→ wiki Physical Entity page)
- Project-specific values (→ Design / Historical Example)
-->

## 0. Purpose and Scope

**Behavior defined:**  
**Why it exists:** (the risk or requirement it handles)  
**Out of scope:**

---

## 1. Required Behavior

<!-- ANCHOR: BEHAVIOR — plain language first, then a truth table or state table if the behavior has more than two conditions. -->

| Condition | Required response |
|---|---|
| {{Condition}} | {{Response}} |

---

## 2. Inputs and Outputs

<!-- Use canonical model roles, not point names. -->

| Direction | Canonical role | Type | Required? | Notes |
|---|---|---|---|---|
| In | {{e.g., equipment_status}} | binary | yes | |
| Out | {{e.g., equipment_alarm}} | binary | yes | |

---

## 3. Parameters

<!--
ANCHOR: PARAMETERS
Classify every parameter. Engineering decisions must trace to a source document or an RFI;
programming decisions may take the default.
-->

| Parameter | Meaning | Default | Range | Decision class |
|---|---|---|---|---|
| {{name}} | | | | engineering / programming |

---

## 4. States and Transitions

<!-- Omit if stateless. Otherwise list states, entry conditions, exit conditions, and outputs per state. -->

---

## 5. Abnormal Conditions

| Condition | Required response |
|---|---|
| Input sensor failed / out of range | |
| Communication loss | |
| Controller restart / power cycle | |
| Manual override (HOA, operator) | |
| Conflicting commands | |

---

## 6. Variants

### {{Variant name}}
- **Select when:**
- **Differs from baseline by:**

---

## 7. Interaction with Other Patterns

<!-- Where this pattern sits in permissive / interlock / safety / alarm chains, and ordering requirements. -->

---

## 8. Assumptions

```yaml
assumptions:
  - id: A-01
    statement: <what must be true for this pattern to be correct>
    invalidated_if: <condition>
    consequence_if_invalid: <effect>
```

---

## 9. Validation Criteria

<!-- What proves an implementation of this pattern is correct. Link validation-check pages. -->

---

## 10. Implementations

| Platform | Code archetype | Status |
|---|---|---|
| KMC | {{link}} | |

---

## 11. Cross-links

## 12. Status and Stewardship

- **Status:** Stub / Draft / Validated / Deprecated
- **Last Review:** YYYY-MM
- **Owner:**
