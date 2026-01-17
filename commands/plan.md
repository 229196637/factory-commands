为以下功能/任务创建详细的实现计划。

**功能:** $ARGUMENTS

## 说明

使用中文输出结果，但内部思考和分析过程使用英文。

## 参数格式

`/plan [计划ID] [内容...]`

- 第一个参数: 计划ID (如 `P001`，可选，不提供则自动生成)
- 剩余参数: 具体内容

**示例:**
- `/plan 实现用户登录功能` - 自动生成ID
- `/plan P005 实现用户登录功能` - 指定ID为P005
- `/plan 添加缓存功能` - 自动生成ID

## 参数解析

**解析 `$ARGUMENTS`：**

1. 检查第一个词是否匹配 `P\d+` 格式（计划ID）
2. 剩余部分为具体内容

## Instructions

启动 `standard-planner` 子 agent 进行计划：

```
Task:
  subagent_type: standard-planner
  description: 创建标准计划
  prompt: |
    用户需求: [具体内容]
    指定计划ID: [如有]
    
    请分析并创建实现计划。
```

---

现在解析参数并创建计划: $ARGUMENTS
