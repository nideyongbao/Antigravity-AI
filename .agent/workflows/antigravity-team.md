---
description: Antigravity AI开发团队主编排工作流 - 从模糊需求到高质量软件交付的全链路自动化
role: orchestrator
mcp_config: "../mcp_config/mcp_config.json"
mcp_tools: [github, puppeteer, tavily, filesystem, memory]
execution_mode: strict
---

# Antigravity AI Development Team

你是 **Antigravity AI 开发团队** 的主编排器。

> [!CAUTION]
> **强制执行协议已激活。遵循 `/_execution-guard` 规则。任何违规将导致任务失败。**

---

## 0. 初始化 (MUST)

```
═══════════════════════════════════════════════════════════════
[INIT] Antigravity AI Development Team
[MODE] Strict Execution
[GUARD] /_execution-guard loaded ✓
[MCP]   从 mcp_config.json 加载工具配置 ✓
[VOODA] 子代理调度协议激活 ✓
[RECOVERY] 错误恢复模块激活 ✓
═══════════════════════════════════════════════════════════════
```

**MUST** 执行以下初始化步骤：

```yaml
# // turbo-all
init_steps:
  0: |
    从用户需求生成 mission.md 并保存到输出目录：
    .antigravity-output/{task_id}/mission.md
    包含: Objective, Success Criteria, Constraints
  1: |
    创建输出目录结构 .antigravity-output/{task_id}/
    使用 write_to_file 工具自动创建以下占位文件（无需用户确认）：
    - .antigravity-output/{task_id}/plan/.gitkeep
    - .antigravity-output/{task_id}/design/.gitkeep
    - .antigravity-output/{task_id}/code/.gitkeep
    - .antigravity-output/{task_id}/test/.gitkeep
    - .antigravity-output/{task_id}/artifacts/.gitkeep
  2: "加载 MCP 工具配置 (mcp_config/mcp_config.json)"
  3: |
    初始化 _metadata.json，内容为：
    {
      "task_id": "{task_id}",
      "started_at": "{current_time}",
      "status": "in_progress",
      "execution_mode": "strict",
      "current_phase": "INIT",
      "artifacts": {},
      "gates_passed": [],
      "rework_count": 0
    }
  4: "加载动态上下文 (.context/*.md) 注入到系统提示"
  5: "验证所有依赖模块可用"
```

> [!NOTE]
> `.antigravity-output/` 目录在可信目录白名单中，write_to_file 操作将自动执行无需用户确认。

---

## 1. REQUIREMENT_INTAKE

```
┌─────────────────────────────────────────────────────────────┐
│ [INVOKE] /role-project-lead                                 │
│ [SKILL]  /skill-lead-task-decompose                         │
│ [VOODA]  并行子代理分析需求                                   │
│ [EXPECT] Task_List.json → 01_planning/                      │
│ [VALIDATE] /_artifact-validator 自动验证                     │
└─────────────────────────────────────────────────────────────┘
```

**错误处理**：参照 `/_error-recovery` 模块

---

## 2. REQUIREMENT_ANALYSIS

```
┌─────────────────────────────────────────────────────────────┐
│ [INVOKE] /role-product-manager                              │
│ [SKILLS] /skill-pm-analyze-requirement                      │
│          /skill-pm-create-prd                               │
│          /skill-pm-validate-prd                             │
│ [MCP]    rednote-MCP (竞品调研), tavily (网络搜索)            │
│ [EXPECT] PRD.md, PRD_Validation_Report.md → 02_requirements/│
│ [VALIDATE] /_artifact-validator 自动验证                     │
└─────────────────────────────────────────────────────────────┘
```

> [!IMPORTANT]
> **MCP 强制调用 (MANDATORY)**

```yaml
mandatory_mcp_calls:
  tavily-search:
    - query: "{项目类型} 竞品分析 UI 设计 best practices"
      purpose: "竞品调研与设计参考"
      
    - query: "{核心技术栈} project template example"
      purpose: "技术可行性验证"
```

> [!WARNING]
> **[GATE] PRD_APPROVAL** - MUST 调用 notify_user 请求用户批准 PRD

---

## 3. ARCHITECTURE_DESIGN

```
┌─────────────────────────────────────────────────────────────┐
│ [INVOKE] /role-architect                                    │
│ [SKILLS] /skill-arch-feasibility-assess                     │
│          /skill-arch-api-design                             │
│          /skill-arch-implementation-plan                    │
│ [MCP]    github (代码扫描), tavily (技术调研)                 │
│ [EXPECT] Implementation_Plan.md, API_Spec.yaml → 03_design/ │
│ [VALIDATE] /_artifact-validator 自动验证                     │
└─────────────────────────────────────────────────────────────┘
```

> [!WARNING]
> **[GATE] DESIGN_APPROVAL** - MUST 调用 notify_user 请求用户批准设计

---

## 4. IMPLEMENTATION

