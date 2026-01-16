深度探索代码库，分析代码边界和调用关系。

**目标:** $ARGUMENTS

## 参数格式

`/explore [模式] [目标...]`

- 第一个参数: 探索模式（可选）
  - `call` 或 `chain` - 调用链分析
  - `boundary` 或 `边界` - 边界分析
  - `dep` 或 `dependency` - 依赖分析
  - `impact` 或 `影响` - 影响分析
  - 不指定则自动判断
- 剩余参数: 探索目标（函数名、类名、文件路径等）

**示例:**
- `/explore call getUserInfo` - 分析 getUserInfo 的调用链
- `/explore boundary src/auth/` - 分析 auth 模块的边界
- `/explore dep LoginService` - 分析 LoginService 的依赖
- `/explore impact handlePayment` - 分析修改 handlePayment 的影响
- `/explore 这个函数是怎么被调用的` - 自动判断模式

## Instructions

启动 `code-explorer` 子 agent 进行深度探索：

```
Task:
  subagent_type: code-explorer
  description: 深度代码探索
  prompt: |
    探索目标: [目标描述]
    探索模式: [如有指定]
    
    请深度分析代码，找出调用关系、边界和依赖。
```

该 agent 使用 `claude-opus-4-5-think` 模型进行深度思考分析。

---

现在开始探索: $ARGUMENTS
