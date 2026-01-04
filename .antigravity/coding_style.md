# Antigravity 编码规范

## Python
- 所有函数必须有类型注解 (`def foo(x: int) -> str:`)
- 使用 Google-style Docstrings
- 数据模型使用 Pydantic
- 异步函数优先使用 `async/await`

## TypeScript/JavaScript
- 使用 ESLint + Prettier
- 优先使用函数式组件 (React)
- 使用 TypeScript 严格模式

## 通用规则
- 变量/函数名：英文，camelCase
- 类名：英文，PascalCase
- 常量：大写下划线 (UPPER_SNAKE_CASE)
- 注释：中文
- 每个文件不超过 300 行
- 单个函数不超过 50 行

## 文档
- API 接口必须有 OpenAPI/Swagger 文档
- 复杂逻辑必须有内联注释
- 公共函数必须有 docstring
