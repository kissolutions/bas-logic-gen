  # BAS Logic Generator

@../knowledgebase_wikijs/AI/BAS-ENGINEERING-CONTEXT.md

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

## Knowledgebase

The knowledgebase_wikijs repository is the authoritative engineering
reference for this project.

Use it to locate applicable:
- engineering workflow guidance;
- system architecture;
- reusable control patterns;
- physical device information;
- integration methods;
- constraints and known failure modes;
- validated implementation examples.

Do not treat an example implementation as a universal standard.

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

Do not modify engineering standards in the knowledgebase merely to make
implementation easier.