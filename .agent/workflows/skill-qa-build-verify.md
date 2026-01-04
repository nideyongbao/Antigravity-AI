---
description: 测试工程师技能 - 真实构建验证（npm/docker/pip等）
role: qa_engineer
skill: build_verify
---

# Skill: Build Verification / 构建验证

在执行 E2E 测试前，验证代码可以成功构建。

## Steps

### Step 1: Observe - 检测项目类型

```yaml
project_detection:
  indicators:
    - file: "package.json"
      type: "nodejs"
    - file: "docker-compose.yml"
      type: "docker-compose"
    - file: "Dockerfile"
      type: "docker"
    - file: "requirements.txt"
      type: "python-pip"
    - file: "pyproject.toml"
      type: "python-poetry"
    - file: "pom.xml"
      type: "maven"
    - file: "build.gradle"
      type: "gradle"
```

### Step 2: Orient - 选择构建策略

| 项目类型 | 主构建命令 | 备用命令 |
|---------|-----------|---------|
| nodejs | `npm install && npm run build` | `npm run lint` |
| docker-compose | `docker compose build --no-cache` | `docker compose config` |
| docker | `docker build -t test-build .` | - |
| python-pip | `pip install -r requirements.txt` | `python -m py_compile *.py` |
| python-poetry | `poetry install` | `poetry check` |

### Step 3: Decide - 规划验证流程

```yaml
verification_plan:
  1: "检测项目根目录中的构建配置文件"
  2: "确定项目类型和对应的构建命令"
  3: "执行构建命令"
  4: "分析构建输出"
  5: "记录构建结果"
```

### Step 4: Act - 执行构建命令

```yaml
# // turbo
build_execution:
  nodejs:
    commands:
      - "npm ci || npm install"
      - "npm run build --if-present"
      - "npm run lint --if-present"
    timeout: 300s
    
  docker_compose:
    commands:
      - "docker compose config"
      - "docker compose build --no-cache"
    timeout: 600s
    
  python:
    commands:
      - "pip install -r requirements.txt"
      - "python -m compileall . -q"
    timeout: 180s
```

### Step 5: Validate - 分析构建结果

```yaml
validation:
  success_indicators:
    - exit_code: 0
    - no_error_keywords: ["error", "failed", "cannot find"]
    
  on_success:
    status: "BUILD_PASSED"
    next: "PLAN_TESTS"
    
  on_failure:
    status: "BUILD_FAILED"
    next: "REPORT_BUILD_FAILURE"
    actions:
      - "提取错误信息"
      - "生成 Build_Error_Report.md"
      - "触发开发工程师返工"
```

## Build Error Report Format

```markdown
# Build Error Report

## Summary
- **Project Type**: nodejs
- **Build Command**: npm run build
- **Status**: FAILED
- **Exit Code**: 1

## Error Details

### Error Message
```
TypeScript error TS2304: Cannot find name 'foo'.
```

### Affected Files
- `src/components/Example.tsx:15:10`

## Suggested Fix
检查是否缺少类型定义或拼写错误

## Rework Required
- [ ] 开发工程师修复编译错误
- [ ] 重新提交代码
```

## Output

- 构建状态：`PASSED` | `FAILED`
- 错误报告：`Build_Error_Report.md`（仅失败时）
