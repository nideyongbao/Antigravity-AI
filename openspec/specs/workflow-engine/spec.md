# Workflow Engine Specification

## 概述
工作流引擎是 Antigravity-AI 的核心组件，负责协调多角色协作。

---

## Requirement: 角色调度
系统 **必须** 支持基于角色的工作流调度。

#### Scenario: 架构师完成设计后通知开发工程师
- **WHEN** 架构师完成 Implementation_Plan.md
- **THEN** 系统调度开发工程师角色
- **AND** 开发工程师收到任务清单

#### Scenario: 测试失败触发返工
- **WHEN** 测试工程师报告测试失败
- **THEN** 系统触发 REWORK 流程
- **AND** 通知开发工程师修复

---

## Requirement: 制品验证
系统 **必须** 在阶段转换前验证制品完整性。

#### Scenario: 缺少必要制品时阻止进入下一阶段
- **WHEN** 当前阶段制品未通过验证
- **THEN** 系统拒绝进入下一阶段
- **AND** 输出缺失制品列表

---

## Requirement: 错误恢复
系统 **必须** 支持任务失败后的断点续传。

#### Scenario: 从检查点恢复
- **WHEN** 任务中断后重新启动
- **THEN** 系统读取 _metadata.json 中的检查点
- **AND** 从最后完成的阶段继续执行
