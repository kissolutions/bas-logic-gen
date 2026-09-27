# BAS Logic Generator

@../knowledgebase_wikijs/AI/AGENT-FRAMEWORK-GUIDE.md

## Purpose

This repository develops the framework for taking a controls project from
mechanical design documents to structured, reviewable, and ultimately
executable control logic.

## Engineering Workflow

The workflow, its stages, checkpoints and decision gates are defined in
docs/design-playbooks/controls-engineering-playbook.md. Follow it; do not
skip stages. In summary:

1. Source intake → 2. System identification → 3. Takeoff →
4. Device selection → 5. MODEL CHECKPOINT → 6. Controls narrative →
7. NARRATIVE CHECKPOINT → 8. Logic architecture → 9. Pattern selection →
10. Archetype binding → 11. Assembly → 12. Validation

The canonical model is an XML takeoff of the source documents, built in
layers (takeoff, selection, architecture), vendor-neutral, with a source
reference on every element. It is the single source of truth: the device
list, I/O list and other lists are generated from it, never edited
separately. Approved layers are frozen; changing them reopens the
checkpoint. The takeoff layer contains facts only — never add devices or
behavior the documents do not show; raise an open item instead.

## Knowledgebase and Authority

This repository is a domain extension of the KIS knowledge framework.

The knowledgebase_wikijs repository is authoritative for:
- the framework: page taxonomy, metadata conventions, templates, reasoning
  model and framework vocabulary;
- physical device pages (Devices/).

This repository is authoritative for all BAS logic content: definitions,
canonical model, logic patterns, concepts, playbooks, constraints, platform
conventions, code archetypes, reference implementations, historical
examples and validation checks. BAS-specific settings and vocabulary live
here, never in knowledgebase_wikijs.

Documentation lives in docs/ and follows the framework:
- Folders organize domains of knowledge, not workflow steps.
- Every page declares page_type in frontmatter and follows its template.
- Extension page types are listed in
  docs/governance-and-doctrine/page-type-registry.md; their templates are
  in templates/.
- Device facts are linked from knowledgebase_wikijs, not copied.
- Use docs/governance-and-doctrine/definitions.md for BAS terms. Terms
  marked "convention pending" are not yet defined; do not assume a meaning.

Do not treat an example implementation as a universal standard.
Historical examples are precedent; logic patterns define behavior; code
archetypes implement it.

## Engineering Discipline

Do not silently invent missing engineering requirements.

When information conflicts or is incomplete:
- identify the issue;
- identify what portions of the system model or implementation are affected;
- distinguish engineering decisions from programming decisions;
- preserve source traceability where practical.

## Repository Boundary

This repository contains schemas, canonical models, implementation
architecture, generators, tests, and generated logic.

Do not modify the framework or device pages in knowledgebase_wikijs
merely to make implementation easier. If the framework is insufficient,
propose a framework change there.