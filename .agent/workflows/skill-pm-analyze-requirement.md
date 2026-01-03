---
description: 产品经理技能 - 意图解析、模糊度检测、竞品调研
role: product_manager
skill: analyze_requirement
mcp_tools: [rednote-MCP, fabric-mcp-server]
---

# Skill: Requirement Analysis / 需求分析

深度分析用户需求，识别核心意图，消除模糊性。

## Steps

### Step 1: Observe - 收集原始需求

```yaml
inputs:
  - task_description: "任务清单中的描述"
  - user_prompt: "用户原始输入"
  - conversation_history: "对话上下文"
  
actions:
  - 提取显式需求
  - 识别隐式需求
  - 标记假设和约束
```

### Step 2: Orient - 意图解析与模糊度检测

**意图分类**：

| 类型 | 示例 | 处理方式 |
|------|------|----------|
| 功能需求 | "添加登录功能" | 细化功能点 |
| 性能需求 | "要快" | 量化指标 |
| 体验需求 | "要好用" | 转化为可测量标准 |
| 约束条件 | "使用现有技术" | 记录限制 |

**模糊度评分**：

```yaml
ambiguity_check:
  - item: "用户类型是否明确？"
    score: 0-10
  - item: "功能边界是否清晰？"
    score: 0-10
  - item: "成功标准是否可量化？"
    score: 0-10
  
threshold: 
  clear: score >= 8
  needs_clarification: score < 8
```

### Step 3: Decide - 竞品调研（可选）

使用 `rednote-MCP` 搜索相关产品信息：

```yaml
research_actions:
  - search_keywords: ["功能名称 + 最佳实践"]
  - analyze_competitors: "提取共同模式"
  - identify_differentiators: "找出差异化机会"
```

### Step 4: Act - 生成需求分析文档

```markdown
# 需求分析报告

## 1. 核心意图
用户希望 [一句话总结]

## 2. 功能分解
- 功能1：描述
- 功能2：描述

## 3. 模糊点澄清
| 问题 | 建议默认值 | 需确认？ |
|------|-----------|---------|
| 用户角色 | 普通用户 | 是 |

## 4. 竞品参考
- 产品A：做法描述
- 产品B：做法描述

## 5. 建议
基于分析，建议采用 [方案描述]
```

### Step 5: Validate - 确认分析完整

- [ ] 所有模糊点都有处理方案？
- [ ] 核心功能无遗漏？
- [ ] 假设已明确记录？

## Output

生成文件：`Requirement_Analysis.md`

## Clarification Flow / 澄清流程

当模糊度 > 30% 时触发：

```yaml
clarification:
  questions:
    - "请问 [模糊点1] 具体是指...？"
    - "[模糊点2] 是否需要支持...？"
  
  format: "向用户提出简洁的澄清问题，不超过 3 个"
```

---

> [!TIP]
> 使用 `fabric-mcp-server.recommend_tool` 选择最合适的分析模式。
