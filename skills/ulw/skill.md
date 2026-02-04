---
name: ulw
description: 深度思考探索技能。当用户需要深度分析代码、理解复杂逻辑、追踪调用链时使用。关键词：深度分析、深度思考、复杂逻辑、调用链、依赖分析、影响范围、ulw。
---

# ULW - 深度思考探索技能

## 概述

ULW (Ultra-Level Wisdom) 是一个深度思考技能，用于启动 `code-explorer` 代理进行复杂代码分析。

## 触发条件

- 用户说"深度分析"、"深度思考"、"复杂逻辑"
- 用户说"调用链"、"依赖分析"、"影响范围"
- 用户说"ulw"、"深度探索"、"代码边界"
- 用户需要深度理解代码逻辑
- 需要追踪复杂调用链
- 需要分析代码边界和依赖关系
- 需要评估代码修改的影响范围

## Instructions

When this skill is triggered, launch the `code-explorer` subagent:

```
Task:
  subagent_type: code-explorer
  description: Deep code exploration
  prompt: |
    User requirement: [User requirement from current context]
    
    Please use deep thinking mode to analyze code:
    1. Understand user's exploration goal
    2. Use Grep, Read, Glob tools to search related code
    3. Trace call chains and dependencies
    4. Analyze code boundaries and impact scope
    5. Output detailed analysis report
```

## Exploration Modes

The `code-explorer` agent supports the following exploration modes:

| Mode | Description |
|------|------|
| call-chain | Trace function call chains |
| boundary | Analyze module boundaries |
| dependency | Analyze dependency relationships |
| impact | Analyze modification impact |

## Output Format

After exploration is complete, the agent will output:

```markdown
# 探索报告: [目标描述]

## 探索范围
- 起始点: [文件/函数/类]
- 探索模式: [模式]

## 发现

### 调用关系
[调用图]

### 关键边界
[边界点列表]

### 依赖项
[依赖列表]

## 风险与建议
[分析结果]

## 相关文件
[文件列表]
```

## Important Notes

1. This skill uses `claude-opus-4-5-think` deep thinking model
2. Suitable for complex code analysis, not needed for simple queries
3. Analysis process may take longer time
