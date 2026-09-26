# BAS Logic Generator

@../knowledgebase_wikijs/AI/AGENT-FRAMEWORK-GUIDE.md

## Purpose

This repository develops the framework for translating completed BAS
engineering design information into structured, reviewable, and ultimately
executable control logic.

## Engineering Workflow

Do not proceed directly from design documentation to executable code.

1. Interpret source engineering documentation.
2. Build a canonical engineering/system model.
3. Validate that model against the source documentation and applicable
   knowledgebase guidance.
4. Identify conflicts, omissions, and unresolved engineering decisions.
5. Build the software/control architecture.
6. Generate or implement control logic.
7. Validate the resulting logic.

The canonical system model is a formal engineering checkpoint and should
remain independent of vendor-specific executable implementation.

The full workflow, with its decision gates, is encoded in
docs/design-playbooks/logic-generation-playbook.md.

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