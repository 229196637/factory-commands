---
name: debug-info-adder
description: 添加调试信息来定位问题。当遇到陌生问题、无法直接从逻辑推断、需要打印变量值或追踪调用链时使用。
---

# 调试信息添加指南

当遇到陌生问题，无法直接从逻辑推断时，系统化地添加调试信息来定位问题。

## 触发场景

- 无法直接从逻辑推断问题原因
- 变量值不明确，需要打印确认
- 调用链不清晰，需要堆栈跟踪
- 时序问题，需要时间戳追踪
- 异步回调中的状态不确定

---

## 调试信息添加策略

### 策略A: 单点调试（简单问题）

适用于：变量值不明确、单个函数内的问题

```lua
--DEBUG: 打印变量值
print("[模块名] 变量名:", 变量值)

--DEBUG: 打印多个变量
print("[模块名] 状态:", var1, var2, var3)

--DEBUG: 打印表内容
print("[模块名] 表内容:", table.tostring(myTable))
```

### 策略B: 调用链追踪（复杂问题）

适用于：不清楚函数被谁调用、调用顺序问题

```lua
--DEBUG: 函数入口 + 堆栈
print("[模块名] 函数入口, 参数:", param1, param2)
print(debug.traceback())

--DEBUG: 简化堆栈（只打印关键信息）
print("[模块名] 调用来源:", debug.getinfo(2, "Sl").source, debug.getinfo(2, "Sl").currentline)
```

### 策略C: 时序追踪（异步/时序问题）

适用于：异步回调、帧更新、事件顺序问题

```lua
--DEBUG: 带时间戳的日志
print(string.format("[%s][帧%d][模块名] 事件: %s", 
    os.date("%H:%M:%S"), 
    UnityEngine.Time.frameCount or 0, 
    描述))

--DEBUG: 异步回调追踪
print("[模块名] 回调触发, 当前状态:", self.m_State)
```

### 策略D: 条件断点（特定条件问题）

适用于：只在特定条件下出现的问题

```lua
--DEBUG: 条件打印
if 特定条件 then
    print("[模块名] 触发条件:", 条件值)
    print(debug.traceback())
end

--DEBUG: 计数器
self._debugCount = (self._debugCount or 0) + 1
if self._debugCount % 100 == 0 then
    print("[模块名] 调用次数:", self._debugCount)
end
```

---

## 调试点选择原则

### 1. 入口点
在函数入口打印参数，确认输入是否正确：
```lua
function MyClass.DoSomething(self, param1, param2)
    --DEBUG: 入口点
    print("[MyClass.DoSomething] 入口, param1:", param1, "param2:", param2)
    -- 原有代码...
end
```

### 2. 分支点
在 if/else 分支打印条件值，确认走了哪个分支：
```lua
--DEBUG: 分支点
print("[模块名] 条件判断, condition:", condition, "type:", type(condition))
if condition then
    print("[模块名] 进入 true 分支")
else
    print("[模块名] 进入 false 分支")
end
```

### 3. 调用点
在关键函数调用前后打印，确认调用是否成功：
```lua
--DEBUG: 调用前
print("[模块名] 即将调用 SomeFunction, 参数:", arg)
local result = SomeFunction(arg)
--DEBUG: 调用后
print("[模块名] SomeFunction 返回:", result, "type:", type(result))
```

### 4. 异常点
在可能出错的位置打印状态：
```lua
--DEBUG: nil 检查点
if not obj then
    print("[模块名] obj 为 nil!")
    print(debug.traceback())
    return
end
print("[模块名] obj 存在, 类型:", type(obj))
```

---

## 调试信息格式规范

### 标准格式
```lua
-- 普通信息
print("[模块名] 描述:", 值)

-- 警告信息
print("[模块名][WARN] 描述:", 值)

-- 错误信息
printerror("[模块名] 错误描述:", 错误信息)
```

### 模块名命名
- 使用类名或文件名: `[CResCtrl]`, `[Camera]`, `[SpringPanel]`
- 包含函数名更清晰: `[Camera.SetPosition]`, `[CResCtrl.Load]`

### 值的格式化
```lua
-- 数字保留小数
print(string.format("[模块名] 位置: (%.2f, %.2f, %.2f)", x, y, z))

-- 布尔值明确显示
print("[模块名] 是否启用:", enabled and "true" or "false")

-- nil 值明确标注
print("[模块名] 对象:", obj or "nil")
```

---

## 调试流程

### 步骤1: 确定问题范围
1. 阅读错误日志，确定出错位置
2. 分析调用栈，理解调用链
3. 确定需要添加调试信息的范围

### 步骤2: 添加调试信息
1. 在关键位置添加 `--DEBUG:` 标记的打印语句
2. 选择合适的策略（单点/调用链/时序）
3. 确保打印信息足够定位问题

### 步骤3: 运行并分析
1. 运行程序复现问题
2. 查看日志输出
3. 根据输出缩小问题范围

### 步骤4: 迭代调试
1. 如果信息不足，添加更多调试点
2. 如果范围太大，缩小调试范围
3. 重复直到定位问题

### 步骤5: 清理调试代码
1. 问题解决后，删除 `--DEBUG:` 标记的代码
2. 保留有价值的日志点（去掉 DEBUG 标记）
3. 确保不影响正常功能

---

## 常见场景调试模板

### 场景1: nil 值错误
```lua
-- 在出错位置前添加
--DEBUG: 追踪 nil 来源
print("[模块名] 检查点1, obj:", obj or "nil")
print("[模块名] 检查点1, obj.field:", obj and obj.field or "nil")
```

### 场景2: 函数未被调用
```lua
-- 在函数入口添加
function MyFunc()
    --DEBUG: 确认函数被调用
    print("[模块名] MyFunc 被调用")
    print(debug.traceback())
end
```

### 场景3: 值不符合预期
```lua
-- 在赋值前后添加
--DEBUG: 追踪值变化
print("[模块名] 赋值前, value:", value)
value = SomeCalculation()
print("[模块名] 赋值后, value:", value)
```

### 场景4: 异步回调问题
```lua
-- 在回调注册和触发时添加
--DEBUG: 回调注册
print("[模块名] 注册回调, 当前帧:", UnityEngine.Time.frameCount)

-- 在回调函数内
--DEBUG: 回调触发
print("[模块名] 回调触发, 当前帧:", UnityEngine.Time.frameCount)
print("[模块名] 回调时状态:", self.m_State)
```

---

## 注意事项

1. **标记调试代码**: 所有调试代码都用 `--DEBUG:` 注释标记，方便后续清理
2. **避免性能影响**: 不要在高频调用的地方添加大量打印
3. **保护敏感信息**: 不要打印密码、密钥等敏感数据
4. **及时清理**: 问题解决后及时删除调试代码
5. **保留有价值的日志**: 对于关键流程，可以保留日志但去掉 DEBUG 标记

---

## 与其他技能的配合

```
问题发现 → [debug-info-adder] 添加调试信息
    ↓
运行程序 → [spark-editor-log-analyzer] 分析日志
    ↓
定位问题 → [legacy-code-bug-fixer] 修复Bug
```
