# 🧠 Antigravity-AI 项目上下文

> 本文档为 AI 代理提供项目一站式上下文，避免重复探索。

## 1. 项目概述

| 属性 | 描述 |
|------|------|
| **类型** | AI 驱动的多角色开发工作流框架 |
| **目标** | 从模糊需求到高质量软件交付的全链路自动化 |
| **特点** | Prompt 驱动，无需 Python Runtime |

## 2. 核心架构

```
用户需求 → 主编排器 → 角色调度 → 技能执行 → 制品输出
                ↓
        5 个专业角色 + 23 个技能
```

### 角色列表
| 角色 | 职责 | MCP 工具 |
|------|------|---------|
| 项目主管 | 任务拆解、进度仲裁 | github, memory |
| 产品经理 | 需求分析、PRD 生成 | tavily |
| 系统架构师 | 技术选型、API 设计 | github, tavily |
| 开发工程师 | TDD 编码、自我修复 | github |
| 测试工程师 | E2E 测试、构建验证 | puppeteer, memory |

## 3. 目录结构速查

| 目录 | 用途 | 说明 |
|------|------|------|
| `.agent/workflows/` | 工作流定义 | 角色、技能、守卫模块 |
| `.antigravity/` | AI 行为规则 | 编码规范、安全限制 |
| `.antigravity-output/` | 任务输出 | 按 task_id 组织 |
| `.context/` | 动态知识 | 自动注入到提示 |
| `mcp_config/` | MCP 服务配置 | 工具授权 |
| `docs/` | 项目文档 | 架构、流程说明 |
| `openspec/` | 变更规范 | 提案、规格 |

## 4. 快速入口

- **启动完整工作流**：`/antigravity-team`
- **查看架构**：`docs/workflow-architecture.md`
- **修改规则**：`.antigravity/rules.md`
- **配置 MCP**：`mcp_config/mcp_config.json`

## 5. 关键文件

| 文件 | 重要性 | 说明 |
|------|--------|------|
| `antigravity-team.md` | ⭐⭐⭐ | 主编排工作流 |
| `_execution-guard.md` | ⭐⭐⭐ | 执行守卫 |
| `_vooda-loop.md` | ⭐⭐ | 认知循环模板 |
| `mcp_config.json` | ⭐⭐ | 工具配置 |
