---
description: 架构师技能 - 接口定义与数据建模
role: architect
skill: api_design
mcp_tools: [github]
---

# Skill: API Design / API 设计

设计 RESTful API 接口和数据模型。

## Steps

### Step 1: Observe - 分析功能需求

```yaml
inputs:
  - prd: "PRD 中的功能描述"
  - existing_apis: "现有 API 结构"
  
actions:
  - 提取数据实体
  - 识别操作类型 (CRUD)
  - 分析关联关系
```

### Step 2: Orient - 确定设计原则

```yaml
principles:
  - RESTful: "资源导向，动词统一"
  - Consistency: "与现有 API 风格一致"
  - Versioning: "API 版本控制"
  - Security: "认证与授权"
```

### Step 3: Decide - 设计接口

#### 资源定义

```yaml
resources:
  - name: User
    endpoints:
      - GET /api/users
      - GET /api/users/{id}
      - POST /api/users
      - PATCH /api/users/{id}
      - DELETE /api/users/{id}
```

#### 数据模型

```typescript
interface User {
  id: string;
  name: string;
  email: string;
  createdAt: Date;
  updatedAt: Date;
}
```

### Step 4: Act - 生成 OpenAPI 规范

```yaml
openapi: 3.0.0
info:
  title: API 名称
  version: 1.0.0
  
paths:
  /api/users:
    get:
      summary: 获取用户列表
      responses:
        '200':
          description: 成功
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/User'
                  
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
```

### Step 5: Validate - 校验 API 设计

- [ ] 遵循 RESTful 规范？
- [ ] 错误响应格式统一？
- [ ] 认证方式明确？
- [ ] 与现有 API 兼容？

## Output

生成文件：`API_Spec.yaml`
