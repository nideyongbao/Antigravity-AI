---
description: 测试工程师技能 - 测试失败时生成缺陷报告并触发返工
role: qa_engineer
skill: defect_report
---

# Skill: Defect Report / 缺陷报告

测试失败时生成标准化缺陷报告，触发开发返工流程。

## Trigger / 触发条件

```yaml
trigger:
  condition: "test_status == FAIL"
  source: "e2e_test | integration_test"
```

## Steps

### Step 1: Observe - 收集失败信息

```yaml
inputs:
  - failed_test: "失败的测试用例"
  - error_message: "错误信息"
  - screenshots: "相关截图"
  - logs: "控制台日志"
```

### Step 2: Orient - 评估严重程度

| 严重程度 | 定义 | 示例 |
|----------|------|------|
| Critical | 阻塞主流程 | 无法登录 |
| High | 主要功能异常 | 支付失败 |
| Medium | 次要功能问题 | 筛选不工作 |
| Low | 体验问题 | 样式错位 |

### Step 3: Decide - 确定缺陷信息

```yaml
defect:
  id: "DEFECT-{timestamp}"
  title: "简明的问题描述"
  severity: "High"
  test_case: "TC-002"
  component: "Login"
  assignee: "developer"
```

### Step 4: Act - 生成缺陷报告

```markdown
# Defect Report

## Basic Info
| 属性 | 值 |
|------|-----|
| ID | DEFECT-20260102-001 |
| 标题 | 表单提交后无响应 |
| 严重程度 | High |
| 关联用例 | TC-005 |
| 发现日期 | 2026-01-02 |

## Description
用户点击提交按钮后，页面无任何响应，请求未发出。

## Reproduction Steps
1. 打开表单页面 `/form`
2. 填写所有必填字段
3. 点击"提交"按钮
4. 观察页面行为

## Expected vs Actual
- **Expected**: 3秒内完成提交，显示成功提示
- **Actual**: 按钮点击后无反应，10秒后仍无变化

## Evidence
### Screenshot
![Error State](./screenshots/defect-001.png)

### Console Log
```
Uncaught TypeError: Cannot read property 'submit' of undefined
    at Form.tsx:45
```

## Environment
- Browser: Chrome 120
- Resolution: 1920x1080
- OS: Windows 11

## Suggested Investigation
检查 `Form.tsx` 第 45 行的事件绑定
```

### Step 5: Validate & Trigger - 触发返工

```yaml
actions:
  - save: "Defect_Report.md"
  - notify: "developer"
  - signal: "REWORK_REQUIRED"
  - update_status: "WAIT_FOR_FIX"
```

## Rework Flow Integration

```
QA 发现缺陷
    ↓
生成 Defect_Report.md
    ↓
发送 REWORK_REQUIRED 信号
    ↓
Developer 接收并处理
    ↓
Developer 发送 REWORK_COMPLETE 信号
    ↓
QA 重新执行失败用例
    ↓
通过 → 继续验收
失败 → 再次生成缺陷报告
```

## Output

- 缺陷报告：`Defect_Report.md`
- 信号：`REWORK_REQUIRED`

> [!WARNING]
> 若同一缺陷返工 3 次仍未通过，升级通知项目主管。
