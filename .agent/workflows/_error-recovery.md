---
description: 错误恢复模块 - 任务失败时的状态保存和断点续传
role: framework
priority: high
---

# Error Recovery / 错误恢复

> [!IMPORTANT]
> 当任务执行失败时，本模块确保状态可恢复，支持断点续传。

## Recovery Protocol / 恢复协议

### 1. 失败检测

```yaml
failure_triggers:
  - compile_error: "编译/构建失败"
  - test_failure: "测试未通过"
  - gate_timeout: "审批门超时"
  - tool_error: "MCP 工具调用失败"
  - context_overflow: "上下文窗口溢出"
```

### 2. 状态保存

失败时 **MUST** 将以下信息写入 `_metadata.json`:

```json
{
  "task_id": "TASK-20260102-001",
  "status": "failed",
  "failure_info": {
    "phase": "IMPLEMENTATION",
    "role": "developer",
    "step": "COMPILE_CHECK",
    "error_type": "compile_error",
    "error_message": "Cannot find module '@/utils'",
    "timestamp": "2026-01-02T13:30:00Z"
  },
  "checkpoint": {
    "artifacts_completed": ["Task_List.json", "PRD.md", "Implementation_Plan.md"],
    "current_artifact": "Code_Diffs.md",
    "progress": 0.65
  },
  "recovery": {
    "suggested_action": "检查模块路径配置",
    "resume_from": "IMPLEMENT",
    "retry_count": 0,
    "max_retries": 3
  }
}
```

### 3. 恢复操作

```yaml
recovery_actions:
  on_failure:
    1: "保存当前状态到 _metadata.json"
    2: "输出错误诊断信息"
    3: "建议恢复操作"
    4: "通知用户"
  
  on_resume:
    1: "读取 _metadata.json 恢复上下文"
    2: "跳转至失败点前的检查点"
    3: "重新执行失败步骤"
    4: "继续后续流程"
```

---

## Timeout Protection / 超时保护

```yaml
timeouts:
  global_task: 1800s  # 30 分钟
  per_phase: 300s     # 5 分钟
  per_tool_call: 60s  # 1 分钟
  gate_approval: 600s # 10 分钟

on_timeout:
  action: "保存状态并通知用户"
  auto_retry: false
```

---

## Retry Strategy / 重试策略

```yaml
retry_config:
  max_retries: 3
  
  retry_conditions:
    - "tool_error && error.transient"
    - "compile_error && retry_count < 2"
    - "test_failure && has_fix_suggestion"
  
  no_retry_conditions:
    - "gate_rejected"      # 用户明确拒绝
    - "context_overflow"   # 需要重新规划
    - "max_retries_exceeded"
  
  backoff:
    type: "exponential"
    initial: 2s
    max: 30s
```

---

## Resume Commands / 恢复命令

### 从检查点恢复

```
┌─────────────────────────────────────────────────────────────┐
│ [RESUME] task_id=TASK-20260102-001                          │
│ [FROM]   checkpoint=IMPLEMENT                               │
│ [SKIP]   Task_List.json ✓, PRD.md ✓, Implementation_Plan ✓  │
└─────────────────────────────────────────────────────────────┘
```

### 重试失败步骤

```
┌─────────────────────────────────────────────────────────────┐
│ [RETRY]  step=COMPILE_CHECK                                 │
│ [ATTEMPT] 2/3                                               │
│ [CONTEXT] 应用上次错误修复建议                               │
└─────────────────────────────────────────────────────────────┘
```

---

## Integration / 集成方式

在主编排器中引用：

```yaml
# antigravity-team.md
error_handling:
  module: "_error-recovery.md"
  on_any_failure:
    - invoke: "save_checkpoint"
    - invoke: "notify_user_with_recovery_options"
```

---

## Diagnostic Output / 诊断输出

失败时输出诊断信息：

```
═══════════════════════════════════════════════════════════════
[ERROR] 任务执行失败
[PHASE] IMPLEMENTATION → COMPILE_CHECK
[TYPE]  compile_error
[MSG]   Cannot find module '@/utils'
───────────────────────────────────────────────────────────────
[DIAGNOSIS]
  • 检查 tsconfig.json 的 paths 配置
  • 验证 @/utils 模块是否存在
  • 确认导入路径是否正确
───────────────────────────────────────────────────────────────
[RECOVERY OPTIONS]
  1. 自动修复: 尝试修正路径配置
  2. 手动介入: 请用户检查配置
  3. 跳过步骤: 继续下一阶段（不推荐）
═══════════════════════════════════════════════════════════════
```

---

> [!TIP]
> 使用 `_metadata.json` 中的 `checkpoint` 字段可以快速恢复长时间运行的任务。
