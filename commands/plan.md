为以下功能/任务创建详细的实现计划。

**功能:** $ARGUMENTS

## 说明

使用中文输出结果，但内部思考和分析过程使用英文。

## 参数格式

`/plan [模式] [计划ID] [内容...]`

- 第一个参数: 模式 (`ulw` 深度思考 或 普通模式)
- 第二个参数: 计划ID (如 `P001`，可选，不提供则自动生成)
- 第三个参数起: 具体内容

**示例:**
- `/plan ulw 实现用户登录功能` - 深度思考模式，自动生成ID
- `/plan ulw P005 实现用户登录功能` - 深度思考模式，指定ID为P005
- `/plan 添加缓存功能` - 普通模式，自动生成ID
- `/plan P003 添加缓存功能` - 普通模式，指定ID为P003

## 模式检测

**解析 `$ARGUMENTS`：**

1. 检查第一个词是否为 `ulw`（不区分大小写）
2. 检查第二个词是否匹配 `P\d+` 格式（计划ID）
3. 剩余部分为具体内容

### 如果以 `ulw` 开头 → 深度思考模式

启动 `deep-thinking-planner` 子 agent 进行深度思考：

```
Task:
  subagent_type: deep-thinking-planner
  description: 深度思考生成计划
  prompt: |
    用户需求: [具体内容]
    指定计划ID: [如有]
    
    请深度分析并创建实现计划。
```

该 agent 使用 `claude-opus-4-5-think` 模型，会自动处理计划生成流程。

### 如果不以 `ulw` 开头 → 普通模式

启动 `standard-planner` 子 agent 进行普通计划：

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
