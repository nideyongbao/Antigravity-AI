---
description: 架构师技能 - 技术选型与风险评估
role: architect
skill: feasibility_assess
mcp_tools: [github, fabric-mcp-server]
---

# Skill: Feasibility Assessment / 可行性评估

评估技术方案的可行性，识别风险和约束。

## Steps

### Step 1: Observe - 收集技术信息

```yaml
inputs:
  - prd: "PRD 文档"
  - codebase: "现有代码库结构"
  
actions:
  - github.get_file_contents: "读取 package.json 等配置"
  - github.search_code: "搜索相关现有实现"
  - analyze_tech_stack: "识别当前技术栈"
```

### Step 2: Orient - 评估可行性矩阵

| 维度 | 评估问题 | 权重 |
|------|----------|------|
| 技术匹配 | 现有技术能否满足需求？ | 30% |
| 依赖风险 | 新依赖是否稳定？版本冲突？ | 25% |
| 架构影响 | 需要多大程度重构？ | 25% |
| 团队能力 | 技术栈是否熟悉？ | 20% |

### Step 3: Decide - 生成评估报告

```markdown
# 技术可行性评估报告

## 1. 评估结论
**可行性**：✅ 高 / ⚠️ 中 / ❌ 低

## 2. 技术选型
| 需求 | 推荐方案 | 备选方案 | 理由 |
|------|----------|----------|------|

## 3. 依赖分析
| 库名称 | 当前版本 | 需要版本 | 兼容性 |
|--------|----------|----------|--------|

## 4. 风险清单
| 风险 | 等级 | 缓解措施 |
|------|------|----------|

## 5. 建议
[综合建议]
```

### Step 4: Act - 输出评估结果

生成 `Feasibility_Assessment.md`

### Step 5: Validate - 校验评估完整性

- [ ] 所有技术点都已评估？
- [ ] 风险都有缓解方案？
- [ ] 依赖冲突已识别？

## Output

生成文件：`Feasibility_Assessment.md`
