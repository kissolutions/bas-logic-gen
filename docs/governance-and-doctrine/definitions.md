---
title: "BAS Logic Definitions"
page_type: governance
page_status: draft
---

# BAS Logic Definitions

Vocabulary for this extension. Framework terms (baseline, variant, decision gate, constraint envelope, failure mode, etc.) are defined in knowledgebase_wikijs `governance-and-doctrine/definitions.md` and are not repeated here.

Each term means **one thing** in this repository. Where a term below carries a KIS-specific convention that has not yet been written down from the working code, it is marked **⚠ convention pending** and must not be relied on until confirmed.

---

## I. Model and Workflow Terms

### Canonical System Model
A vendor-neutral, structured description of a controlled system — points, setpoints, alarms, safeties, modes, relationships and dependencies — built from source documents and validated before any logic is designed. It is a formal checkpoint between engineering and implementation.

### Source Document
A project document that carries engineering authority: sequence of operations, points list, schedules, drawings, specifications, submittals. Authority order is defined on *Source Documents and Authority*.

### Source Traceability
The record of which source document, section and revision each model attribute came from.

### Unresolved Requirement
A required model attribute that is missing or conflicting in the source documents. It is recorded, never silently defaulted, and becomes an RFI when someone outside the team must decide it.

### Engineering Decision
A choice that changes how the equipment or building behaves (e.g., a safety response, a proof delay that protects equipment). Must trace to a source document or an RFI.

### Programming Decision
A choice that changes only how logic is written, not how the system behaves (e.g., variable allocation, program order within the same scan behavior). May take the documented default.

### Canonical Role
The vendor-neutral name of what a point or signal does in a pattern (e.g., `equipment_command`, `equipment_status`), used instead of platform object names.

---

## II. Equipment Command Terms

### Command
The output the controller sends to start, stop, enable or position equipment.

### Status
An input that reports the actual state of equipment, independent of the command.

### Proof
Confirmation, through a status input, that commanded equipment is actually operating (e.g., current switch, differential pressure switch, drive run contact). A proof failure is a mismatch between command and status that persists beyond a proof delay.

### HOA (Hand-Off-Auto)
A local selector that lets equipment be run or stopped outside controller command. In Hand or Off, command and status can legitimately disagree.

### Enable
A binary permission sent to a drive or packaged controller that allows it to run under its own or a separate speed reference.

---

## III. Protective Logic Terms

### Permissive
A condition that must be true before equipment may **start**. A lost permissive may or may not stop running equipment, depending on the pattern.

### Interlock
A dependency that makes one piece of equipment's operation require or forbid another's (e.g., fan must be proven before heating is enabled).

### Safety
A protective condition that must stop or restrict equipment to prevent damage or hazard. Safeties may be hardwired, software, or both; the canonical model records which.

### Trip
The act of a safety stopping equipment. ⚠ convention pending — relationship between *safety* and *trip* as used in the KIS SAFETY-WORD / TRIP-WORD structures.

### Alarm
A notification to operators that a condition needs attention. An alarm does not by itself change equipment behavior.

### Latch
A logic element that holds a state after its triggering condition clears, until a reset.

### Manual Reset / Auto Reset
Whether a latched condition clears only on a deliberate operator action (manual) or automatically once the condition clears (auto).

### First-Out
Identification of which protective condition occurred first when several occur together, so the root cause is not masked. ⚠ convention pending — KIS capture and reset rules.

### Fail-Safe Position
The state an output or device assumes on loss of signal, power or control, determined by the device and the engineering intent.

---

## IV. KIS Word Structures

⚠ convention pending for all three — definitions to be captured from the working KMC code before any logic pattern relies on them.

### SAFETY-WORD
KIS structure for aggregating safety conditions.

### TRIP-WORD
KIS structure for aggregating trip conditions.

### ALARM-WORD
KIS structure for aggregating alarm conditions.

---

## V. Implementation Terms

### Logic Pattern
A vendor-neutral definition of a control behavior. See the page-type registry.

### Code Archetype
The standard copy-ready implementation of a logic pattern on one platform, offered as a menu of variants with named tokens for project values.

### Token
A named placeholder `{{TOKEN}}` in archetype code that is replaced with a project value from the canonical model.

### Reference Implementation
A complete, validated worked system showing archetypes composed together. Demonstrates; does not mandate.

### Historical Example
Code from a real project, kept as precedent. Never a standard.
