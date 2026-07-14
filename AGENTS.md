# AGENTS.md

# Repository Execution Instructions

This file defines the repository-level execution rules for Codex when performing disease mapping tasks.

The purpose of this file is to ensure consistent execution of pharmaceutical indication disease mapping workflows.


---

# Execution Order

Before performing any disease mapping task, Codex must read and understand the following files in order:


1. AGENTS.md

↓

2. references/ICD10_REFERENCE.md

↓

3. DISEASE_MAPPING_RULES.md

↓

4. Input data files


The mapping task must not start before all required instructions and reference standards have been reviewed.


---

# General Rules


## 1. Follow Disease Mapping Rules

Always follow:

```text
DISEASE_MAPPING_RULES.md
```

for all disease mapping logic.

Do not create alternative mapping rules unless explicitly requested by the user.

The disease mapping methodology, normalization logic, and output requirements defined in this file must be strictly followed.


---

## 2. Use the Correct ICD-10 Standard

Always use the ICD-10 reference standard defined in:

```text
references/ICD10_REFERENCE.md
```

The project uses:

```text
China ICD-10 Medical Insurance Version
中国医疗保障疾病分类与代码（医保版 ICD-10）
```

as the official ICD-10 reference standard.


Codex must NOT mix different ICD-10 versions, including:

- WHO ICD-10;
- ICD-10-CM;
- ICD-10-AM;
- Other national ICD-10 adaptations.


If ICD-10 information is uncertain, Codex must mark:

```text
Review Required = Yes
```

rather than selecting an unsupported code.


---

## 3. Preserve Original Data

Codex must:

- Never modify original input files;
- Never overwrite source datasets;
- Preserve original indication descriptions;
- Preserve all unmapped records.


Original pharmaceutical database files and internal disease classification files must remain unchanged.


---

## 4. Disease Mapping Principles

During mapping, Codex must:


- Identify the normalized disease entity before ICD-10 mapping;
- Separate disease entities from disease modifiers;
- Consider clinical meaning rather than keyword similarity only;
- Avoid forced mappings;
- Provide mapping rationale for each output;
- Maintain uncertainty when appropriate.


Disease modifiers include, but are not limited to:

- Disease stage;
- Severity;
- Biomarker status;
- Genetic subtype;
- Patient population;
- Clinical phenotype.


Modifiers should not automatically create separate disease entities.


---

## 5. Handling Indication Descriptions

Codex must preserve the original indication description.

The default assumption is:

```text
One indication description
        ↓
One normalized disease entity
        ↓
One ICD-10 mapping
        ↓
Internal disease mapping
```


Codex should only split indications when multiple independent diseases are explicitly present.


Examples requiring split:

```text
动脉粥样硬化性心血管疾病；肥胖
```


Examples not requiring split:

```text
晚期非小细胞肺癌
```

The result should be:

```text
Disease Entity:
非小细胞肺癌

Modifier:
晚期
```


---

## 6. Avoid Forced Mapping

Codex must not force a disease into an unrelated ICD-10 category or internal disease category.


If no reliable mapping exists:

Return:

```text
Internal Disease = Unmapped
Review Required = Yes/No
```

with a clear explanation.


---

## 7. Handling Unmapped Records

All unmapped records must be preserved.


Examples include:

- No corresponding ICD-10 classification;
- No corresponding internal disease category;
- Ambiguous indication descriptions;
- Multiple possible mappings;
- Cases requiring manual review.


Unmapped records must never be deleted.


---

## 8. Output Requirements

All generated outputs must be saved under:

```text
outputs/
```


The output folder should contain:


### Mapping Results

Including:

- Original indication description;
- Normalized disease entity;
- Disease modifier;
- ICD-10 version;
- ICD-10 code;
- ICD-10 disease name;
- Internal disease classification;
- Mapping confidence;
- Mapping rationale.


### Review Files

Including:

- Unmapped records;
- Ambiguous records;
- Records requiring manual validation.


Generated files must be separated from original input files.


---

## 9. File Modification Restrictions

Codex must NOT modify the following files unless explicitly instructed by the user:


```text
AGENTS.md

DISEASE_MAPPING_RULES.md

references/ICD10_REFERENCE.md
```


These files define project-level rules and reference standards.


---

## 10. Quality Control Before Completion

Before completing any mapping task, Codex should verify:


### ICD-10 Validation

- Correct ICD-10 version is used;
- ICD-10 source is consistent;
- No mixed ICD-10 standards are used.


### Disease Mapping Validation

- Original indication description is preserved;
- Disease entity normalization is clinically reasonable;
- Disease modifiers are separated correctly;
- No unsupported assumptions are introduced.


### Output Validation

- Mapping results are complete;
- Unmapped records are retained;
- Manual review cases are clearly identified;
- Output files are stored separately from input files.


---

# Final Execution Principle

The objective of this repository is not only to maximize mapping rate.

The priority order is:

```text
Mapping Accuracy
        >
Clinical Interpretability
        >
Mapping Coverage
```


Codex should prefer transparent and reviewable mappings over forced classifications.
