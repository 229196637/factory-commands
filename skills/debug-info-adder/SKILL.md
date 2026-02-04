---
name: debug-info-adder
description: 添加调试信息来定位问题。当遇到陌生问题、无法直接从逻辑推断、需要打印变量值或追踪调用链时使用。关键词：调试、debug、打印、print、日志、log、追踪、trace。
---

# 调试信息添加指南

当遇到陌生问题、无法直接从逻辑推断时，系统性地添加调试信息来定位问题。

## 触发条件

- 用户说"调试"、"debug"、"打印"、"print"
- 用户说"加日志"、"加log"、"追踪"、"trace"
- 无法直接从逻辑推断问题原因
- 变量值不明确，需要打印确认
- 调用链不清楚，需要堆栈跟踪
- 时序问题，需要时间戳追踪
- 异步回调中状态不确定

---

## Debug Information Addition Strategies

### Strategy A: Single Point Debugging (Simple Problems)

Applicable for: Unclear variable values, problems within a single function

```lua
--DEBUG: Print variable value
print("[ModuleName] varName:", varValue)

--DEBUG: Print multiple variables
print("[ModuleName] state:", var1, var2, var3)

--DEBUG: Print table contents
print("[ModuleName] table contents:", table.tostring(myTable))
```

### Strategy B: Call Chain Tracing (Complex Problems)

Applicable for: Unclear who calls the function, call order issues

```lua
--DEBUG: Function entry + stack
print("[ModuleName] Function entry, params:", param1, param2)
print(debug.traceback())

--DEBUG: Simplified stack (only print key info)
print("[ModuleName] Call source:", debug.getinfo(2, "Sl").source, debug.getinfo(2, "Sl").currentline)
```

### Strategy C: Timing Tracing (Async/Timing Problems)

Applicable for: Async callbacks, frame updates, event order issues

```lua
--DEBUG: Log with timestamp
print(string.format("[%s][Frame%d][ModuleName] Event: %s", 
    os.date("%H:%M:%S"), 
    UnityEngine.Time.frameCount or 0, 
    description))

--DEBUG: Async callback tracing
print("[ModuleName] Callback triggered, current state:", self.m_State)
```

### Strategy D: Conditional Breakpoint (Specific Condition Problems)

Applicable for: Problems that only occur under specific conditions

```lua
--DEBUG: Conditional print
if specificCondition then
    print("[ModuleName] Condition triggered:", conditionValue)
    print(debug.traceback())
end

--DEBUG: Counter
self._debugCount = (self._debugCount or 0) + 1
if self._debugCount % 100 == 0 then
    print("[ModuleName] Call count:", self._debugCount)
end
```

---

## Debug Point Selection Principles

### 1. Entry Points
Print parameters at function entry to confirm input is correct:
```lua
function MyClass.DoSomething(self, param1, param2)
    --DEBUG: Entry point
    print("[MyClass.DoSomething] Entry, param1:", param1, "param2:", param2)
    -- Original code...
end
```

### 2. Branch Points
Print condition values at if/else branches to confirm which branch is taken:
```lua
--DEBUG: Branch point
print("[ModuleName] Condition check, condition:", condition, "type:", type(condition))
if condition then
    print("[ModuleName] Entering true branch")
else
    print("[ModuleName] Entering false branch")
end
```

### 3. Call Points
Print before and after key function calls to confirm call success:
```lua
--DEBUG: Before call
print("[ModuleName] About to call SomeFunction, param:", arg)
local result = SomeFunction(arg)
--DEBUG: After call
print("[ModuleName] SomeFunction returned:", result, "type:", type(result))
```

### 4. Exception Points
Print state at potentially error-prone locations:
```lua
--DEBUG: nil check point
if not obj then
    print("[ModuleName] obj is nil!")
    print(debug.traceback())
    return
end
print("[ModuleName] obj exists, type:", type(obj))
```

---

## Debug Information Format Standards

### Standard Format
```lua
-- Normal info
print("[ModuleName] Description:", value)

-- Warning info
print("[ModuleName][WARN] Description:", value)

-- Error info
printerror("[ModuleName] Error description:", errorInfo)
```

### Module Naming
- Use class name or file name: `[CResCtrl]`, `[Camera]`, `[SpringPanel]`
- Including function name is clearer: `[Camera.SetPosition]`, `[CResCtrl.Load]`

### Value Formatting
```lua
-- Keep decimal places for numbers
print(string.format("[ModuleName] Position: (%.2f, %.2f, %.2f)", x, y, z))

-- Explicitly show boolean values
print("[ModuleName] Is enabled:", enabled and "true" or "false")

-- Explicitly mark nil values
print("[ModuleName] Object:", obj or "nil")
```

---

## Debug Workflow

### Step 1: Determine Problem Scope
1. Read error log, determine error location
2. Analyze call stack, understand call chain
3. Determine scope for adding debug info

### Step 2: Add Debug Information
1. Add print statements marked with `--DEBUG:` at key locations
2. Choose appropriate strategy (single point/call chain/timing)
3. Ensure printed info is sufficient to locate problem

### Step 3: Run and Analyze
1. Run program to reproduce problem
2. View log output
3. Narrow down problem scope based on output

### Step 4: Iterative Debugging
1. If info is insufficient, add more debug points
2. If scope is too large, narrow debug scope
3. Repeat until problem is located

### Step 5: Clean Up Debug Code
1. After problem is solved, delete code marked with `--DEBUG:`
2. Keep valuable log points (remove DEBUG marker)
3. Ensure normal functionality is not affected

---

## Common Scenario Debug Templates

### Scenario 1: nil Value Error
```lua
-- Add before error location
--DEBUG: Trace nil source
print("[ModuleName] Checkpoint1, obj:", obj or "nil")
print("[ModuleName] Checkpoint1, obj.field:", obj and obj.field or "nil")
```

### Scenario 2: Function Not Called
```lua
-- Add at function entry
function MyFunc()
    --DEBUG: Confirm function is called
    print("[ModuleName] MyFunc is called")
    print(debug.traceback())
end
```

### Scenario 3: Value Not As Expected
```lua
-- Add before and after assignment
--DEBUG: Trace value change
print("[ModuleName] Before assignment, value:", value)
value = SomeCalculation()
print("[ModuleName] After assignment, value:", value)
```

### Scenario 4: Async Callback Problem
```lua
-- Add at callback registration and trigger
--DEBUG: Callback registration
print("[ModuleName] Register callback, frame:", UnityEngine.Time.frameCount)

-- Inside callback function
--DEBUG: Callback triggered
print("[ModuleName] Callback triggered, frame:", UnityEngine.Time.frameCount)
print("[ModuleName] State at callback:", self.m_State)
```

---

## Important Notes

1. **Mark debug code**: All debug code marked with `--DEBUG:` comment for easy cleanup
2. **Avoid performance impact**: Don't add lots of prints in high-frequency calls
3. **Protect sensitive info**: Don't print passwords, keys, or other sensitive data
4. **Clean up timely**: Delete debug code promptly after problem is solved
5. **Keep valuable logs**: For key flows, keep logs but remove DEBUG marker

---

## Coordination with Other Skills

```
Problem found → [debug-info-adder] Add debug info
    ↓
Run program → [spark-editor-log-analyzer] Analyze logs
    ↓
Locate problem → [legacy-code-bug-fixer] Fix Bug
```
