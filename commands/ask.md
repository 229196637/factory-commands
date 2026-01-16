启动 Ask 研究代理，深度分析代码库回答问题。

**用户问题:** $ARGUMENTS

## Instructions

使用 `Task` 工具调用 `ask-researcher` droid 来处理用户的咨询问题：

```
Task:
  subagent_type: ask-researcher
  description: 代码库研究咨询
  prompt: |
    用户问题: $ARGUMENTS
    
    请深度分析代码库，回答上述问题。
    - 使用 Grep/Glob 定位相关代码
    - 使用 Read 仔细阅读代码逻辑
    - 提供有代码依据的答案
    - 如果用户要求修改代码，礼貌拒绝并说明这是只读研究模式
```

执行完成后，将 agent 的研究结果简洁地呈现给用户。

如果用户要求执行修改，回复：
> ⚠️ 当前处于咨询模式 (`/ask`)，不允许修改文件。如需执行修改，请退出咨询模式后直接提出修改请求。
