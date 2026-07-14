# Disease Mapping Rules

## Project Objective

The purpose of this project is to map pharmaceutical indications exported from PharmaCube to:

1. ICD-10 disease classifications.
2. Internal disease taxonomy.

For every record in the PharmaCube file, Codex must:

- Read the indication description.
- Identify the disease entity.
- Map the disease to ICD-10.
- Match the ICD-10 result to the internal disease classification.
- Return unmapped diseases.
- Explain the mapping rationale.

---

# Input Files

## File 1: PharmaCube Export

Main sheet:

```text
创新药
```

Important columns:

| Column | Meaning |
|----------|----------|
| 登记号 | Unique record ID |
| 主试验药 | Drug name |
| 适应症 | PharmaCube summarized indication |
| 适应症描述 | Detailed indication description |
| 试验题目 | Trial title |

Important:

The primary field for disease identification is:

```text
适应症描述
```

The following columns are supplementary only:

```text
适应症
试验题目
```

Codex must NOT use:

```text
疾病领域
```

as the primary source for disease mapping.

---

## File 2: Internal Disease Classification

Main sheet:

```text
Disease Mapping
```

Important columns:

| Column | Meaning |
|----------|----------|
| B | TA |
| C | Disease (EN) |
| D | Disease (CN) |

Examples:

- 心力衰竭
- 心律失常/房颤
- 冠心病
- 高血压
- 高甘油三酯血症
- 高胆固醇血症
- 急性冠脉综合征

Score columns must NOT be used for disease matching.

---

## File 3: ICD-10 Reference

ICD-10 is used as the standardized disease classification layer between PharmaCube indication descriptions and internal disease taxonomy.

The project primarily uses:

```text
China ICD-10 Medical Insurance Version (医保版 ICD-10)

---

# Overall Workflow

For every row in the PharmaCube file:

```text
登记号
      ↓
主试验药
      ↓
适应症描述
      ↓
Identify indication structure
      ↓
Normalize Disease Entity
      ↓
Identify Disease Modifier
      ↓
Map Disease Entity to ICD-10
      ↓
Match Internal Disease(CN)
      ↓
Generate output
```
---

# Disease Identification Rules

## Rule 1: Always prioritize indication description

Priority:

1. 适应症描述
2. 适应症
3. 试验题目
4. 历史备注

Never prioritize:

- 疾病领域

---

## Rule 2: Indication Structure Identification

In the current dataset, each PharmaCube indication description represents one clinical indication.

Therefore:

- Codex should NOT split indication descriptions by default.
- Each indication description should initially be treated as one mapping unit.
- The original indication description must always be preserved.

  
Each indication description should first be classified into one of the following structures：
1. Single disease entity;
2. Disease entity with modifiers;
3. Multiple independent diseases.

The original indication description must always be preserved.

The workflow is:

```text
Original indication description

↓


Identify indication structure

↓

Normalize Disease Entity

↓

Identify Disease Modifier

↓

ICD-10 mapping

↓

Internal disease mapping
```

---

## Rule 3: Disease Entity Normalization

Disease Entity represents the core disease that should be mapped to ICD-10 classification and internal disease taxonomy.

Indication descriptions may contain:

- Disease subtype;
- Disease phenotype;
- Clinical classification;
- Severity;
- Biomarker information
- Genetic subtype;
- Disease stage.

These characteristics should not automatically create separate disease entities.

Codex should:

1. Identify the core disease entity.
2. Preserve clinically relevant modifiers separately.
3. Map the disease entity to ICD-10.
4. Use the internal disease taxonomy for final classification.

## Examples

### Example 1

Input:

```text
症状性心力衰竭伴射血分数降低
```

Correct:

Disease Entity:

```text
心力衰竭
```

Disease Modifier:

```text
症状性
射血分数降低
```

Mapping:

One ICD-10 mapping.

---

### Example 2
Input:

```text
杂合子型家族性高胆固醇血症
```

Correct:

Disease Entity:

```text
高胆固醇血症
```

Disease Modifier:

```text
杂合子型家族性
```

---

Disease modifiers should not create separate disease entities unless they correspond to an independent ICD-10 disease classification.

---

## Rule 4: Disease Modifier Indication

Indication descriptions may contain disease modifiers.

Common modifiers include:

- Disease stage;
- Severity;
- Biomarker status;
- Genetic subtype;
- Patient population;
- Treatment history;
- Acute/chronic status.


Modifiers should be preserved but should not be treated as independent diseases.


Examples:
Input:

```text
重度高甘油三酯血症
```

Disease Entity:

```text
高甘油三酯血症
```

Modifier:

```text
重度
```
---

Input:

```text
杂合子型家族性高胆固醇血症
```

Disease Entity:

```text
高胆固醇血症
```

Modifier:

```text
杂合子型家族性
```

---

## Rule 5: Multiple Independent Disease Handling

Codex should only split an indication description when it contains clearly independent diseases.

Independent diseases mean:

- They represent different disease entities;
- They have separate ICD-10 mappings;
- They cannot be represented by one common disease entity.

Example:

Input:

```text
动脉粥样硬化性心血管疾病；肥胖
```

Result:

Disease Entity 1:

```text
动脉粥样硬化性心血管疾病
```

Disease Entity 2:

```text
肥胖
```

Mapping Status:

```text
Partially Mapped
```

---


# ICD-10 Mapping Rules

The ICD-10 mapping should follow:

```text
PharmaCube indication description
          ↓
Normalized Disease Entity
          ↓
China ICD-10 Medical Insurance classification
          ↓
Internal disease classification


For every indication:

Codex must return:

| Field |
|--------|
| ICD-10 Version |
| ICD-10 Code |
| ICD-10 Disease Name |
| ICD-10 Category |
| ICD-10 Confidence |

