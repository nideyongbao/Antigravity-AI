# OpenSpec AI 操作指南

## 快速检查清单

在开始任何涉及变更的任务前：
- [ ] 检查 `openspec/changes/` 是否有相关提案
- [ ] 检查 `openspec/specs/` 是否有相关规格
- [ ] 确定是否需要创建新提案

## 何时打开此文件

当请求涉及以下关键词时：
- proposal, spec, change, plan
- 新功能, 重构, 架构变更
- Breaking change, API 变更

## 创建提案步骤

1. **选择 change-id**（动词开头，kebab-case）
2. **创建目录**：`openspec/changes/{change-id}/`
3. **编写 proposal.md**：
   ```markdown
   # Change: {简短描述}
   
   ## Why
   {问题/机会}
   
   ## What Changes
   - {变更点1}
   - {变更点2}
   
   ## Impact
   - 影响的文件/模块
   ```
4. **编写 tasks.md**：
   ```markdown
   ## Implementation
   - [ ] Task 1
   - [ ] Task 2
   ```

## 归档已完成变更

完成后移动到：`changes/archive/YYYY-MM-DD-{change-id}/`
