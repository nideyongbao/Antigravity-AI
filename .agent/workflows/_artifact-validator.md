---
description: 制品验证模块 - 自动化验证工作流输出制品
role: framework
---

# Artifact Validator / 制品验证器

> [!IMPORTANT]
> 本模块定义制品格式规范和自动验证规则，确保输出质量。

## Validation Rules / 验证规则

### Task_List.json

```yaml
validator: json_schema
required_fields:
  - project.name
  - project.description
  - tasks[]
  - tasks[].id
  - tasks[].title
  - tasks[].owner
  - tasks[].priority
  
optional_fields:
  - milestones[]
  - tasks[].dependencies

constraints:
  - "tasks.length >= 1"
  - "tasks[].priority in ['P0', 'P1', 'P2']"
  - "tasks[].owner in ['product_manager', 'architect', 'developer', 'qa_engineer']"
```

---

### PRD.md

```yaml
validator: markdown_sections
required_sections:
  - "背景与目标|Background"
  - "用户故事|User Stories"
  - "功能描述|Features"
  - "验收标准|Acceptance Criteria"
  - "非功能性需求|Non-functional"

optional_sections:
  - "排除范围|Out of Scope"
  - "术语定义|Glossary"

constraints:
  - "total_length >= 500 chars"
  - "has_acceptance_criteria_checkboxes"
```

---

### PRD_Validation_Report.md

```yaml
validator: markdown_sections
required_sections:
  - "Executive Summary"
  - "Critical Issues"
  - "Detailed Analysis"

required_elements:
  - "score_table: Pass|Fail|Needs Improvement"
  - "issues_table: ID|Category|Issue|Fix"
  - "five_dimensions: AI-Readability, Industry Standards, Logic, Edge Cases, UI"

constraints:
  - "language: Chinese"
  - "critical_issues_has_table_format"
```

---

### Implementation_Plan.md

```yaml
validator: markdown_sections
required_sections:
  - "技术概述|Overview"
  - "文件变更|File Changes"
  - "实施步骤|Implementation Steps"

optional_sections:
  - "依赖变更|Dependencies"
  - "API 接口|API"
  - "数据模型|Data Model"

constraints:
  - "has_file_change_list"
  - "steps_are_numbered"
```

---

### API_Spec.yaml

```yaml
validator: openapi_schema
required_fields:
  - openapi
  - info.title
  - info.version
  - paths

constraints:
  - "openapi_version >= 3.0.0"
  - "each_path_has_method"
```

---

### Test_Report.md

```yaml
validator: markdown_sections
required_sections:
  - "Summary"
  - "Test Results"

required_elements:
  - "summary: Total|Passed|Failed|Coverage"
  - "results_table: Test Case|Status|Duration"

conditional:
  - if: "has_failures"
    require: "Failed Tests Detail"
```

---

### Walkthrough.md

```yaml
validator: markdown_sections
required_sections:
  - "Feature Demo"
  - "Test Evidence"

required_elements:
  - "screenshots or video"
  - "verification_checklist"

constraints:
  - "has_at_least_one_image_or_video"
```

---

## Validation Execution / 执行验证

### 自动验证时机

```yaml
validate_on:
  - artifact_created: "制品创建后立即验证"
  - phase_complete: "阶段完成前验证所有制品"
  - before_gate: "审批门前验证"
```

### 验证输出

```
┌─────────────────────────────────────────────────────────────┐
│ [VALIDATE] PRD.md                                           │
│ [RESULT]   ✅ PASS                                           │
│ [CHECKS]                                                    │
│   ✓ Required sections: 5/5                                  │
│   ✓ Length: 1,234 chars (min: 500)                          │
│   ✓ Acceptance criteria: 8 items                            │
└─────────────────────────────────────────────────────────────┘
```

### 验证失败处理

```
┌─────────────────────────────────────────────────────────────┐
│ [VALIDATE] PRD.md                                           │
│ [RESULT]   ❌ FAIL                                           │
│ [ERRORS]                                                    │
│   ✗ Missing section: 验收标准                                │
│   ✗ Length: 320 chars (min: 500)                            │
│ [ACTION]   返回 DRAFT_PRD 阶段补充内容                        │
└─────────────────────────────────────────────────────────────┘
```

---

## Integration / 集成方式

在执行守卫中引用：

```yaml
# _execution-guard.md
artifact_validation:
  module: "_artifact-validator.md"
  enforce: true
  on_failure: "block_phase_transition"
```

---

> [!TIP]
> 验证规则可根据项目需求在此文件中扩展。
