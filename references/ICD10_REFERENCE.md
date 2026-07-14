# ICD-10 Reference Standard

## Purpose

This document defines the ICD-10 reference standard used in the Disease Mapping Project.

ICD-10 is used as an intermediate standardized disease classification layer between:

```text
PharmaCube indication description

↓

Normalized Disease Entity

↓

ICD-10 disease classification

↓

Internal disease taxonomy
```

The purpose of this reference file is to ensure that all disease mappings are performed using the same ICD-10 source and version.

---

# ICD-10 Version Definition

## Selected ICD-10 Standard

This project uses:

```text
China ICD-10 Medical Insurance Version
中国医疗保障疾病分类与代码（医保版 ICD-10）
```

as the official ICD-10 reference standard.

All disease mappings within this project should follow the terminology, disease names, and coding structure defined by this standard.

---

# Official Reference Source

## Primary Reference Website

The official reference source is:

```text
国家医疗保障局疾病诊断相关查询平台
National Healthcare Security Administration (NHSA) Disease Diagnosis Query Platform
```

Reference URL:

```text
https://code.nhsa.gov.cn/jbzd/public/dataWesterSearch.html
```

Organization:

```text
National Healthcare Security Administration (NHSA)
国家医疗保障局
```

---

# Reference Usage Rules

When performing ICD-10 mapping, Codex must:

1. Use the China ICD-10 Medical Insurance Version.
2. Refer to the official NHSA reference website.
3. Select ICD-10 codes based on the official disease classification.
4. Preserve the mapping rationale.

The ICD-10 reference should be used to standardize disease entities, but it should not automatically determine the final internal disease category.

The final internal disease mapping should consider:

1. Original PharmaCube indication description;
2. Normalized Disease Entity;
3. ICD-10 classification;
4. Internal disease taxonomy definition.

---

# ICD-10 Version Control

Before each disease mapping task, record:

```text
ICD-10 Version:

China ICD-10 Medical Insurance Version


Reference Source:

National Healthcare Security Administration (NHSA)

https://code.nhsa.gov.cn/jbzd/public/dataWesterSearch.html


Access Date:

YYYY-MM-DD


Mapping Date:

YYYY-MM-DD
```

---

# Version Consistency Rules

The same ICD-10 version must be used throughout one mapping project.

Codex must NOT mix different ICD-10 adaptations.

The following classifications should NOT be mixed unless explicitly requested:

```text
WHO ICD-10

ICD-10-CM

ICD-10-AM

Other regional ICD-10 adaptations
```

---

# ICD-10 Mapping Principles

## Rule 1: Use the Most Appropriate Disease Classification

The selected ICD-10 code should:

- Match the normalized disease entity;
- Follow China ICD-10 Medical Insurance terminology;
- Use the most specific available classification supported by the indication description.

Do not assign a more specific ICD-10 code when the indication description does not provide sufficient evidence.

---

## Rule 2: Avoid Over-Interpretation

Codex must not infer disease information that is not explicitly supported.

Do not automatically infer:

- Disease subtype;
- Disease severity;
- Disease stage;
- Disease etiology;
- Disease complications.

Example:

Correct:

```text
Indication:

急性心肌梗死
```

↓

Map to acute myocardial infarction related ICD-10 classification.


Incorrect:

```text
Indication:

冠心病
```

↓

Automatically mapping to acute myocardial infarction.

---

## Rule 3: Disease Groups and Clinical Syndromes

Disease groups or clinical syndromes should not automatically be expanded into specific diseases.

Example:

```text
动脉粥样硬化性心血管疾病（ASCVD）
```

should not automatically become:

```text
冠心病

缺血性卒中

外周动脉疾病
```

unless the original indication explicitly specifies these diseases.

---

## Rule 4: Multiple Possible ICD-10 Mappings

If one disease entity may correspond to multiple ICD-10 classifications:

Codex should return:

- Primary ICD-10 mapping;
- Alternative possible mappings;
- Mapping confidence;
- Review requirement.

Do not force a single mapping when uncertainty exists.

---

# ICD-10 Output Requirements

For every mapped disease entity, Codex should return:

| Field | Description |
|---|---|
| ICD-10 Version | ICD-10 reference version used |
| ICD-10 Code | Official ICD-10 code |
| ICD-10 Disease Name | Standard disease name |
| ICD-10 Category | Broader disease classification |
| Mapping Confidence | High / Medium / Low |
| Mapping Rationale | Explanation of mapping |
| Review Required | Yes / No |

---

# Example

## Input

```text
重度高甘油三酯血症
```

## Output

```text
ICD-10 Version:

China ICD-10 Medical Insurance Version


ICD-10 Code:

E78.1


ICD-10 Disease Name:

高甘油三酯血症


ICD-10 Category:

Disorders of lipoprotein metabolism and other lipidemias


Mapping Confidence:

High
```

---

# Handling Unmapped Diseases

If a disease cannot be reliably mapped using China ICD-10 Medical Insurance Version:

Codex must:

1. Preserve the original indication description.
2. Mark:

```text
ICD-10 Mapping Status:

Unmapped
```

3. Provide the reason.
4. Flag:

```text
Review Required = Yes
```

Unmapped records must never be deleted.

---

# Change Management

Any future change to ICD-10 reference standard requires recording:

- New ICD-10 version;
- New reference source;
- Effective date;
- Reason for change.

Different ICD-10 versions should not be mixed within the same mapping project.

