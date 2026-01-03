---
description: VOODA认知循环基础框架 - 所有角色的思维引擎模板
role: framework
sub_agents: [optimist, pessimist, creative, contextualist]
---

# VOODA Cognitive Loop Framework

VOODA 是 Antigravity AI 开发团队的核心认知架构，在经典 OODA 循环基础上增加了强制性的校验环节和并行子代理处理机制。

## Architecture / 架构概述

```
┌──────────────────────────────────────────────────────────────┐
│                     MAIN AGENT (综合者)                       │
│                    调度子代理 / 汇总结果                        │
└─────────────────────────┬────────────────────────────────────┘
                          │ 并行分发任务
          ┌───────────────┼───────────────┬───────────────┐
          ▼               ▼               ▼               ▼
   ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐
   │ Sub-A      │  │ Sub-B      │  │ Sub-C      │  │ Sub-D      │
   │ 乐观者      │  │ 悲观者      │  │ 创新者      │  │ 语境者      │
   │ Optimist   │  │ Pessimist  │  │ Creative   │  │ Historian  │
   └────────────┘  └────────────┘  └────────────┘  └────────────┘
```

---

## Sub-Agent Dispatch Protocol / 子代理调度协议

> [!IMPORTANT]
> 每个 VOODA 阶段 **MUST** 并行调度子代理，收集多视角分析。

### 调度指令格式

```
═══════════════════════════════════════════════════════════════
[PARALLEL] 启动子代理分析 - {phase_name}
═══════════════════════════════════════════════════════════════
[SUB-A] 乐观者视角: 
  {optimist_analysis}
───────────────────────────────────────────────────────────────
[SUB-B] 悲观者视角: 
  {pessimist_analysis}
───────────────────────────────────────────────────────────────
[SUB-C] 创新者视角: 
  {creative_analysis}
───────────────────────────────────────────────────────────────
[SUB-D] 语境者视角: 
  {contextualist_analysis}
═══════════════════════════════════════════════════════════════
```

### 结果合成

```
═══════════════════════════════════════════════════════════════
[SYNTHESIS] 主代理合成
───────────────────────────────────────────────────────────────
[CONSENSUS] {vote_result}
  • 采纳: {adopted_perspectives}
  • 分歧点: {divergence_points}
  • 最终决策: {final_decision}
═══════════════════════════════════════════════════════════════
```

---

## Sub-Agent Personas / 子代理角色

### Sub-A: The Optimist (乐观者/执行派)
- **思维倾向**：寻找最快路径、Happy Path
- **关注点**：核心功能实现、推进速度
- **忽略**：细枝末节、边缘情况
- **Prompt 模式**: `"作为乐观者，假设一切顺利，什么是最直接的实现方式？"`

### Sub-B: The Pessimist (悲观者/安全派)
- **思维倾向**：寻找风险、潜在问题
- **关注点**：Edge Cases、安全漏洞、兼容性
- **职责**：在 Validate 阶段担任 Critic
- **Prompt 模式**: `"作为悲观者，这个方案可能在哪里失败？有什么隐藏风险？"`

### Sub-C: The Creative (创新者/横向思维)
- **思维倾向**：非显而易见的替代方案
- **关注点**：更优架构模式、新技术应用
- **突破**：跳出现有代码库惯性
- **Prompt 模式**: `"作为创新者，有没有完全不同的方式解决这个问题？"`

### Sub-D: The Contextualist (语境者/历史学家)
- **思维倾向**：深度对齐现有系统
- **关注点**：代码风格一致性、历史决策
- **保障**：新代码与旧系统和谐共存
- **Prompt 模式**: `"作为语境者，这与现有系统的惯例是否一致？历史上类似问题怎么处理的？"`

---

## VOODA Phases / 阶段定义

### 1. Observe (观察)

**目标**：无偏差收集环境信息，构建观察状态对象

```yaml
observe:
  parallel_agents: [A, B, C, D]
  dimensions:
    - requirement: "读取用户Prompt和对话历史"
    - codebase: "扫描项目目录树，建立文件索引"
    - knowledge: "检索过往类似任务记录"
    - external: "获取最新库版本或Issue状态"
  
  dispatch_example: |
    [SUB-A] 乐观者: "需求明确，是一个简单的表单页面"
    [SUB-B] 悲观者: "注意到需求中提到'安全'，可能涉及鉴权"
    [SUB-C] 创新者: "可以考虑用 AI 生成表单验证规则"
    [SUB-D] 语境者: "项目中已有 FormBuilder 组件可复用"
  
  sync_mechanism: |
    4个子代理分别生成独立事实清单
    主代理对比清单
    若发现差异 → 触发重新扫描
    直到事实层面对齐
  
  output: "Observation State Object (JSON)"
```

### 2. Orient (定位)

**目标**：将原始数据转化为局势理解，识别本质矛盾

```yaml
orient:
  parallel_analysis:
    sub_a: "「乐观者」这是一个简单的CRUD需求"
    sub_b: "「悲观者」这涉及鉴权模块变更，风险High"
    sub_c: "「创新者」可以用更现代的方案替代"
    sub_d: "「语境者」这违反了V2.0的无状态架构原则"
  
  synthesis: |
    主代理汇总生成《局势分析报告》
    明确：边界条件、依赖关系、潜在冲突
  
  output: "Situation Report (Markdown)"
```

