# 调试代码模板库

## 基础模板

### 变量打印
```lua
--DEBUG: 打印单个变量
print("[模块名] varName:", varName)

--DEBUG: 打印多个变量
print("[模块名] a:", a, "b:", b, "c:", c)

--DEBUG: 格式化数字
print(string.format("[模块名] pos: (%.2f, %.2f, %.2f)", x, y, z))
```

### 表打印
```lua
--DEBUG: 简单表
print("[模块名] table:", table.tostring(t))

--DEBUG: 深度打印（如果有 dump 函数）
print("[模块名] table:", dump(t))

--DEBUG: 手动遍历
for k, v in pairs(t) do
    print("[模块名] key:", k, "value:", v)
end
```

### 堆栈追踪
```lua
--DEBUG: 完整堆栈
print(debug.traceback())

--DEBUG: 简化堆栈（调用者信息）
local info = debug.getinfo(2, "Sl")
print("[模块名] 调用自:", info.source, "行:", info.currentline)

--DEBUG: 带消息的堆栈
print(debug.traceback("[模块名] 追踪点"))
```

---

## 场景模板

### nil 值追踪
```lua
--DEBUG: nil 检查
if obj == nil then
    print("[模块名] obj 为 nil!")
    print(debug.traceback())
    return
end

--DEBUG: 链式 nil 检查
print("[模块名] self:", self or "nil")
print("[模块名] self.m_Data:", self and self.m_Data or "nil")
print("[模块名] self.m_Data.value:", self and self.m_Data and self.m_Data.value or "nil")
```

### 函数调用追踪
```lua
--DEBUG: 函数入口
function MyClass.MyFunc(self, param1, param2)
    print("[MyClass.MyFunc] 入口")
    print("[MyClass.MyFunc] param1:", param1, "param2:", param2)
    print(debug.traceback())
    
    -- 原有代码...
    
    --DEBUG: 函数出口
    print("[MyClass.MyFunc] 出口, 返回:", result)
    return result
end
```

### 条件分支追踪
```lua
--DEBUG: 分支追踪
print("[模块名] 条件值:", condition, "类型:", type(condition))
if condition then
    print("[模块名] → 进入 true 分支")
    -- ...
elseif otherCondition then
    print("[模块名] → 进入 elseif 分支, otherCondition:", otherCondition)
    -- ...
else
    print("[模块名] → 进入 else 分支")
    -- ...
end
```

### 循环追踪
```lua
--DEBUG: 循环追踪
print("[模块名] 循环开始, 总数:", #list)
for i, item in ipairs(list) do
    print("[模块名] 迭代", i, "/", #list, "item:", item)
    -- 原有代码...
end
print("[模块名] 循环结束")
```

### 异步回调追踪
```lua
--DEBUG: 回调注册
print("[模块名] 注册回调, 帧:", UnityEngine.Time.frameCount)
SomeAsyncFunc(function(result)
    --DEBUG: 回调触发
    print("[模块名] 回调触发, 帧:", UnityEngine.Time.frameCount)
    print("[模块名] 回调结果:", result)
    -- 原有代码...
end)
```

### 时序追踪
```lua
--DEBUG: 带时间戳
local function debugLog(module, msg, ...)
    local time = os.date("%H:%M:%S")
    local frame = UnityEngine.Time.frameCount or 0
    print(string.format("[%s][帧%d][%s] %s", time, frame, module, msg), ...)
end

debugLog("Camera", "位置更新", x, y, z)
```

---

## 特定问题模板

### 对象生命周期
```lua
--DEBUG: 创建追踪
function MyClass.ctor(self)
    print("[MyClass] 创建实例, id:", self.m_ID or "无ID")
    print(debug.traceback())
end

--DEBUG: 销毁追踪
function MyClass.OnDestroy(self)
    print("[MyClass] 销毁实例, id:", self.m_ID or "无ID")
    print(debug.traceback())
end

--DEBUG: 使用前检查
if self.m_IsDestroyed then
    print("[MyClass] 警告: 对象已销毁但仍被调用!")
    print(debug.traceback())
    return
end
```

### 事件系统
```lua
--DEBUG: 事件发送
print("[EventSystem] 发送事件:", eventName, "数据:", data)

--DEBUG: 事件接收
print("[EventSystem] 接收事件:", eventName, "处理器:", handlerName)
```

### 网络消息
```lua
--DEBUG: 发送消息
print("[Net] 发送:", msgName, "数据:", table.tostring(data))

--DEBUG: 接收消息
print("[Net] 接收:", msgName, "数据:", table.tostring(data))
```

### 资源加载
```lua
--DEBUG: 加载追踪
print("[Res] 开始加载:", path)
local res = LoadResource(path)
print("[Res] 加载完成:", path, "结果:", res and "成功" or "失败")
```

---

## 性能相关

### 耗时统计
```lua
--DEBUG: 函数耗时
local startTime = os.clock()
-- 执行代码...
print("[模块名] 耗时:", (os.clock() - startTime) * 1000, "ms")
```

### 调用计数
```lua
--DEBUG: 调用计数（放在函数开头）
self._debugCallCount = (self._debugCallCount or 0) + 1
if self._debugCallCount % 100 == 0 then
    print("[模块名] 调用次数:", self._debugCallCount)
end
```

---

## 清理检查表

调试完成后，搜索以下标记进行清理：
- `--DEBUG:`
- `print("[`
- `debug.traceback`
- `_debugCallCount`
- `_debugLog`
