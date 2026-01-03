---
description: 开发工程师技能 - 响应QA返工请求修复缺陷
role: developer
skill: bug_fix
---

# Skill: Bug Fix / 缺陷修复

响应 QA 返工请求，定位并修复缺陷。

## Trigger / 触发条件

```yaml
trigger:
  source: "qa_engineer"
  artifact: "Defect_Report.md"
  signal: "REWORK_REQUIRED"
```

## Steps

### Step 1: Observe - 解析缺陷报告

```yaml
inputs:
  - defect_report: "QA 提交的缺陷报告"
  
extract:
  - defect_id: "缺陷编号"
  - description: "问题描述"
  - reproduction_steps: "复现步骤"
  - expected_vs_actual: "预期与实际"
  - evidence: "截图/日志"
```

### Step 2: Orient - 定位问题

```yaml
analysis:
  - trace_code_path: "根据复现步骤追踪代码"
  - identify_root_cause: "确定根本原因"
  - assess_impact: "评估影响范围"
  
root_cause_categories:
  - logic_error: "逻辑错误"
  - edge_case_missing: "边缘情况未处理"
  - integration_issue: "集成问题"
  - timing_issue: "时序问题"
```

### Step 3: Decide - 制定修复方案

```yaml
fix_plan:
  - description: "修复描述"
  - files_affected: ["需要修改的文件"]
  - regression_test: "需要添加的回归测试"
```

### Step 4: Act - 实施修复

```yaml
actions:
  1: "修改代码修复缺陷"
  2: "添加回归测试用例"
  3: "运行本地测试验证"
  4: "生成修复后的 Code Diffs"
```

### Step 5: Validate - 验证修复

```yaml
validation:
  - run_regression_test: "运行回归测试"
  - run_related_tests: "运行相关测试"
  - check_no_side_effects: "确保无副作用"
  
on_success:
  - signal: "REWORK_COMPLETE"
  - notify: "qa_engineer"
  
on_failure:
  - retry_fix
```

## Output

### Fix_Report.md

```markdown
# 缺陷修复报告

## 缺陷信息
- **ID**: DEFECT-001
- **标题**: 表单提交超时

## 根因分析
onClick 事件处理函数中缺少异步等待

## 修复内容
### 修改文件
- `src/components/Form.tsx` (L45-52)

### 代码变更
```diff
- const handleSubmit = () => {
+ const handleSubmit = async () => {
-   submitForm(data);
+   await submitForm(data);
  };
```

## 回归测试
添加测试用例：`Form.test.tsx` - "should handle async submit"

## 验证结果
- [x] 本地测试全部通过
- [x] 无副作用
```

---

> [!TIP]
> 修复完成后自动触发 QA 重测流程。
