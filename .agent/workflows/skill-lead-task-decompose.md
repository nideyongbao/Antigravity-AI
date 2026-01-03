---
description: 项目主管技能 - 将模糊需求拆解为可执行任务清单
role: project_lead
skill: task_decompose
---

# Skill: Task Decomposition / 任务拆解

将用户的模糊需求拆解为结构化的可执行任务清单。

## Steps

### Step 1: Observe - 收集需求信息

读取用户输入，识别核心意图：

```yaml
inputs:
  - user_prompt: "用户的原始需求描述"
  - project_context: "项目背景信息（如有）"
  
actions:
  - 提取关键词和核心功能点
  - 识别隐含需求和假设
  - 标记模糊或歧义之处
```

### Step 2: Orient - 评估复杂度

分析需求规模和难度：

| 评估项 | 低 | 中 | 高 |
|--------|-----|-----|-----|
| 功能数量 | 1-2 | 3-5 | 5+ |
| 技术难度 | 现有技术 | 需调研 | 需创新 |
| 依赖复杂度 | 无外部依赖 | 少量 API | 多系统集成 |
| 时间压力 | 充裕 | 正常 | 紧急 |

### Step 3: Decide - 规划任务结构

按照标准阶段拆解：

```yaml
phases:
  1_requirement:
    owner: product_manager
    tasks:
      - "需求分析与澄清"
      - "PRD 文档编写"
      - "PRD 验证与审批"
      
  2_design:
    owner: architect
    tasks:
      - "技术可行性评估"
      - "架构方案设计"
      - "API 接口定义"
      
  3_implementation:
    owner: developer
    tasks:
      - "环境准备与依赖安装"
      - "核心功能开发"
      - "单元测试编写"
      
  4_testing:
    owner: qa_engineer
    tasks:
      - "测试用例设计"
      - "E2E 自动化测试"
      - "验收演示准备"
```

### Step 4: Act - 生成任务清单

输出 JSON 格式的任务清单：

```json
{
  "project": {
    "name": "项目名称",
    "description": "一句话描述",
    "priority": "P0",
    "estimated_days": 5
  },
  "milestones": [
    {
      "id": "M1",
      "name": "需求确认",
      "due_offset_days": 1
    },
    {
      "id": "M2",
      "name": "设计完成",
      "due_offset_days": 2
    },
    {
      "id": "M3",
      "name": "开发完成",
      "due_offset_days": 4
    },
    {
      "id": "M4",
      "name": "测试通过",
      "due_offset_days": 5
    }
  ],
  "tasks": [
    {
      "id": "T001",
      "title": "需求分析",
      "owner": "product_manager",
      "priority": "P0",
      "milestone": "M1",
      "dependencies": [],
      "description": "分析用户需求，生成 PRD"
    }
  ]
}
```

### Step 5: Validate - 校验完整性

检查任务清单：

- [ ] 所有需求点都有对应任务？
- [ ] 任务依赖关系正确？
- [ ] 每个任务都有明确的负责人？
- [ ] 里程碑时间线合理？

## Output

生成文件：`Task_List.json`

---

> [!TIP]
> 如需求过于模糊（模糊度 > 30%），优先触发澄清流程，向用户反问确认。