ICD-10 Version should always be:

China ICD-10 Medical Insurance Version

Codex must:

- Choose the most specific ICD-10 disease supported by the indication description.
- Avoid over-interpretation.
- Keep uncertainty explicit.

---

## Example

Input:

```text
重度高甘油三酯血症
```

Output:

ICD-10 Version:

```text
China ICD-10 Medical Insurance Version
```

ICD-10 Code:

```text
E78.1
```

ICD-10 Disease Name:

```text
高甘油三酯血症
```

---

# Internal Disease Mapping Rules

Internal matching target:

```text
Disease Mapping
        ↓
Disease (CN)
```

Codex must determine:

1. Can the indication be mapped?
2. Which disease does it map to?
3. Why?

---

## Rule 1: Exact matching preferred

Prefer:

- Exact disease names.
- Accepted medical synonyms.
- ICD-10-supported mappings.

---

## Rule 2: Do not force mapping

If no disease exists:

Return:

```text
Can Match = No

Internal Disease = Unmapped
```

Never force diseases into existing categories.

---

## Rule 3: Multiple disease mappings

Some disease entities may correspond to multiple possible internal diseases.

Example:

```text
动脉粥样硬化性心血管疾病
```

Possible diseases:

- 冠心病
- 外周动脉疾病
- 缺血性卒中

These are possible clinical associations, not automatic mappings.

Codex should only map when supported by:

- Original indication description;
- ICD-10 classification;
- Internal disease taxonomy.

---

## Rule 4: Cross-TA indications

Some indications do not belong to cardiovascular diseases.

Example:

```text
肥胖
超重
2型糖尿病
```

Codex must:

- Mark:

```text
Non-cardiovascular indication
```

- Exclude them from cardiovascular mapping.

---

## Rule 5: Partial mapping

One drug may contain both cardiovascular and non-cardiovascular indications.

Example:

```text
动脉粥样硬化性心血管疾病；肥胖；超重
```

Result:

| Indication | Match |
|------------|--------|
| 动脉粥样硬化性心血管疾病 | Yes |
| 肥胖 | No |
| 超重 | No |

Return:

```text
Mapping Status:

Partially Mapped
```

---

# Mapping Status

Use one of:

```text
Fully Mapped
Partially Mapped
Unmapped
Multiple Internal Matches
Manual Review Required
```

Definitions:

### Fully Mapped

All indications match internal diseases.

### Partially Mapped

Some indications match and some do not.

### Unmapped

No internal disease exists.

### Multiple Internal Matches

One indication corresponds to multiple diseases.

### Manual Review Required

Codex cannot confidently determine the mapping.

---

# Manual Review Rules

Mark:

```text
Review Required = Yes
```

if:

- Multiple ICD-10 diseases are possible.
- Multiple internal diseases are possible.
- The indication is ambiguous.
- It is unclear whether the disease belongs to cardiovascular TA.

Codex must never force uncertain mappings.

---

# Required Output

For every normalized disease entity:

| Field |
|--------|
| 登记号 |
| 主试验药 |
| 医药魔方适应症 |
| 原始适应症描述 |
| Normalized Disease Entity |
| Disease Modifier |
| ICD-10 Version |
| ICD-10 Code |
| ICD-10 Disease Name |
| ICD-10 Category |
| ICD-10 Confidence |
| Can Match Internal Disease |
| Internal Disease (CN) |
| Internal Disease (EN) |
| Mapping Status |
| Review Required |
| Mapping Reason |

---

# Output Examples

## Example 1

Input:

```text
登记号：

DR10624

主试验药：

DR10624

适应症：

高甘油三酯血症

适应症描述：

重度高甘油三酯血症
```

Output:

| Field | Value |
|--------|--------|
| Normalized Disease Entity | 高甘油三酯血症 |
| Disease Modifier | 重度 |
| ICD-10 Code | E78.1 |
| ICD-10 Disease Name | Hypertriglyceridemia |
| Can Match | Yes |
| Internal Disease | 高甘油三酯血症 |
| Mapping Status | Fully Mapped |

Reason:

```text
The indication describes severe hypertriglyceridemia.
"Severe" is a disease modifier and does not create a separate disease entity.
The core disease entity is hypertriglyceridemia.
```

---

## Example 2

Input:

```text
适应症描述：

急性穿支动脉梗死
```

Output:

| Field | Value |
|--------|--------|
| Normalized Disease Entity | 脑梗死/缺血性卒中 |
| Disease Modifier | 急性穿支动脉梗死 |
| ICD-10 Code | I63 |
| ICD-10 Disease Name | Ischemic stroke |
| Can Match | No |
| Internal Disease | Unmapped |
| Status | Unmapped |

Reason:

```text
The indication can be mapped to ischemic stroke in ICD-10, but there is no corresponding disease in the internal disease table.
```

---

## Example 3

Input:

动脉粥样硬化性心血管疾病；肥胖；超重


Because these represent independent disease entities:

Split is required.


Result:

| Disease Entity | Match |
|---|---|
| 动脉粥样硬化性心血管疾病 | Yes |
| 肥胖 | No |

Additional information:

超重 is retained as a clinical condition but is not mapped as an independent disease entity.

Status:

Partially Mapped
```

This is an exception case where multiple independent clinical conditions are explicitly present.

Semicolon alone should NOT trigger splitting.

---

# Restrictions

Codex must NOT:

- Use score columns.
- Ignore indication descriptions.
- Use disease area as the primary source.
- Force mappings.
- Delete unmapped diseases.
- Modify original files.
- Use ICD-10 versions other than China ICD-10 Medical Insurance Version unless explicitly requested.
- Mix different ICD-10 versions within one mapping task.
- Select ICD-10 codes only based on keyword similarity.
All unmapped records must be preserved.
