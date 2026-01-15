---
name: spark-editor-log-analyzer
description: 分析星火编辑器Lua日志文件，排查错误和调试信息。当用户提到星火编辑器日志、lua日志、报错分析、stack traceback时使用。
---

# 星火编辑器日志分析

## 日志位置

- **路径**: `E:\Tools\星火编辑器\logs\lua\`
- **编辑器日志**: `lua-editor-*.log`
- **游戏日志**: `lua-game-*.log`
- **其他日志目录**: `server/`, `game/`, `lobby/`, `Network/` 等

## 分析流程

### 1. 读取日志（必须读取两次）

**重要**: 由于日志文件可能正在被写入，第一次读取可能不是最新内容。

```
1. 第一次读取：获取文件内容
2. 立即进行第二次读取：确保获取最新内容
3. 对比两次内容，使用最新的版本进行分析
```

### 2. 定位错误

搜索以下关键词：
- `[error]` - 错误级别日志
- `stack traceback` - Lua调用栈
- `attempt to` - 常见Lua错误（如 `attempt to index a nil value`）
- `nil value` - 空值错误
- `bad argument` - 参数错误
- `printerror` - 代码中主动输出的错误

### 3. 解析日志格式

**标准格式**: `[时间][线程ID][级别][帧号][文件:行号] 内容`

**示例**:
```
[2026-01-12 17:40:18.938][26032][info][1][base/base/ip.lua:30] IP editor-alpha.spark.xd.com
[2026-01-12 17:40:18.938][26032][error][1][client/logic/base/ceffect.lua:58] attempt to perform arithmetic on a nil value
```

### 4. 分析调用栈

从 `stack traceback` 提取：
- 错误发生的文件和行号（第一行）
- 调用链条（从上往下是调用顺序）
- 根本原因定位（通常在最内层调用）

**示例调用栈**:
```
stack traceback:
    p_qaz1/client/core/global.lua:175: in global 'check_call'
    p_qaz1/client/logic/base/cresctrl.lua:396: in function 'LoadCloneAsync'
    p_qaz1/client/logic/base/ceffect.lua:14: in function 'ctor'
```

### 5. 路径映射

日志中的路径需要映射到项目实际路径：

| 日志路径 | 项目路径 |
|---------|---------|
| `p_qaz1/client/` | `ui/script/client/` |
| `p_qaz1/server/` | `script/server/` |
| `client/` | `ui/script/client/` |
| `server/` | `script/server/` |

## 输出格式

分析完成后，按以下格式输出：

### 1. 错误摘要
- 错误类型
- 发生位置
- 发生时间

### 2. 调用链分析
- 完整调用链条
- 每个函数的作用

### 3. 根本原因
- 为什么会出现这个错误
- 变量为何为nil/错误值

### 4. 修复方案
提供多个方案供选择：
- **方案 A**: [推荐方案]
- **方案 B**: [备选方案]
- **方案 C**: [其他方案]

每个方案需说明优缺点。

## 常见错误模式

### nil value 错误
- 检查变量初始化顺序
- 检查异步回调时机
- 检查对象是否已销毁

### bad argument 错误
- 检查参数类型
- 检查参数数量
- 检查参数是否为nil

### 缓存命中导致的同步回调问题
- 资源加载时，缓存命中会同步执行回调
- 确保回调依赖的变量在调用前已初始化

## 验证

分析完成后：
1. 使用 Grep 工具在项目中搜索相关代码
2. 使用 Read 工具查看具体文件内容
3. 确认修复方案不会引入新问题
