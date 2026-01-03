---
description: 开发工程师技能 - 测试驱动开发模式编码
role: developer
skill: tdd_coding
---

# Skill: TDD Coding / TDD 编码

采用测试驱动开发模式，先写测试后写实现。

## Steps

### Step 1: Observe - 理解任务需求

```yaml
inputs:
  - atomic_task: "当前原子任务"
  - api_spec: "接口规范"
  - existing_code: "相关现有代码"
```

### Step 2: Orient - 规划测试用例

```yaml
test_cases:
  happy_path:
    - "正常输入返回预期结果"
  edge_cases:
    - "空输入处理"
    - "边界值处理"
  error_cases:
    - "无效输入抛出错误"
```

### Step 3: Decide - 编写测试代码 (Red Phase)

```typescript
// __tests__/example.test.ts
describe('Example', () => {
  it('should return expected result for valid input', () => {
    const result = example('valid');
    expect(result).toBe('expected');
  });
  
  it('should handle empty input', () => {
    const result = example('');
    expect(result).toBe('default');
  });
  
  it('should throw for invalid input', () => {
    expect(() => example(null)).toThrow();
  });
});
```

### Step 4: Act - 实现功能代码 (Green Phase)

```typescript
// example.ts
export function example(input: string): string {
  if (input === null || input === undefined) {
    throw new Error('Invalid input');
  }
  if (input === '') {
    return 'default';
  }
  return 'expected';
}
```

### Step 5: Validate - 运行测试并重构 (Refactor Phase)

```yaml
validation:
  - run_tests: "npm test"
  - check_coverage: ">= 80%"
  - lint_check: "npm run lint"
  
on_failure:
  - analyze_error: "读取错误信息"
  - fix_code: "修正实现"
  - rerun_tests: "再次验证"
```

## TDD Cycle

```
┌─────────────────────────────────────────┐
│           TDD 循环                       │
│                                         │
│    ┌───────┐  编写测试  ┌───────┐       │
│    │ RED   │ ─────────→ │ GREEN │       │
│    │ 失败  │            │ 通过  │       │
│    └───┬───┘            └───┬───┘       │
│        │                    │           │
│        │    ┌───────────────┘           │
│        │    ↓                           │
│        │  ┌─────────┐                   │
│        └─→│REFACTOR │                   │
│           │  重构   │                   │
│           └─────────┘                   │
└─────────────────────────────────────────┘
```

## Output

- 测试文件：`__tests__/[name].test.ts`
- 实现文件：`[name].ts`
