---
description: 测试工程师技能 - Puppeteer自动化E2E测试
role: qa_engineer
skill: e2e_test
mcp_tools: [puppeteer]
---

# Skill: E2E Test / E2E 测试

使用 Puppeteer MCP 执行端到端自动化测试。

## Steps

### Step 1: Observe - 准备测试环境

```yaml
inputs:
  - test_cases: "测试用例列表"
  - app_url: "应用 URL"
  
setup:
  - 确认应用已启动
  - 准备测试数据
```

### Step 2: Orient - 加载测试用例

```yaml
current_test:
  id: "TC-001"
  title: "用户登录成功"
  steps:
    - action: "navigate"
      url: "/login"
    - action: "fill"
      selector: "#username"
      value: "testuser"
    - action: "fill"
      selector: "#password"
      value: "password123"
    - action: "click"
      selector: "#submit-btn"
    - action: "wait"
      selector: ".welcome-message"
    - action: "screenshot"
      name: "login_success"
```

### Step 3: Decide - 规划执行顺序

```yaml
execution_plan:
  1: "导航到目标页面"
  2: "执行交互操作"
  3: "等待响应"
  4: "验证结果"
  5: "截取证据"
```

### Step 4: Act - 执行 Puppeteer 操作

```yaml
puppeteer_actions:
  - puppeteer_navigate:
      url: "http://localhost:3000/login"
      
  - puppeteer_fill:
      selector: "#username"
      value: "testuser"
      
  - puppeteer_fill:
      selector: "#password"
      value: "password123"
      
  - puppeteer_click:
      selector: "#submit-btn"
      
  - puppeteer_screenshot:
      name: "login_result"
      
  - puppeteer_evaluate:
      script: "document.querySelector('.welcome-message').textContent"
```

### Step 5: Validate - 验证测试结果

```yaml
validation:
  - compare: "actual_result vs expected_result"
  - on_match: "PASS"
  - on_mismatch: "FAIL"
  
record:
  - test_id: "TC-001"
  - status: "PASS | FAIL"
  - duration: "2.3s"
  - screenshot: "login_result.png"
  - error: "null | error message"
```

## Test Report Format

```markdown
# E2E Test Report

## Summary
- Total: 10
- Passed: 9
- Failed: 1
- Duration: 45s

## Results

### ✅ TC-001: 用户登录成功
- Duration: 2.3s
- Screenshot: ![](./screenshots/TC-001.png)

### ❌ TC-002: 登录失败处理
- Duration: 5.1s
- Error: Timeout waiting for ".error-message"
- Screenshot: ![](./screenshots/TC-002.png)
```

## Output

- 测试报告：`E2E_Test_Report.md`
- 截图目录：`./screenshots/`
