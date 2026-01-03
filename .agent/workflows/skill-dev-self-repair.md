---
description: 开发工程师技能 - 编译/测试错误自动修复
role: developer
skill: self_repair
---

# Skill: Self Repair / 自我修复

自动分析和修复编译错误、测试失败等问题。

## Steps

### Step 1: Observe - 收集错误信息

```yaml
inputs:
  - error_type: "compile | test | lint"
  - error_message: "完整错误信息"
  - error_location: "文件和行号"
  - source_code: "相关代码上下文"
```

### Step 2: Orient - 分类错误

| 错误类型 | 示例 | 修复策略 |
|----------|------|----------|
| 语法错误 | Unexpected token | 检查语法规则 |
| 类型错误 | Type mismatch | 修正类型定义 |
| 引用错误 | Cannot find module | 检查导入路径 |
| 逻辑错误 | Test assertion failed | 修正实现逻辑 |

### Step 3: Decide - Reflexion 分析

```yaml
reflexion:
  1_what_went_wrong: "错误的具体原因"
  2_why_it_happened: "为什么会产生这个错误"
  3_how_to_fix: "修复方案"
  4_how_to_prevent: "如何避免再次发生"
```

### Step 4: Act - 应用修复

```yaml
fix_actions:
  - locate_issue: "定位到具体代码行"
  - apply_fix: "修改代码"
  - verify_syntax: "确保语法正确"
```

### Step 5: Validate - 重新验证

```yaml
validation:
  - recompile: "重新编译"
  - rerun_tests: "重新运行测试"
  - check_no_regression: "确保没有引入新问题"
  
on_success:
  - continue_workflow
  
on_failure:
  - increment_retry_count
  - if retry_count < 3: goto Step 1
  - else: escalate_to_human
```

## Error Patterns / 常见错误模式

### 1. 类型不匹配

```typescript
// Error: Type 'string' is not assignable to type 'number'
// Before
const count: number = "5";

// After
const count: number = 5;
// or
const count: number = parseInt("5");
```

### 2. 缺少导入

```typescript
// Error: Cannot find name 'useState'
// Before
const [state, setState] = useState(0);

// After
import { useState } from 'react';
const [state, setState] = useState(0);
```

### 3. 测试断言失败

```typescript
// Error: Expected 'hello' but received 'Hello'
// 分析: 大小写不匹配
// 修复: 检查实现逻辑或调整期望值
```

## Output

修复后的代码，继续原工作流。

> [!WARNING]
> 最多重试 3 次，超过后上报人工介入。
