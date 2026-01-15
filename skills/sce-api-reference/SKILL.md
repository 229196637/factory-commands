---
name: sce-api-reference
description: 星火编辑器API参考指南。当设计星火引擎底层API、查阅官方文档、遇到base._xxx调用时使用。提供文档导航和核心API速查。
---

# 星火编辑器 API 参考指南

当需要设计星火引擎底层API或查阅官方文档时，使用此技能快速定位所需信息。

## 触发场景

- 设计或实现星火引擎底层API
- 遇到 `base._xxx` 或 `self._xxx` 调用需要查阅
- 需要了解星火编辑器的Lua/TypeScript API
- 客户端/服务端API差异问题

## 官方文档入口

**主文档**: https://doc.sce.xd.com/

> **重要**: 文档URL可能变化，如果直接链接404，使用搜索引擎 `site:doc.sce.xd.com 关键词` 查找

---

## 文档结构导航

### 1. 功能手册 (Manual)
- 地形编辑器: `/Manual/SceneEditor/Intro`
- 数据编辑器: `/Manual/DataEditor/Intro`
- 触发编辑器: `/Manual/TriggerEditor/Intro`
- 界面编辑器: `/Manual/UIEditor/Introduction`
- 游戏机制: `/docs/1_Manual/4_GameMechanics/`

### 2. 技术文档 - Lua API
- **服务端Lua API**: `/技术文档/服务端Lua API/简介`
- **客户端Lua API**: `/技术文档/客户端Lua API/简介`
- **Lua修改说明**: `/技术文档/服务端Lua API/我们对Lua做的修改`

### 3. 技术文档 - TypeScript API (主推)
- **TS入门**: `/技术文档/代码开发必读/HelloWorld_TS`
- **Lua入门**: `/技术文档/代码开发必读/HelloWorld_lua`

---

## 核心API速查

### 事件系统
```lua
-- 服务端事件监听
base.game:event("玩家-按键按下", function(trg, player, key)
    if key == "L" then
        log.info("按下了L键")
    end
end)

-- 客户端事件监听
base.game:event("按键-按下", function(trg, key)
    if key == "Q" then
        log.info("按下了Q键")
    end
end)
```

### 单位属性操作
```lua
-- 获取属性值
local attack = u:get('攻击')

-- 设置属性值
u:set('攻击', 100)

-- 增加属性值
u:add('攻击', 10)

-- 属性计算公式: 最终值 = 基础值 * (100 + 百分比加成) / 100
```

### 小地图API
```lua
-- 创建小地图图标
local icon = base.minimap.icon(player, name, point)
icon:show()
icon:hide(team)
icon:set_time(time)  -- 设置倒计时

-- 发送信号
base.minimap.signal(player, name, point)
```

### 日志输出
```lua
log.info("普通信息")
printerror("错误信息")
```

---

## Lua特殊限制

### 版本信息
- 基于 **Lua 5.3.4**
- 客户端: **32位** | 服务端: **64位**
- **注意**: 64位数字从服务端发到客户端可能截断，建议转字符串

### 禁用的标准库函数
```lua
-- 不支持的OS函数
os.execute, os.exit, os.getenv, os.remove, os.rename, os.setlocale, os.tmpname

-- 不支持的IO函数
io.input, io.output, io.tmpfile, io.popen

-- 不支持的加载函数
dofile, loadfile, package.loadlib
```

### 可用的文件操作
```lua
-- 只读访问地图文件
local f = assert(io.open('script/main.lua'))
print(f:read('a'))
f:close()

for line in io.lines('script/main.lua') do
    print(line)
end
```

### 调试函数
```lua
if debug_bp then
    debug_bp()        -- 强制断点（需调试器连接）
    debug_bp(60000)   -- 等待调试器连接，最多60秒
    debug_bp(true)    -- 无限等待调试器连接
end
-- 注意: 正式环境不支持 debug_bp
```

### 随机数
```lua
-- math.randomseed 已禁用
-- 使用定制的随机数发生器，保证不受外部因素影响
local n = math.random(1, 100)
```

---

## 项目文件结构

```
项目目录/
├── script/           # 服务端脚本
│   └── main.lua      # 服务端入口
├── ui/
│   └── src/
│       └── main.lua  # 客户端入口
├── editor/           # 数据编辑器配置
├── scene/            # 场景文件
└── res/              # 资源文件
```

---

## 查阅流程

### 当遇到未知API时：

1. **识别API来源**
   - `base.xxx` → 星火底层API
   - `self._xxx` → 可能是内部方法
   - 检查是服务端还是客户端代码

2. **查阅官方文档**
   ```
   搜索: site:doc.sce.xd.com [API名称]
   ```

3. **如果文档未找到**
   - 检查项目中是否有类似用法
   - 查看触发编辑器生成的代码
   - 咨询开发者社区

### 常用链接
- 创作者中心(测试版): https://developer-alpha.spark.xd.com/
- 创作者中心(线上版): https://developer.spark.xd.com/
- 开发者交流群: https://qm.qq.com/q/kuDBOInVPq

---

## 注意事项

1. **TypeScript是主推语言** - Lua文档可能不完整
2. **Lua+已废弃** - 文件顶部有 `--- lua_plus ---` 的是旧格式
3. **客户端/服务端分离** - 注意代码运行环境
4. **32/64位差异** - 大数字跨端传输需转字符串
5. **文件路径UTF-8** - 所有文件路径编码都是UTF-8
