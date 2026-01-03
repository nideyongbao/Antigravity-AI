---
description: 测试工程师技能 - 基于PRD生成测试策略和用例
role: qa_engineer
skill: test_strategy
---

# Skill: Test Strategy / 测试策略

基于 PRD 验收标准生成测试策略和用例。

## Steps

### Step 1: Observe - 读取验收标准

```yaml
inputs:
  - prd: "PRD 文档"
  - acceptance_criteria: "验收标准列表"
  - code_diffs: "代码变更范围"
```

### Step 2: Orient - 规划测试类型

| 测试类型 | 适用场景 | 工具 |
|----------|----------|------|
| 单元测试 | 函数/组件逻辑 | Jest |
| 集成测试 | API 调用链 | Jest + MSW |
| E2E 测试 | 用户流程 | Puppeteer |
| 回归测试 | 受影响功能 | All |

### Step 3: Decide - 设计测试用例

```yaml
test_cases:
  - id: "TC-001"
    title: "用户登录成功"
    type: "e2e"
    priority: "P0"
    preconditions: ["用户已注册"]
    steps:
      - "打开登录页面"
      - "输入用户名和密码"
      - "点击登录按钮"
    expected: "跳转到首页，显示用户名"
    
  - id: "TC-002"
    title: "登录失败 - 密码错误"
    type: "e2e"
    priority: "P1"
    preconditions: ["用户已注册"]
    steps:
      - "打开登录页面"
      - "输入错误密码"
      - "点击登录按钮"
    expected: "显示错误提示，保持在登录页"
```

### Step 4: Act - 输出测试策略文档

```markdown
# 测试策略

## 1. 测试范围
- 新功能：[列表]
- 回归范围：[列表]

## 2. 测试用例

### P0 关键路径
| ID | 标题 | 类型 | 状态 |
|----|------|------|------|
| TC-001 | 用户登录成功 | E2E | 待执行 |

### P1 重要功能
...

### P2 边缘情况
...

## 3. 测试环境
- 浏览器：Chrome Latest
- 分辨率：1920x1080

## 4. 准入/准出标准
- 准入：代码通过编译
- 准出：P0 用例 100% 通过，P1 95% 通过
```

### Step 5: Validate - 校验覆盖度

- [ ] 每个验收标准都有测试用例？
- [ ] 关键路径有 P0 用例？
- [ ] 边缘情况已覆盖？

## Output

生成文件：`Test_Strategy.md`
