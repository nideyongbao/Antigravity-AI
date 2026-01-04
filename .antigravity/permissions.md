# Antigravity 权限白名单

## 可信目录（自动创建/修改）
```yaml
auto_allowed:
  - .antigravity-output/**
  - artifacts/**
  - tests/**
  - docs/**
```

## 需确认目录（修改需审批）
```yaml
require_approval:
  - src/**
  - config/**
  - .github/**
  - package.json
  - pyproject.toml
  - docker-compose.yml
  - Dockerfile
```

## 禁止访问目录
```yaml
forbidden:
  - .git/objects/**
  - .git/refs/**
  - node_modules/**  # 仅读取
  - __pycache__/**
```

## MCP 工具权限
| 工具 | 自动执行 | 需审批 |
|------|:--------:|:------:|
| github.search_code | ✅ | |
| github.push_files | | ✅ |
| puppeteer.screenshot | ✅ | |
| filesystem.write_file | ✅ (白名单内) | ✅ (其他) |
| memory.* | ✅ | |
