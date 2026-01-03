---
description: 测试工程师技能 - 生成验收演示文档与录屏
role: qa_engineer
skill: walkthrough
mcp_tools: [puppeteer]
---

# Skill: Walkthrough / 演示演练

生成验收演示文档，包含截图和录屏证据。

## Steps

### Step 1: Observe - 收集测试成果

```yaml
inputs:
  - test_report: "测试报告"
  - screenshots: "所有截图"
  - recordings: "录屏文件"
  - prd: "验收标准"
```

### Step 2: Orient - 规划演示结构

```yaml
walkthrough_structure:
  1: "功能概述"
  2: "实现亮点"
  3: "演示流程"
  4: "测试证据"
  5: "验收确认"
```

### Step 3: Decide - 选择关键场景

```yaml
demo_scenarios:
  - name: "核心流程演示"
    priority: "P0"
    media: ["screenshot", "video"]
    
  - name: "边缘情况处理"
    priority: "P1"
    media: ["screenshot"]
```

### Step 4: Act - 生成演示文档

```markdown
# Walkthrough: [功能名称]

## 1. Feature Overview / 功能概述

本次实现了 [功能描述]，满足以下需求：
- ✅ 需求点1
- ✅ 需求点2

## 2. Implementation Highlights / 实现亮点

- 采用了 [技术方案]
- 性能优化：[指标]
- 代码质量：[覆盖率]

## 3. Demo Flow / 演示流程

### 3.1 用户登录
![Step 1: 打开登录页](./screenshots/demo_login_01.png)

用户打开登录页面，输入凭证。

![Step 2: 登录成功](./screenshots/demo_login_02.png)

登录成功后跳转到首页。

### 3.2 核心功能操作

![操作演示](./screenshots/demo_feature.png)

展示核心功能的操作过程。

## 4. Test Evidence / 测试证据

### 4.1 测试覆盖

| 类型 | 数量 | 通过率 |
|------|------|--------|
| 单元测试 | 45 | 100% |
| E2E 测试 | 12 | 100% |

### 4.2 关键截图

````carousel
![登录页面](./screenshots/login.png)
<!-- slide -->
![首页](./screenshots/home.png)
<!-- slide -->
![功能页面](./screenshots/feature.png)
````

### 4.3 完整演示视频

![功能演示录屏](./recordings/demo.webp)

## 5. Acceptance Checklist / 验收确认

基于 PRD 验收标准：

- [x] AC-001: 用户可以登录
- [x] AC-002: 数据正确展示
- [x] AC-003: 错误提示友好
- [x] AC-004: 响应时间 < 200ms

## 6. Summary / 总结

本次开发周期：X 天
测试发现缺陷：X 个（已全部修复）
最终验收状态：✅ **PASS**

---

> 验收人员确认后，本功能可进入发布流程。
```

### Step 5: Validate - 确认完整性

- [ ] 所有验收标准都有证据？
- [ ] 截图清晰可见？
- [ ] 录屏涵盖关键流程？

## Output

生成文件：`Walkthrough.md`

---

> [!TIP]
> Walkthrough 是项目交付的最终制品，应尽量详细展示成果。
