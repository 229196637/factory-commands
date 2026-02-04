---
name: lua-deep-reader
description: 深度阅读Lua代码，追踪变量、函数的依赖关系和调用链。当遇到底层API（base._xxx）或服务器交互协议时自动停止。关键词：阅读代码、分析代码、理解代码、调用链、依赖关系、深度阅读、Lua。
---

# Lua深度代码阅读

深度阅读Lua代码，理解代码、变量和函数的依赖关系。遇到无法阅读的底层API或服务器交互点时停止。

## 触发条件

- 用户说"阅读代码"、"分析代码"、"理解代码"
- 用户说"调用链"、"依赖关系"、"谁调用了"
- 用户说"这个函数做什么"、"这个变量从哪来"
- 用户说"深度阅读"、"代码分析"、"Lua代码"
- 用户需要追踪函数调用关系
- 用户需要理解代码逻辑和架构

---

## Phase 1: Entry Point Location

### 1.1 Determine Starting Point

Determine reading starting point based on user input:

| Input Type | Processing Method |
|---------|---------|
| File path | Read the file directly |
| Function name | Use Grep to search function definition |
| Class name | Search `class("ClassName"` or `ClassName = class` |
| Error stack | Start tracing from stack top |
| Log output | Search function name/file name in log |

### 1.2 Search Strategy

```lua
-- Search function definition
Grep: "function ClassName.FunctionName" or "function ClassName:FunctionName"

-- Search class definition
Grep: "class(\"ClassName\"" or "ClassName = class"

-- Search variable usage
Grep: "self.m_VariableName" or "g_VariableName"
```

---

## Phase 2: Recursive Tracing

### 2.1 Trace Upward (Who calls this function)

1. Use Grep to search where function is called
2. Record caller's file and line number
3. Continue tracing caller's caller (recursive)

```
Search patterns:
- ClassName:FunctionName(
- ClassName.FunctionName(
- self:FunctionName(
- g_CtrlName:FunctionName(
```

### 2.2 Trace Downward (What this function calls)

1. Read function body content
2. Identify all function calls
3. Recursively trace each call

### 2.3 Data Flow Tracing

Trace variable sources and destinations:

```
Variable sources:
- Function parameters
- self.m_xxx member variables
- g_xxx global variables
- local variables
- Function return values
```

### 2.4 Record Dependencies

Use tree structure to record:

```
FunctionA (file:line)
  ├─> FunctionB (file:line)
  │     ├─> FunctionC (file:line)
  │     └─> [Boundary] base._xxx
  └─> FunctionD (file:line)
        └─> [Boundary] GS2CXxx
```

---

## Phase 3: Boundary Identification (Stop Conditions)

### 3.1 Stop Boundary Definitions

| Boundary Type | Recognition Pattern | Description |
|---------|---------|------|
| Spark Low-level API | `base._xxx` | Spark Editor internal implementation, cannot trace |
| Engine API | `UnityEngine.*` | Unity/Spark engine wrapper |
| Server Protocol | `GS2C*`, `C2GS*` | Client-server boundary |
| Type System | `classtype.*` | Low-level type definitions |
| Global Controllers | `g_*Ctrl` | Stop if already analyzed |

### 3.2 Boundary Handling

When encountering boundary:
1. Mark as boundary node
2. Record boundary type
3. Stop tracing this branch
4. Continue other branches

### 3.3 Common Boundary API Reference

**Spark Low-level API (base._xxx)**:
- `base._unit_set_attr` - Set unit attribute
- `base._unit_get_attr` - Get unit attribute
- `base._create_unit` - Create unit
- `base._destroy_unit` - Destroy unit
- `base.gui_new` - Create UI
- `base.gui_check` - Check UI

**Server Protocols**:
- `GS2C*` - Server to client messages
- `C2GS*` - Client to server messages
- `netwar.*` - Battle network protocol
- `netlogin.*` - Login network protocol

**Engine API**:
- `UnityEngine.GameObject.*`
- `UnityEngine.Transform.*`
- `classtype.MeshRender`
- `classtype.SceTransform`

---

## Phase 4: Output Report

### 4.1 Report Template

```markdown
## 代码阅读报告

### 1. 入口点
- **文件**: [文件路径]
- **函数**: [函数名]
- **目的**: [用户的分析目标]

### 2. 调用关系图

```
[入口函数] (file:line)
  ├─> [被调用函数1] (file:line)
  │     └─> [边界API] ← 停止
  └─> [被调用函数2] (file:line)
        └─> [边界API] ← 停止
```

### 3. 数据流向

| 变量 | 来源 | 处理 | 使用 |
|-----|------|------|------|
| xxx | 参数/成员 | 函数处理 | 传递给xxx |

### 4. 边界API列表

| API | 类型 | 说明 |
|-----|------|------|
| base._xxx | 星火底层 | [功能说明] |
| GS2CXxx | 服务器协议 | [功能说明] |

### 5. 关键发现

- [重要逻辑说明]
- [潜在问题]
- [架构特点]
```

---

## Tracing Depth Control

### Default Depth
- Downward tracing: Maximum 5 levels
- Upward tracing: Maximum 3 levels

### Depth Adjustment
User can specify:
- "Deep read to the end" - Trace to all boundaries
- "Only one level" - Only trace direct calls
- "Trace N levels" - Specify depth

---

## Common Tracing Patterns

### Pattern 1: Feature Understanding
```
Goal: Understand implementation of a feature
Starting point: Feature entry function
Direction: Trace downward
Output: Call relationship diagram + key logic explanation
```

### Pattern 2: Bug Location
```
Goal: Locate problem from error stack
Starting point: Error occurrence point
Direction: Trace callers upward + trace data sources downward
Output: Call chain + data flow + problem point
```

### Pattern 3: Architecture Analysis
```
Goal: Understand inter-module relationships
Starting point: Module entry/controller
Direction: Breadth-first tracing
Output: Module dependency diagram + interface description
```

---

## Coordination with Other Skills

```
Code understanding → [lua-deep-reader] Deep reading
    ↓
Problem found → [legacy-code-bug-fixer] Fix Bug
    ↓
Need debugging → [debug-info-adder] Add debug info
    ↓
Check API → [sce-api-reference] Check documentation
```

---

## Important Notes

1. **Avoid circular tracing**: Record traced functions, avoid repetition
2. **Control output volume**: For large call chains, only show key paths
3. **Mark uncertainty**: Clearly mark speculated logic
4. **Stay focused**: Focus on user's analysis goal, don't diverge
5. **Stop timely**: Stop immediately at boundaries, don't try to guess low-level implementation
