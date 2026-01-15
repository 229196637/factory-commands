# 星火编辑器日志分析参考

## 日志目录结构

```
E:\Tools\星火编辑器\logs\
├── lua/                    # Lua脚本日志（主要分析目标）
│   ├── lua-editor-*.log    # 编辑器Lua日志
│   └── lua-game-*.log      # 游戏运行时Lua日志
├── server/                 # 服务端日志
├── game/                   # 游戏日志
├── lobby/                  # 大厅日志
├── Network/                # 网络日志
├── CSharp/                 # C#日志
├── downloader/             # 下载器日志
├── updater/                # 更新器日志
└── ui/                     # UI日志
```

## 路径映射表

| 日志中的路径前缀 | 项目实际路径 |
|-----------------|-------------|
| `p_qaz1/client/` | `ui/script/client/` |
| `p_qaz1/server/` | `script/server/` |
| `client/` | `ui/script/client/` |
| `server/` | `script/server/` |
| `base/` | 星火引擎内部代码（通常不可修改） |
| `common/` | 星火引擎公共代码 |

## 常见错误类型

### 1. nil value 错误
```
attempt to index a nil value (field 'xxx')
attempt to call a nil value (method 'xxx')
attempt to perform arithmetic on a nil value
```

**常见原因**:
- 对象未初始化
- 异步加载未完成就访问
- 对象已被销毁

### 2. bad argument 错误
```
bad argument #1 to 'xxx' (number expected, got nil)
bad argument #2 to 'xxx' (string expected, got table)
```

**常见原因**:
- 参数类型错误
- 参数缺失

### 3. 类型错误
```
attempt to concatenate a nil value
attempt to compare nil with number
```

## 日志级别

| 级别 | 说明 |
|-----|------|
| `[info]` | 普通信息 |
| `[warn]` | 警告信息 |
| `[error]` | 错误信息 |
| `[debug]` | 调试信息 |

## 项目代码约定

### 调试日志格式
```lua
print("[模块名] 信息")
printerror("[模块名] 错误信息")
```

### 错误处理
```lua
check_call(func, ...)  -- 安全调用，捕获错误
xxpcall(func, ...)     -- 扩展pcall
```

### 星火API标识
- `base._xxx` - 星火引擎API
- `self._xxx` - 可能是星火API，需确认

## 快速定位命令

### 搜索错误日志
```
Grep pattern="error|错误|nil value|bad argument" path="E:\Tools\星火编辑器\logs\lua"
```

### 搜索特定模块日志
```
Grep pattern="\[CWarrior\]|\[netwar\]" path="E:\Tools\星火编辑器\logs\lua"
```

### 获取最新日志文件
按修改时间排序，选择最新的 `lua-game-*.log` 或 `lua-editor-*.log`
