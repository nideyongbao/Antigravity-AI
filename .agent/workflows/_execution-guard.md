---
description: 执行守卫模块 - 强制工作流严格执行的检查点协议
role: framework
priority: critical
depends_on: [_artifact-validator.md, _error-recovery.md]
---

# Execution Guard / 执行守卫

> [!CAUTION]
> **本模块定义强制执行规则。所有角色工作流 MUST 遵守以下协议，违反将导致任务失败。**

## 核心强制规则

### Rule 1: 阶段标记输出 (Mandatory Phase Markers)

每个 VOODA 阶段开始和结束时，**MUST** 输出阶段标记：

```
═══════════════════════════════════════════════════════════════
[PHASE] OBSERVE - 开始观察阶段
[ROLE] Product Manager
[TASK] 需求分析
[VOODA] 子代理调度中...
═══════════════════════════════════════════════════════════════
```

```
───────────────────────────────────────────────────────────────
[PHASE_COMPLETE] OBSERVE ✓
[ARTIFACTS] Observation_State.json
[NEXT] ORIENT
───────────────────────────────────────────────────────────────
```

### Rule 2: 制品产出检查点 (Artifact Checkpoints)

每个角色 **MUST** 在完成时产出规定的制品，并通过 `_artifact-validator.md` 验证：

| 角色 | 必须产出的制品 | 存放路径 | 验证规则 |
|------|---------------|----------|----------|
| Project Lead | `Task_List.json` | `{output_dir}/01_planning/` | json_schema |
| Product Manager | `PRD.md`, `PRD_Validation_Report.md` | `{output_dir}/02_requirements/` | markdown_sections |
| Architect | `Implementation_Plan.md`, `API_Spec.yaml` | `{output_dir}/03_design/` | markdown_sections, openapi |
| Developer | `Code_Diffs.md`, Source Files | `{output_dir}/04_implementation/` | code_diff_format |
| QA Engineer | `Test_Report.md`, `Walkthrough.md` | `{output_dir}/05_testing/` | markdown_sections |

### Rule 3: 门控阻断 (Gate Blocking)

```yaml
gates:
  approval_gate:
    description: "需要用户明确批准才能继续"
    blocking: true
    applies_to: [PRD, Implementation_Plan]
    action: |
      MUST 调用 notify_user 请求审批
      MUST 等待用户回复 "approved" 或 "批准"
      SHALL NOT 自行判断为通过
      
  rework_gate:
    description: "测试失败触发返工"
    blocking: true
    applies_to: [Test Failure]
    action: |
      MUST 生成 Defect_Report.md
      MUST 调用 /skill-dev-bug-fix
      SHALL NOT 跳过修复直接标记通过
      MUST 更新 _metadata.json 的 rework_count
```

### Rule 4: 子工作流调用指令 (Workflow Invocation Protocol)

调用子工作流时 **MUST** 使用标准格式：

```
┌─────────────────────────────────────────────────────────────┐
│ [INVOKE] /role-product-manager                              │
│ [INPUT]  Task_List.json                                     │
│ [MCP]    rednote-MCP, fabric-mcp-server                     │
│ [EXPECT] PRD.md, PRD_Validation_Report.md                   │
└─────────────────────────────────────────────────────────────┘
```

完成后 **MUST** 输出：

```
┌─────────────────────────────────────────────────────────────┐
│ [COMPLETE] /role-product-manager                            │
│ [OUTPUT]   PRD.md ✓, PRD_Validation_Report.md ✓             │
│ [VALIDATED] /_artifact-validator ✓                          │
│ [STATUS]   PASS                                             │
└─────────────────────────────────────────────────────────────┘
```

### Rule 5: MCP 工具权限检查 (MCP Authorization)

调用 MCP 工具前 **MUST** 验证权限：

```yaml
before_mcp_call:
  1: "读取 mcp_config.json 的 roleAuthorization"
  2: "验证当前角色有权访问该 MCP Server"
  3: "验证调用的工具在允许列表中"
  4: "检查 securityControls 是否需要审批"
  
on_unauthorized:
  action: "拒绝调用并记录警告"
```

---

## 执行模式定义

### Strict Mode (严格模式) - 默认

```yaml
strict_mode:
  enabled: true
  rules:
    - phase_markers: required
    - artifact_checkpoints: required
    - artifact_validation: enforced  # 自动验证
    - gate_blocking: enforced
    - workflow_invocation: standardized
    - mcp_authorization: enforced
    - error_recovery: enabled
    
  violations:
    on_missing_artifact: FAIL
    on_validation_failed: BLOCK
    on_skipped_phase: FAIL
    on_unapproved_gate: BLOCK
    on_unauthorized_mcp: FAIL
```

### Express Mode (快速模式)

```yaml
express_mode:
  enabled: true  # 可通过用户命令激活
  trigger: "用户说 '快速模式' 或 'express' 或任务复杂度 < 3"
  
  rules:
    - phase_markers: optional
    - artifact_checkpoints: required  # 仍然强制
    - artifact_validation: relaxed  # 宽松验证
    - gate_blocking: advisory  # 变为建议
    - vooda_simplified: true  # 跳过 OBSERVE/ORIENT
    
  simplified_vooda:
    phases: [DECIDE, ACT, VALIDATE]
    skip: [OBSERVE, ORIENT]
```

---

## 可信目录白名单 (Trusted Directories)

> [!NOTE]
> 以下目录中的文件操作将**自动执行**，无需用户确认：

