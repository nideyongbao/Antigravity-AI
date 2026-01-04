---
description: 开发工程师技能 - Git版本控制工作流
role: developer
mcp_tools: [git, github]
---

# Git 版本控制工作流

## 概述

在实施阶段使用 Git 进行版本控制，支持：
- 初始化新仓库
- 阶段性提交代码变更
- 与 GitHub 远程仓库同步

---

## 阶段性提交策略

```yaml
commit_strategy:
  granularity: feature  # file | feature | phase
  timing:
    - after_each_feature_complete
    - before_phase_transition
  message_format: "[{phase}] {type}: {description}"
  
commit_types:
  - feat: 新功能
  - fix: 修复
  - refactor: 重构
  - docs: 文档
  - test: 测试
  - chore: 杂项
```

---

## 工作流程

### Step 1: 初始化仓库

检查是否已有 Git 仓库，如无则初始化：

```
使用 git_status 检查状态
IF 非Git仓库 THEN
  使用 run_command 执行 "git init"
  创建 .gitignore 文件
  执行首次提交
```

### Step 2: 阶段性提交

在每个功能完成后：

```yaml
commit_flow:
  1: "git_status - 查看变更文件"
  2: "git_diff - 确认变更内容"
  3: "git_add - 暂存相关文件"
  4: "git_commit - 提交并附带规范消息"
```

**提交消息规范**：
```
[IMPLEMENTATION] feat: 添加用户认证模块
[IMPLEMENTATION] fix: 修复登录验证逻辑
[TESTING] test: 添加认证模块单元测试
```

### Step 3: 推送到远程

在阶段完成时同步到 GitHub：

```yaml
push_flow:
  1: "确认所有本地提交已完成"
  2: "使用 github.create_branch 创建特性分支（如需）"
  3: "使用 github.push_files 推送代码"
  4: "使用 github.create_pull_request 创建PR（如需）"
```

---

## 提交粒度指南

| 场景 | 提交粒度 | 说明 |
|------|---------|------|
| 新增完整功能 | 1次 | 功能代码 + 测试 + 文档 |
| 修复Bug | 1次 | 修复代码 + 测试 |
| 重构 | 1次/模块 | 保持每次提交可编译运行 |
| 配置变更 | 1次 | 单独提交配置 |

---

## 与开发工作流集成

在 `role-developer.md` 的实施阶段调用：

```yaml
implementation_with_git:
  1: "接收任务"
  2: "编写代码"
  3: "本地测试通过"
  4: "调用 /skill-dev-git-workflow 提交变更"
  5: "继续下一任务"
```

---

> [!NOTE]
> 此技能需要 `git` 和 `github` MCP 服务均可用。
