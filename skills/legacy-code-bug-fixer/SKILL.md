---
name: legacy-code-bug-fixer
description: 修复遗留代码中的逻辑Bug。当用户报告代码逻辑错误、运行时异常、功能不符合预期时使用。关键词：修复、fix、bug、错误、崩溃、crash、异常、exception、旧代码、遗留代码。
---

# 遗留代码Bug修复

修复没有AI辅助时编写的旧代码中的逻辑Bug，提供系统性的分析和修复流程。

## 触发条件

- 用户说"修复"、"fix"、"bug"、"错误"
- 用户说"崩溃"、"crash"、"异常"、"exception"
- 用户说"旧代码"、"遗留代码"、"老代码"
- 用户报告代码逻辑错误
- 运行时异常或崩溃
- 功能行为不符合预期
- 需要理解和修复旧代码

## Core Principles

1. **Deep analysis first** - Not just simple nil checks, but understanding code intent and logic
2. **Solution first** - Provide multiple fix solutions first, execute after user confirmation
3. **Impact assessment** - Assess fix impact on existing logic
4. **Verification loop** - Verify effect after fix

---

## Phase 1: Problem Collection

### 1.1 Collect Error Information

```
Information to collect:
- Error log/stack trace
- Context when error occurred
- Trigger conditions and reproduction steps
- Expected behavior vs actual behavior
```

### 1.2 Locate Problem Code

1. Extract key information from error message (file, line number, function name)
2. Use Grep to search related code
3. Use Read to view complete context

---

## Phase 2: Deep Analysis

### 2.1 Call Chain Analysis

```
Analysis steps:
1. Find error occurrence point
2. Trace call chain upward (who called this function)
3. Trace data flow downward (where data comes from)
4. Draw call relationship diagram
```

### 2.2 Code Intent Understanding

- Read function comments and documentation
- Analyze function naming and parameters
- Check related test cases
- Compare similar feature implementations

### 2.3 Logic Defect Identification

**Common logic defect types:**

| Type | Description | Check Method |
|------|------|----------|
| Null value issues | nil/null/undefined not handled | Check all variable sources |
| Timing issues | Async callback timing wrong | Check initialization order |
| Boundary issues | Array out of bounds, loop conditions | Check boundary conditions |
| State issues | State machine transition errors | Check state flow |
| Cache issues | Sync callback when cache hits | Check cache logic |
| Type issues | Type conversion/comparison errors | Check type consistency |

### 2.4 Special Case Handling

**Missing methods:**
- Search original project (e.g., Unity original project) for implementation
- Analyze method purpose and expected behavior
- Decide whether to implement or remove call

**Async issues:**
- Check variables that callback function depends on
- Confirm variables are initialized before callback
- Consider sync callback scenario when cache hits

---

## Phase 3: Solution Design

### 3.1 Provide Multiple Solutions

**Must provide at least 2-3 solutions:**

```markdown
## 修复方案

### 方案 A: [方案名称]
**描述**: [具体做法]
**优点**: 
- ...
**缺点**: 
- ...
**影响范围**: [会影响哪些功能]

### 方案 B: [方案名称]
**描述**: [具体做法]
**优点**: 
- ...
**缺点**: 
- ...
**影响范围**: [会影响哪些功能]

### 方案 C: [方案名称]（如有）
...

### 推荐方案: [A/B/C]
**理由**: [为什么推荐这个方案]
```

### 3.2 Wait for User Confirmation

**Important: Do not execute any code modifications before user explicitly chooses a solution!**

---

## Phase 4: Fix Implementation

### 4.1 Implement Fix

1. Modify code according to user's chosen solution
2. Keep code style consistent
3. Add necessary comments explaining fix reason

### 4.2 Verify Fix

- Check if original problem is solved
- Check if new problems are introduced
- Run related tests (if any)

---

## Output Format

### Analysis Report

```markdown
## Bug分析报告

### 1. 问题描述
[错误现象和触发条件]

### 2. 根本原因
[为什么会出现这个错误]

### 3. 调用链
```
函数A (文件:行号)
  └─> 函数B (文件:行号)
      └─> 函数C (文件:行号) ← 错误发生点
```

### 4. 修复方案
[方案A/B/C详情]

### 5. 推荐方案
[推荐哪个方案及理由]

请选择方案后回复，我将执行修复。
```

### Fix Completion Report

```markdown
## 修复完成

### 修改文件
- `path/to/file.lua`: [修改描述]

### 修复内容
[具体修改了什么]

### 验证结果
[修复是否有效]

### 注意事项
[后续需要注意的问题]
```

---

## Checklist

During analysis, ensure the following items are checked:

- [ ] Error information fully collected
- [ ] Call chain fully traced
- [ ] Original code intent understood
- [ ] Logic defects identified
- [ ] Multiple fix solutions prepared
- [ ] Solution pros and cons listed
- [ ] Impact scope assessed
- [ ] User confirmed solution
- [ ] Fix implemented
- [ ] Fix effect verified
