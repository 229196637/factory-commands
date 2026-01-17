启动深度代码探索，分析调用链、变量关系和依赖。

**目标:** $ARGUMENTS

## 说明

使用中文输出结果。

## Instructions

启动 `code-explorer` 子 agent 进行深度探索分析：

```
Task:
  subagent_type: code-explorer
  description: 深度代码探索
  prompt: |
    探索目标: $ARGUMENTS
    
    请深度分析代码，重点关注：
    1. 调用链 - 函数/方法的调用关系
    2. 变量关系 - 变量的定义、赋值、传递
    3. 依赖关系 - 模块间的依赖
    4. 边界分析 - 模块的公开接口和交互点
    
    【重要】输出要求：
    - 简洁明了，只击重点
    - 不要冗长描述，用列表和简短语句
    - 关键发现用一句话总结
    - 省略无关细节
```

该 agent 使用 `claude-opus-4-5-think` 深度思考模型。

---

现在开始探索: $ARGUMENTS
