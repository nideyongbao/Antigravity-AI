# OpenSpec 项目配置

> 简化版 OpenSpec 系统，用于结构化变更管理。

## 何时需要创建 Proposal

| 场景 | 需要 Proposal |
|------|:-------------:|
| 新功能/特性 | ✅ |
| 重构/架构变更 | ✅ |
| Breaking Changes | ✅ |
| Bug 修复 | ❌ |
| 文档更新 | ❌ |
| 依赖升级（非Breaking） | ❌ |

## 变更命名规则

- **格式**：动词 + 名词，kebab-case
- **示例**：
  - `add-user-auth` - 新增用户认证
  - `refactor-api-layer` - 重构 API 层
  - `remove-legacy-code` - 移除遗留代码

## 目录结构

```
openspec/
├── project.md          # 本文件
├── AGENTS.md           # AI 操作指南
├── specs/              # 已实现功能规格
│   └── {capability}/
│       └── spec.md
└── changes/            # 变更提案
    ├── {change-id}/
    │   ├── proposal.md
    │   └── tasks.md
    └── archive/        # 已归档
```

## 工作流程

1. **创建提案**：`changes/{change-id}/proposal.md`
2. **定义任务**：`changes/{change-id}/tasks.md`
3. **实施开发**：按 tasks.md 逐项完成
4. **归档**：移动到 `archive/YYYY-MM-DD-{change-id}/`
