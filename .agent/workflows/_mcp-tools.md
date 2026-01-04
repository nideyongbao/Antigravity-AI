---
description: MCP工具配置参考 - 从 JSON 文件动态加载配置
role: framework
config_source: "../mcp_config/mcp_config.json"
---

# MCP Tools Configuration

> [!IMPORTANT]
> **配置源**: 所有 MCP 工具配置从 `mcp_config/mcp_config.json` 文件读取。
> 修改配置请编辑 JSON 文件，本文档仅作为参考说明。

## 配置加载协议

```yaml
load_config:
  source: "file://mcp_config/mcp_config.json"
  on_init:
    1: "读取 mcpServers 配置"
    2: "根据当前角色加载 roleAuthorization"
    3: "应用 securityControls 规则"
  
  on_tool_call:
    1: "检查角色是否有权限调用该工具"
    2: "检查工具是否需要用户审批"
    3: "执行工具调用"
```

---

## Available MCP Servers / 当前可用服务

### 1. GitHub MCP (`github`)

**授权角色**: 项目主管, 架构师, 开发工程师

| 工具 | 说明 | 需审批 |
|------|------|--------|
| `get_file_contents` | 获取文件内容 | ❌ |
| `search_code` | 搜索代码 | ❌ |
| `list_issues` | 列出 Issues | ❌ |
| `create_issue` | 创建 Issue | ✅ |
| `push_files` | 推送文件 | ✅ |
| `create_pull_request` | 创建 PR | ✅ |
| `create_branch` | 创建分支 | ✅ |

### 2. Puppeteer MCP (`puppeteer`)

**授权角色**: 测试工程师

| 工具 | 说明 | 需审批 |
|------|------|--------|
| `puppeteer_navigate` | 导航到URL | ❌ |
| `puppeteer_screenshot` | 截取屏幕 | ❌ |
| `puppeteer_click` | 点击元素 | ❌ |
| `puppeteer_fill` | 填写表单 | ❌ |
| `puppeteer_evaluate` | 执行JS | ❌ |

### 3. RedNote MCP (`rednote-MCP`)

**授权角色**: 产品经理

| 工具 | 说明 | 需审批 |
|------|------|--------|
| `search_notes` | 搜索笔记 | ❌ |
| `get_note_content` | 获取笔记内容 | ❌ |
| `get_note_comments` | 获取评论 | ❌ |

### 4. Fabric MCP (`fabric-mcp-server`)

**授权角色**: 所有角色

| 工具 | 说明 | 需审批 |
|------|------|--------|
| `recommend_tool` | 推荐分析模式 | ❌ |

### 5. Tavily MCP (`tavily`)

**授权角色**: 产品经理, 架构师

| 工具 | 说明 | 需审批 |
|------|------|--------|
| `search` | Web 搜索 | ❌ |
| `extract` | 内容提取 | ❌ |

### 6. Filesystem MCP (`filesystem`)

**授权角色**: 主编排器 (Orchestrator)

| 工具 | 说明 | 需审批 |
|------|------|--------|
| `read_file` | 读取文件 | ❌ |
| `write_file` | 写入文件 | ✅ |
| `create_directory` | 创建目录 | ❌ |
| `list_directory` | 列出目录 | ❌ |

> [!NOTE]
> Filesystem 仅限于 `.antigravity-output/` 目录，用于制品管理。

### 7. Memory MCP (`memory`)

**授权角色**: 项目主管, 测试工程师, 主编排器

| 工具 | 说明 | 需审批 |
|------|------|--------|
| `create_entities` | 创建知识实体 | ❌ |
| `create_relations` | 创建关联关系 | ❌ |
| `search_nodes` | 搜索知识图谱 | ❌ |
| `read_graph` | 读取图谱 | ❌ |

### 8. Slack MCP (`slack`)

**授权角色**: 项目主管, 主编排器

| 工具 | 说明 | 需审批 |
|------|------|--------|
| `slack_post_message` | 发送消息 | ✅ |
| `slack_list_channels` | 列出频道 | ❌ |
| `slack_get_channel_history` | 获取历史消息 | ❌ |

---

## Role-Tool Matrix / 角色工具矩阵

> 配置来源: `mcp_config.json` → `roleAuthorization`

| 角色 | github | puppeteer | rednote | fabric | tavily | filesystem | memory | slack |
|------|--------|-----------|---------|--------|--------|------------|--------|-------|
| 主编排器 | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ✅ |
| 项目主管 | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ✅ | ✅ |
| 产品经理 | ❌ | ❌ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| 架构师 | ✅ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ |
| 开发工程师 | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ |
| 测试工程师 | ❌ | ✅ | ❌ | ✅ | ❌ | ❌ | ✅ | ❌ |

---

## Security Controls / 安全控制

> 配置来源: `mcp_config.json` → `securityControls`

### 需要用户审批的操作

```json
[
  "github.push_files",
  "github.create_pull_request",
  "github.create_issue",
  "github.create_branch"
]
```

### 自动审批的操作

所有读取类操作（get、search、list）和低风险操作自动审批。

---

## Dynamic Discovery / 动态发现

> 配置来源: `mcp_config.json` → `dynamicDiscovery`

```yaml
strategy:
  handshake_only: true  # 初始化时仅获取工具名称
  on_demand_schema: true  # 使用时请求详细 Schema
  tool_search: true  # 支持语义搜索工具
```

---

## Usage in Workflows / 工作流中使用

### 声明所需工具

在角色/技能工作流的 frontmatter 中声明：

```yaml
---
description: 工作流描述
role: developer
mcp_tools: [github, fabric-mcp-server]
---
```

### 调用前验证

```yaml
before_invoke:
  - check: "role in mcp_config.roleAuthorization[tool].allowed"
  - check: "tool_method in mcp_config.roleAuthorization[role].tools[server]"
  - if_require_approval: "notify_user for confirmation"
```

---

> [!TIP]
> 配置新的 MCP Server 时，编辑 `mcp_config/mcp_config.json`，添加到 `mcpServers` 和 `roleAuthorization`。