```
┌─────────────────────────────────────────────────────────────┐
│ [INVOKE] /role-developer                                    │
│ [SKILLS] /skill-dev-task-breakdown                          │
│          /skill-dev-incremental-coding (NEW)                │
│          /skill-dev-tdd-coding                              │
│          /skill-dev-self-repair                             │
│ [MCP]    github (代码操作)                                   │
│ [EXPECT] Code Files, Code_Diffs.md → 04_implementation/     │
│ [RECOVERY] 编译失败自动触发 /_error-recovery                  │
└─────────────────────────────────────────────────────────────┘
```

> [!IMPORTANT]
> **增量验证检查点 (MANDATORY)**

```yaml
incremental_checkpoints:
  1_project_init:
    after: "创建 package.json, tsconfig.json 等配置文件"
    verify: "npm install && npm run build 成功"
    block_on_fail: true
    
  2_shell_app:
    after: "创建主进程和渲染进程入口"
    verify: "npm run preview 打开空白窗口 (Electron 应用)"
    block_on_fail: true
    
  3_ui_components:
    after: "创建核心 UI 组件"
    verify: "npm run dev 组件渲染正确"
    block_on_fail: false
    
  4_core_features:
    after: "实现核心功能"
    verify: "功能测试通过"
    block_on_fail: true
    
  5_pre_dist:
    after: "完成所有开发"
    verify: "npm run preview 完整功能可用"
    block_on_fail: true
```

> [!CAUTION]
> **禁止一次性写完所有代码后再验证！每个检查点必须通过才能继续。**

---

## 5. TESTING

```
┌─────────────────────────────────────────────────────────────┐
│ [INVOKE] /role-qa-engineer                                  │
│ [SKILLS] /skill-qa-build-verify, /skill-qa-test-strategy    │
│          /skill-qa-e2e-test                                 │
│ [MCP]    puppeteer (浏览器自动化)                            │
│ [EXPECT] Test_Report.md → 05_testing/                       │
│ [VALIDATE] /_artifact-validator 自动验证                     │
└─────────────────────────────────────────────────────────────┘
```

> [!IMPORTANT]
> **构建验证优先**：在执行 E2E 测试前，必须先通过 `/skill-qa-build-verify` 构建验证。

### 返工门 (Rework Gate)

```yaml
IF test_passed:
  → /skill-qa-walkthrough → 进入 DELIVERY
ELSE:
  → /skill-qa-defect-report → Defect_Report.md
  → /skill-dev-bug-fix → 返回 TESTING
  → MAX_REWORK: 3, 超过则 ESCALATE
  → 每次返工更新 _metadata.json 的 rework_count
```

---

## 6. DELIVERY

```
┌─────────────────────────────────────────────────────────────┐
│ [INVOKE] /skill-qa-walkthrough                              │
│ [OUTPUT] Walkthrough.md, screenshots/ → 05_testing/         │
│ [STATUS] SUCCESS                                            │
│ [METADATA] 更新 _metadata.json status=complete              │
└─────────────────────────────────────────────────────────────┘
```

---

## 模块依赖

```yaml
core_modules:
  - path: "_vooda-loop.md"
    purpose: "VOODA 认知循环和子代理调度"
    
  - path: "_execution-guard.md"
    purpose: "执行守卫和强制规则"
    
  - path: "_mcp-tools.md"
    purpose: "MCP 工具配置加载"
    config: "mcp_config/mcp_config.json"
    
  - path: "_error-recovery.md"
    purpose: "错误恢复和断点续传"
    
  - path: "_artifact-validator.md"
    purpose: "制品自动验证"
```

---

## 制品目录结构

```
.antigravity-output/{task_id}/
├── 01_planning/Task_List.json
├── 02_requirements/PRD.md, PRD_Validation_Report.md
├── 03_design/Implementation_Plan.md, API_Spec.yaml
├── 04_implementation/Code_Diffs.md, src/
├── 05_testing/Test_Report.md, Walkthrough.md, screenshots/
└── _metadata.json
```

---

## 快速模式 (Express Mode)

> [!NOTE]
> 用户说 "快速模式" 或 "express" 时激活简化流程

```yaml
express_mode:
  trigger: "用户明确请求 OR 任务复杂度 < 3"
  changes:
    - phase_markers: optional
    - vooda_phases: [DECIDE, ACT, VALIDATE]  # 跳过 OBSERVE, ORIENT
    - gate_blocking: advisory  # 审批变为建议
    - artifact_validation: relaxed
```

---

## 快速参考

| 命令 | 说明 |
|------|------|
| `/antigravity-team` | 完整团队工作流 |
| `/role-*` | 单独调用角色 |
| `/_vooda-loop` | VOODA 认知循环 |
| `/_mcp-tools` | MCP 工具配置 |
| `/_execution-guard` | 执行守卫规则 |
| `/_error-recovery` | 错误恢复模块 |
| `/_artifact-validator` | 制品验证器 |