```yaml
trusted_directories:
  auto_create:
    # 输出目录可自动创建，无需用户确认
    - "{project_root}/.antigravity-output/**"
    
  require_approval:
    # 以下目录的修改需要用户确认
    - "{project_root}/src/**"
    - "{project_root}/config/**"
    - "{project_root}/package.json"
    - "{project_root}/docker-compose.yml"
```

**适用规则**:
- `write_to_file` 操作在 `auto_create` 目录中时，设置 `SafeToAutoRun: true`
- 其他目录遵循标准安全检查流程

---

## 输出目录结构 (Output Directory)

所有制品 **MUST** 存放在统一的输出目录：

```
{project_root}/
└── .antigravity-output/
    └── {task_id}/
        ├── 01_planning/
        │   └── Task_List.json
        ├── 02_requirements/
        │   ├── PRD.md
        │   └── PRD_Validation_Report.md
        ├── 03_design/
        │   ├── Implementation_Plan.md
        │   └── API_Spec.yaml
        ├── 04_implementation/
        │   ├── Code_Diffs.md
        │   └── src/...
        ├── 05_testing/
        │   ├── Test_Strategy.md
        │   ├── Test_Report.md
        │   ├── Walkthrough.md
        │   └── screenshots/
        └── _metadata.json
```

### _metadata.json 格式

```json
{
  "task_id": "TASK-20260102-001",
  "started_at": "2026-01-02T12:00:00Z",
  "status": "in_progress",
  "execution_mode": "strict",
  "current_phase": "IMPLEMENTATION",
  "current_role": "developer",
  "artifacts": {
    "Task_List.json": { "status": "complete", "validated": true, "path": "01_planning/" },
    "PRD.md": { "status": "complete", "validated": true, "path": "02_requirements/" }
  },
  "gates_passed": ["prd_approval", "design_approval"],
  "rework_count": 0,
  "mcp_calls": [
    { "server": "github", "tool": "get_file_contents", "approved": true }
  ]
}
```

---

## 违规处理

> [!WARNING]
> **以下行为将被视为违规并触发失败：**

1. ❌ 跳过 VOODA 阶段直接输出结果
2. ❌ 未产出必需制品就声称完成
3. ❌ 制品未通过 `_artifact-validator` 验证
4. ❌ 未经用户批准就跨过审批门
5. ❌ 测试失败后未触发返工流程
6. ❌ 制品未存放在规定目录
7. ❌ 调用未授权的 MCP 工具

---

## 检查点清单

在每个角色结束时，**MUST** 执行以下检查：

```markdown
## Checkpoint Verification

- [ ] 所有 VOODA 阶段都已执行并标记？
- [ ] 必需制品都已生成并存放正确位置？
- [ ] 制品通过 /_artifact-validator 验证？
- [ ] 如有审批门，用户是否已明确批准？
- [ ] 如有失败，是否触发了正确的返工/恢复流程？
- [ ] _metadata.json 是否已更新？
- [ ] MCP 工具调用是否都经过授权检查？
```

---

## 错误恢复集成

```yaml
on_any_error:
  module: "_error-recovery.md"
  actions:
    - save_checkpoint: true
    - notify_user: true
    - suggest_recovery: true
```

---

> [!IMPORTANT]
> 本守卫模块由 `antigravity-team.md` 强制加载，任何角色工作流启动前都会检查是否遵守这些规则。

---

## Rule 7: 上下文管理 (Context Management)

> [!CAUTION]
> **每个阶段结束时 MUST 更新以下状态，以支持断点续传和跨阶段记忆。**

```yaml
context_management:
  after_each_phase:
    - action: "更新 _metadata.json"
      content: |
        {
          "current_phase": "{phase_name}",
          "last_action": "{action_description}",
          "key_decisions": ["{decision_1}", "{decision_2}"],
          "pending_issues": ["{issue_1}"]
        }
        
    - action: "记录关键决策到 memory MCP (如可用)"
      mcp_call: |
        memory_store({
          "key": "{task_id}_{phase}",
          "value": "{phase_summary}",
          "metadata": {"type": "phase_summary"}
        })

  on_error:
    - action: "保存错误上下文"
      content: |
        {
          "error_type": "{error_type}",
          "error_message": "{message}",
          "attempted_fixes": ["{fix_1}", "{fix_2}"],
          "files_modified": ["{file_1}", "{file_2}"]
        }
        
    - action: "记录到 _metadata.json error_log"
      
  on_resume:
    - action: "读取 _metadata.json 恢复状态"
    - action: "查询 memory MCP 获取历史决策"
    - action: "验证已完成阶段的产物完整性"
```

### 跨阶段信息传递

```yaml
inter_phase_data:
  from_requirement_to_design:
    - "PRD.md 中的功能列表"
    - "用户故事验收标准"
    
  from_design_to_implementation:
    - "Implementation_Plan.md 的文件列表"
    - "技术栈决策"
    - "目录结构"
    
  from_implementation_to_testing:
    - "已实现功能清单"
    - "已知限制"
    - "验证命令"
```

---

## Rule 8: 阶段间总结 (Phase Summary Report)

每个主要阶段结束时 **MUST** 生成简短总结供下一阶段参考:

```markdown
## Phase Summary: {PHASE_NAME}

### 完成的工作
- {item_1}
- {item_2}

### 关键决策
- {decision_1}: {rationale}

### 下一阶段需要的信息
- {info_1}
- {info_2}

### 已知问题/风险
- {issue_1}
```