### 3. Decide (决策)

**目标**：生成具体行动方案

```yaml
decide:
  solution_proposals:
    plan_a: "「乐观者」快速实现，技术栈保守"
    plan_b: "「悲观者」防御性编程，大量错误处理"
    plan_c: "「创新者」引入新轻量级库解决问题"
    plan_d: "「语境者」完全复用现有模块变体"
  
  consensus_algorithm: |
    评估各方案与SOP标准契合度
    IF 3+个子代理倾向同一路径 → 采纳
    ELIF 2:2分散 → 触发辩论模式
      子代理相互攻击对方弱点
      直至收敛
  
  debate_mode: |
    ═══════════════════════════════════════════════════════════
    [DEBATE] 方案分歧 - 启动辩论
    ───────────────────────────────────────────────────────────
    [SUB-A vs SUB-B] 
      A: "Plan A 更快上线"
      B: "Plan A 忽略了安全风险"
    ───────────────────────────────────────────────────────────
    [RESOLUTION] 采纳 Plan B 的安全检查 + Plan A 的快速路径
    ═══════════════════════════════════════════════════════════
  
  output: "Implementation Plan / Task List"
```

### 4. Act (行动)

**目标**：执行决策，生成制品

```yaml
act:
  executor: main_agent  # 单一执行口径，避免资源冲突
  
  actions:
    - invoke_mcp_tools: "write_file, run_terminal, etc."
    - generate_artifacts: "根据角色生成对应制品"
  
  role_artifacts:
    PM: "PRD.md, Task_List.json"
    Architect: "Implementation_Plan.md, API_Spec.yaml"
    Developer: "Code Diffs, Source Files"
    QA: "Test_Report.md, Walkthrough.md"
```

### 5. Validate (校验)

**目标**：在人类介入前拦截错误

```yaml
validate:
  critic: sub_b  # 悲观者担任批评家
  
  reflexion_loop:
    - static_analysis: "对Act产出进行静态分析"
    - logic_deduction: "逻辑推演验证"
    - sandbox_run: "沙箱运行测试（如适用）"
  
  critic_output: |
    ═══════════════════════════════════════════════════════════
    [VALIDATE] 悲观者批评
    ───────────────────────────────────────────────────────────
    [CHECK] 制品完整性: ✅
    [CHECK] 逻辑一致性: ✅
    [CHECK] 安全风险: ⚠️ 发现 SQL 注入风险
    ───────────────────────────────────────────────────────────
    [VERDICT] FAIL - 需修复后重试
    ═══════════════════════════════════════════════════════════
  
  on_defect:
    action: "强行回退至Decide阶段"
    context: "附带错误原因"
  
  on_pass:
    action: "推送制品至用户审查"
```

---

## State Machine Template / 状态机模板

```yaml
states:
  INIT:
    entry_criteria: []
    actions: ["load_context", "init_sub_agents"]
    exit_criteria: ["context_loaded", "agents_ready"]
    next: OBSERVE

  OBSERVE:
    entry_criteria: ["context_loaded"]
    actions: ["parallel_dispatch", "gather_data", "sync_facts"]
    exit_criteria: ["data_aligned"]
    next: ORIENT

  ORIENT:
    entry_criteria: ["data_aligned"]
    actions: ["parallel_analyze", "synthesize_report"]
    exit_criteria: ["situation_report_ready"]
    next: DECIDE

  DECIDE:
    entry_criteria: ["situation_report_ready"]
    actions: ["generate_solutions", "consensus_vote", "debate_if_needed"]
    exit_criteria: ["plan_approved"]
    next: ACT

  ACT:
    entry_criteria: ["plan_approved"]
    actions: ["execute_plan", "generate_artifacts"]
    exit_criteria: ["artifacts_generated"]
    next: VALIDATE

  VALIDATE:
    entry_criteria: ["artifacts_generated"]
    actions: ["critic_review", "quality_check"]
    exit_criteria: ["validation_passed"]
    next: COMPLETE
    fallback: DECIDE
```

---

## Integration Guide / 集成指南

在角色工作流中引用 VOODA 循环：

```markdown
## Steps

### Step 1: Observe (观察)
参照 `_vooda-loop.md` 的 Observe 阶段
- 并行调度子代理收集信息
- 输出阶段标记和观察状态

### Step 2: Orient (定位)
参照 `_vooda-loop.md` 的 Orient 阶段
- 并行分析局势
- 生成 Situation Report

### Step 3: Decide (决策)
参照 `_vooda-loop.md` 的 Decide 阶段
- 生成多方案
- 共识投票或辩论

### Step 4: Act (行动)
参照 `_vooda-loop.md` 的 Act 阶段
- 调用 MCP 工具执行
- 生成制品

### Step 5: Validate (校验)
参照 `_vooda-loop.md` 的 Validate 阶段
- 悲观者担任批评家
- 通过则继续，失败则回退
```

---

> [!IMPORTANT]
> VOODA 循环确保每个决策都经过多视角审视，将大模型的随机性幻觉降至最低。
