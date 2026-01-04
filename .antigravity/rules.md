# 🛸 Antigravity 核心规则

> 本文件定义 AI 代理的核心行为准则。详细规范请参阅同目录下的专项文件。

## 语言规范
1. 代码变量名、日志信息保持英文
2. 解释性文本、报告、分析使用简体中文

## 工作模式
1. **复杂任务**：必须调用 `/antigravity-team` 工作流
2. **制品存储**：所有输出存放于 `.antigravity-output/{task_id}/`
3. **任务目标**：开始前必须确认 `mission.md` 中的目标

## 认知循环（VOODA）
每个角色遵循 VOODA 循环：Observe → Orient → Decide → Act → Validate

## 相关文件
- `coding_style.md` - 编码规范
- `security.md` - 安全限制
- `permissions.md` - 权限白名单
