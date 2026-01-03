---
description: 架构师技能 - 生成详细技术实施方案
role: architect
skill: implementation_plan
mcp_tools: [github]
---

# Skill: Implementation Plan / 实施计划

生成详细的技术实施方案，指导开发执行。

## Steps

### Step 1: Observe - 综合输入

```yaml
inputs:
  - prd: "PRD 文档"
  - feasibility: "可行性评估"
  - api_spec: "API 设计"
  - codebase: "现有代码结构"
```

### Step 2: Orient - 规划变更范围

```yaml
change_scope:
  new_files: ["需要创建的文件列表"]
  modified_files: ["需要修改的文件列表"]
  dependencies: ["需要添加的依赖"]
```

### Step 3: Decide - 编写实施计划

```markdown
# Implementation Plan: [功能名称]

## 1. 概述
[一段话描述技术方案]

## 2. 依赖变更

### 添加依赖
```bash
npm install package-name@version
```

### 版本更新
| 包名 | 当前 | 目标 |
|------|------|------|

## 3. 文件变更

### 3.1 新增文件

#### [NEW] src/components/NewComponent.tsx
- 职责：[描述]
- 接口：[props 定义]

### 3.2 修改文件

#### [MODIFY] src/pages/Home.tsx
- 变更：[描述变更内容]
- 原因：[为什么需要修改]

## 4. 实施步骤

### Step 1: [步骤名称]
- 操作：[具体操作]
- 验证：[如何验证完成]

### Step 2: [步骤名称]
...

## 5. 测试要点
- [需要覆盖的测试场景]

## 6. 回滚方案
- [如何回滚变更]
```

### Step 4: Act - 输出实施计划

生成 `Implementation_Plan.md`

### Step 5: Validate - 校验计划完整性

- [ ] 所有功能点都有实现方案？
- [ ] 步骤顺序正确（依赖在前）？
- [ ] 测试要点覆盖验收标准？

## Output

生成文件：`Implementation_Plan.md`
