---
description: 产品经理技能 - 五维度深度验证PRD文档
role: product_manager
skill: validate_prd
---

# Skill: PRD Validation / PRD 验证

基于五个核心维度深度验证 PRD 文档质量。

## Context

你是一位专业的 **产品经理** 和 **AI 解决方案架构师**。目标是审查 PRD 文档并生成综合验证报告。

## Steps

### Step 1: Observe - 读取 PRD 文档

```yaml
inputs:
  - prd_file: "待验证的 PRD 文档"
  
actions:
  - 解析文档结构
  - 提取核心需求点
  - 建立验证检查清单
```

### Step 2: Orient - 加载验证标准

**五维度验证框架**：

| 维度 | 关注点 | 权重 |
|------|--------|------|
| AI 可读性 | 结构清晰、术语明确、类型定义 | 20% |
| 行业规范 | 隐私安全、UX 标准、合规性 | 20% |
| 逻辑闭环 | 流程完整、状态转换、数据流 | 25% |
| 边缘情况 | 异常处理、空状态、并发访问 | 20% |
| UI 一致性 | 视觉描述与功能匹配 | 15% |

### Step 3: Decide - 逐维度分析

#### 3.1 AI 可读性检查

```yaml
check_items:
  - "需求描述是否具体可执行？"
  - "是否定义了数据类型和格式？"
  - "API 接口是否明确？"
  
bad_example: "用户信息应该被更新"
critique: "模糊，无法确定哪些字段、什么接口"
suggestion: "定义 updateUserProfile schema，指定 PATCH /api/user/profile"
```

#### 3.2 行业规范检查

```yaml
check_items:
  - "是否考虑用户隐私保护？"
  - "是否遵循安全最佳实践？"
  - "UX 模式是否符合常规？"
  
bad_example: "用户点击删除，数据立即清除"
critique: "缺少安全网，违反数据保留标准"
suggestion: "实现软删除，30天恢复期，二次确认"
```

#### 3.3 逻辑闭环检查

```yaml
check_items:
  - "每个流程是否有起点和终点？"
  - "状态转换是否完整？"
  - "异步操作是否有回调处理？"
  
bad_example: "用户点击支付 → 调用 API → 显示成功"
critique: "开环，缺少 webhook 处理和失败状态"
suggestion: "添加：处理 payment_intent.succeeded，更新订单，发送邮件，失败重试"
```

#### 3.4 边缘情况检查

```yaml
check_items:
  - "网络错误如何处理？"
  - "空数据如何展示？"
  - "并发访问是否安全？"
  - "输入限制是否明确？"
  
bad_example: "用户上传头像图片"
critique: "缺少约束和错误处理"
suggestion: "定义：大小限制(5MB)，格式限制，超时处理，恶意文件检测"
```

#### 3.5 UI 一致性检查

```yaml
check_items:
  - "UI 描述是否支持所有功能？"
  - "交互元素是否完整？"
  - "响应式设计是否考虑？"
  
bad_example: |
  逻辑：搜索支持日期和分类筛选
  UI：一个简单的搜索输入框
critique: "UI 与功能矛盾，用户无法输入筛选条件"
suggestion: "添加筛选按钮，打开模态框包含日期选择器和分类下拉"
```

### Step 4: Act - 生成验证报告

```markdown
# PRD Validation Report for [文件名]

## 1. Executive Summary
评分：[Pass/Fail/Needs Improvement]
总分：[X]/100

## 2. Critical Issues (Blockers)
| ID | Category | Issue Description | Suggested Fix |
|----|----------|-------------------|---------------|
| 1  | Logic    | 支付流程缺少失败处理 | 添加失败状态和重试逻辑 |

## 3. Detailed Analysis

### 3.1 AI-Readability (Score: X/20)
分析内容...

### 3.2 Industry Standards (Score: X/20)
分析内容...

### 3.3 Logical Completeness (Score: X/25)
分析内容...

### 3.4 Edge Cases (Score: X/20)
分析内容...

### 3.5 UI Consistency (Score: X/15)
分析内容...

## 4. Optimized Spec Proposal
(针对最严重问题给出重写示例)
```

### Step 5: Validate - 自检报告质量

- [ ] 报告包含所有五个维度？
- [ ] Critical Issues 使用表格格式？
- [ ] 每个问题都有修复建议？
- [ ] 报告使用中文？

## Output

生成文件：`PRD_Validation_Report.md`

> [!IMPORTANT]
> **不要**直接修改原始 PRD 文件，仅生成验证报告。
