# Disease Mapping Skill

## Skill Overview

This skill provides a structured workflow for pharmaceutical indication disease mapping.

The purpose of this skill is to transform raw pharmaceutical indication descriptions into standardized disease classifications.

The workflow is:

    Pharmaceutical indication description
              ↓
    Normalized Disease Entity
              ↓
    China ICD-10 Medical Insurance Version
              ↓
    Internal Disease Classification


This skill works together with:

- AGENTS.md
- DISEASE_MAPPING_RULES.md
- references/ICD10_REFERENCE.md


## Execution Principle

Codex must:

1. Read all required reference files before execution.
2. Follow disease mapping rules strictly.
3. Ask user confirmation before each major execution stage.
4. Preserve unmapped records.
5. Never modify original input files.


## Required Files

### AGENTS.md

Defines:

- Repository execution rules;
- File handling requirements;
- Output requirements.


### DISEASE_MAPPING_RULES.md

Defines:

- Disease identification rules;
- Disease entity normalization;
- Disease modifier handling;
- ICD-10 mapping rules;
- Internal disease mapping rules.


### references/ICD10_REFERENCE.md

Defines:

- ICD-10 version;
- ICD-10 source;
- ICD-10 mapping standard.


Required ICD-10 standard:

    China ICD-10 Medical Insurance Version


## Workflow

### Stage 1: Input Data Inspection

Objective:

Understand input files before performing mapping.

Codex must:

- Identify uploaded files;
- Identify sheets;
- Identify columns;
- Confirm data structure.


Required fields:

    登记号
    主试验药
    适应症
    适应症描述


Primary disease identification field:

    适应症描述


Codex must not use:

    疾病领域

as the primary mapping source.


Before continuing:

Ask user:

"Please confirm the input data structure before proceeding."


## Stage 2: Disease Entity Normalization

Objective:

Extract the core disease entity.


Disease Entity:

The underlying disease.

Example:

Input:

    原发性高血压


Output:

    Disease Entity:
    高血压


Disease Modifier:

原发性

Examples:

- Disease stage
- Severity
- Biomarker status
- Genetic subtype
- Patient population


Example:

Input:

    严重心力衰竭


Output:

    Disease Entity:
    心力衰竭

    Disease Modifier:
    严重


Modifiers should not create independent disease entities unless they correspond to different ICD-10 categories.


Before ICD-10 mapping:

Ask user:

"Please confirm disease entity normalization before proceeding."


## Stage 3: ICD-10 Mapping

Objective:

Map normalized disease entities into ICD-10.


Required standard:

    China ICD-10 Medical Insurance Version


Codex must provide:

- ICD-10 Version
- ICD-10 Code
- ICD-10 Disease Name
- ICD-10 Category
- Mapping Confidence
- Mapping Rationale


Codex must not:

- Use keyword-only matching;
- Mix ICD-10 versions;
- Over-infer disease information.


Before proceeding:

Ask user:

"Please confirm ICD-10 mapping before internal disease matching."


## Stage 4: Internal Disease Mapping

Objective:

Match ICD-10 standardized diseases with internal disease taxonomy.


Codex must determine:

- Whether mapping exists;
- Target internal disease;
- Mapping rationale.


Possible results:

    Fully Mapped

    Partially Mapped

    Unmapped

    Multiple Internal Matches

    Manual Review Required


Before generating output:

Ask user:

"Please confirm internal disease mapping results."


## Stage 5: Output Generation

Output fields:

    登记号

    主试验药

    医药魔方适应症

    原始适应症描述

    Normalized Disease Entity

    Disease Modifier

    ICD-10 Version

    ICD-10 Code

    ICD-10 Disease Name

    ICD-10 Category

    ICD-10 Confidence

    Internal Disease (CN)

    Internal Disease (EN)

    Mapping Status

    Review Required

    Mapping Reason


Before saving:

Ask user:

"Please confirm output generation."


## Data Protection Rules

Codex must:

- Never modify original files;
- Never delete unmapped records;
- Save outputs separately;
- Store generated files in:

    outputs/


## Quality Control Checklist

Before completion verify:

- Disease entity is correctly normalized;
- Disease modifiers are separated;
- ICD-10 version is consistent;
- Internal disease mapping is justified;
- Unmapped cases are preserved;
- Mapping rationale is provided.


## Final Principle

Priority:

    Mapping Accuracy

    >

    Clinical Interpretability

    >

    Mapping Coverage


Human confirmation is required before each major execution stage.
