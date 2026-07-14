# Disease Mapping Skill


## Overview

Disease Mapping Skill is a reusable workflow designed for Codex to perform pharmaceutical indication disease mapping tasks.

This skill provides a standardized approach to transform raw pharmaceutical indication descriptions into structured disease classifications.

The main objectives are:

- Disease entity normalization;
- ICD-10 mapping;
- Internal disease taxonomy mapping;
- Unmapped record identification and review.


The overall workflow is:

Pharmaceutical indication description

↓

Normalized Disease Entity

↓

China ICD-10 Medical Insurance Version

↓

Internal Disease Classification


---

# Purpose

Pharmaceutical databases often contain indication descriptions with:

- Different disease naming conventions;
- Different levels of clinical specificity;
- Disease subtypes;
- Clinical modifiers;
- Ambiguous disease definitions.


This skill standardizes indication information by:

- Extracting the core disease entity;
- Separating disease modifiers from disease entities;
- Mapping diseases to China ICD-10 Medical Insurance Version;
- Mapping standardized diseases to internal disease categories;
- Preserving ambiguous and unmapped records for review.


---

# Repository Structure


Disease-Mapping-Skill/

├── SKILL.md

│   └── Main execution workflow for Codex


├── AGENTS.md

│   └── Repository-level execution instructions


├── DISEASE_MAPPING_RULES.md

│   └── Detailed disease mapping methodology


├── references/

│   └── ICD10_REFERENCE.md

│       └── ICD-10 version and mapping standards


└── README.md

    └── Skill overview and usage guide



---

# File Description


## SKILL.md

Defines the main workflow for Codex execution.

This file specifies:

- Execution workflow;
- Required confirmation checkpoints;
- Input handling;
- Output generation process.


Codex should read this file when using this skill.


---

## AGENTS.md

Defines repository-level execution rules.

It specifies:

- File protection requirements;
- Data handling rules;
- Output management;
- Execution restrictions.


Key principles:

- Never modify original input files;
- Preserve unmapped records;
- Save generated outputs separately;
- Follow disease mapping rules before execution.


---

## DISEASE_MAPPING_RULES.md

Defines detailed disease mapping methodology.

This file contains:

- Pharmaceutical indication interpretation rules;
- Disease entity normalization rules;
- Disease modifier handling;
- ICD-10 mapping principles;
- Internal disease classification rules;
- Mapping status definitions;
- Manual review criteria.


Codex must follow this file when performing disease mapping.


---

## references/ICD10_REFERENCE.md

Defines the ICD-10 reference standard.

The project uses:

China ICD-10 Medical Insurance Version


This file specifies:

- ICD-10 source;
- Reference version;
- Mapping principles;
- Version control rules.


ICD-10 is used as an intermediate disease standardization layer:

Raw indication description

↓

Normalized Disease Entity

↓

ICD-10 Classification

↓

Internal Disease Classification



---

# Execution Workflow


Before performing any disease mapping task, Codex should:


Step 1:

Read the following files:


1. SKILL.md

↓

2. AGENTS.md

↓

3. DISEASE_MAPPING_RULES.md

↓

4. references/ICD10_REFERENCE.md



---

Step 2:

Inspect input data.


Identify:

- Input files;
- Relevant sheets;
- Required columns;
- Data structure.


The primary disease identification field should be:

Indication Description



---

Step 3:

Perform disease mapping.


The mapping workflow includes:


1. Disease Entity Extraction

Identify the core disease entity from the indication description.


2. Disease Modifier Separation

Separate disease entities from modifiers.

Examples of modifiers:

- Disease stage;
- Disease severity;
- Biomarker status;
- Genetic subtype;
- Patient population.


3. ICD-10 Mapping

Map normalized disease entities to:

China ICD-10 Medical Insurance Version



4. Internal Disease Mapping

Map ICD-10-standardized diseases into internal disease taxonomy.


Ambiguous cases should be preserved for manual review.



---

# Output Requirements


The output should include:


- Original indication description;
- Normalized Disease Entity;
- Disease Modifier;
- ICD-10 Version;
- ICD-10 Code;
- ICD-10 Disease Name;
- ICD-10 Category;
- ICD-10 Confidence;
- Internal Disease Classification;
- Mapping Status;
- Review Requirement;
- Mapping Rationale.



---

# Mapping Principles


The skill must always follow:


- Preserve original input data;
- Do not modify original files;
- Do not force uncertain mappings;
- Separate disease entities from disease modifiers;
- Use China ICD-10 Medical Insurance Version;
- Maintain mapping rationale;
- Preserve unmapped records.



---

# Intended Use Cases


This skill can support:


- Pharmaceutical pipeline analysis;
- Clinical trial indication mapping;
- Drug portfolio analysis;
- Therapeutic area landscaping;
- Market intelligence projects;
- Disease taxonomy standardization.



---

# Current Scope


Current focus:

- Cardiovascular disease indication mapping;
- ICD-10 standardization;
- Internal disease taxonomy alignment.


Future extensions:

- Additional therapeutic areas;
- Automated synonym dictionaries;
- Expanded disease ontology mapping;
- Validation workflows.



---

# Version Control


Updates to:

- Disease mapping rules;
- ICD-10 references;
- Execution workflows;


should be documented through repository commits.
