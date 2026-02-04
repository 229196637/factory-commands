---
name: sce-api-reference
description: 星火编辑器API参考指南。当设计星火引擎底层API、查阅官方文档、遇到base._xxx调用时使用。关键词：星火、SCE、API、base._xxx、官方文档、引擎API。
---

# 星火编辑器API参考指南

设计星火引擎底层API或查阅官方文档时，使用此技能快速定位所需信息。

## 触发条件

- 用户说"星火API"、"SCE API"、"引擎API"
- 用户说"base._xxx"、"官方文档"、"API文档"
- 设计或实现星火引擎底层API
- 遇到 `base._xxx` 或 `self._xxx` 调用需要查阅
- 需要了解星火编辑器的Lua/TypeScript API
- 客户端/服务端API差异问题

## Official Documentation Entry

**Main Documentation**: https://doc.sce.xd.com/

> **Important**: Documentation URLs may change. If direct link returns 404, use search engine `site:doc.sce.xd.com keyword` to find

---

## Documentation Structure Navigation

### 1. Manual
- Terrain Editor: `/Manual/SceneEditor/Intro`
- Data Editor: `/Manual/DataEditor/Intro`
- Trigger Editor: `/Manual/TriggerEditor/Intro`
- UI Editor: `/Manual/UIEditor/Introduction`
- Game Mechanics: `/docs/1_Manual/4_GameMechanics/`

### 2. Technical Documentation - Lua API
- **Server Lua API**: `/技术文档/服务端Lua API/简介`
- **Client Lua API**: `/技术文档/客户端Lua API/简介`
- **Lua Modifications**: `/技术文档/服务端Lua API/我们对Lua做的修改`

### 3. Technical Documentation - TypeScript API (Recommended)
- **TS Getting Started**: `/技术文档/代码开发必读/HelloWorld_TS`
- **Lua Getting Started**: `/技术文档/代码开发必读/HelloWorld_lua`

---

## Core API Quick Reference

### Event System
```lua
-- Server event listening
base.game:event("玩家-按键按下", function(trg, player, key)
    if key == "L" then
        log.info("Pressed L key")
    end
end)

-- Client event listening
base.game:event("按键-按下", function(trg, key)
    if key == "Q" then
        log.info("Pressed Q key")
    end
end)
```

### Unit Attribute Operations
```lua
-- Get attribute value
local attack = u:get('攻击')

-- Set attribute value
u:set('攻击', 100)

-- Add attribute value
u:add('攻击', 10)

-- Attribute calculation formula: Final value = Base value * (100 + Percentage bonus) / 100
```

### Minimap API
```lua
-- Create minimap icon
local icon = base.minimap.icon(player, name, point)
icon:show()
icon:hide(team)
icon:set_time(time)  -- Set countdown

-- Send signal
base.minimap.signal(player, name, point)
```

### Log Output
```lua
log.info("Normal info")
printerror("Error info")
```

---

## Lua Special Restrictions

### Version Info
- Based on **Lua 5.3.4**
- Client: **32-bit** | Server: **64-bit**
- **Note**: 64-bit numbers sent from server to client may be truncated, recommend converting to string

### Disabled Standard Library Functions
```lua
-- Unsupported OS functions
os.execute, os.exit, os.getenv, os.remove, os.rename, os.setlocale, os.tmpname

-- Unsupported IO functions
io.input, io.output, io.tmpfile, io.popen

-- Unsupported load functions
dofile, loadfile, package.loadlib
```

### Available File Operations
```lua
-- Read-only access to map files
local f = assert(io.open('script/main.lua'))
print(f:read('a'))
f:close()

for line in io.lines('script/main.lua') do
    print(line)
end
```

### Debug Functions
```lua
if debug_bp then
    debug_bp()        -- Force breakpoint (requires debugger connection)
    debug_bp(60000)   -- Wait for debugger connection, max 60 seconds
    debug_bp(true)    -- Wait indefinitely for debugger connection
end
-- Note: debug_bp not supported in production environment
```

### Random Numbers
```lua
-- math.randomseed is disabled
-- Use customized random number generator, guaranteed not affected by external factors
local n = math.random(1, 100)
```

---

## Project File Structure

```
Project Directory/
├── script/           # Server scripts
│   └── main.lua      # Server entry
├── ui/
│   └── src/
│       └── main.lua  # Client entry
├── editor/           # Data editor config
├── scene/            # Scene files
└── res/              # Resource files
```

---

## Lookup Workflow

### When Encountering Unknown API:

1. **Identify API Source**
   - `base.xxx` → Spark low-level API
   - `self._xxx` → May be internal method
   - Check if it's server or client code

2. **Consult Official Documentation**
   ```
   Search: site:doc.sce.xd.com [API name]
   ```

3. **If Documentation Not Found**
   - Check if there's similar usage in project
   - Check trigger editor generated code
   - Consult developer community

### Common Links
- Creator Center (Test): https://developer-alpha.spark.xd.com/
- Creator Center (Production): https://developer.spark.xd.com/
- Developer Exchange Group: https://qm.qq.com/q/kuDBOInVPq

---

## Important Notes

1. **TypeScript is the recommended language** - Lua documentation may be incomplete
2. **Lua+ is deprecated** - Files with `--- lua_plus ---` at top are old format
3. **Client/Server separation** - Pay attention to code execution environment
4. **32/64-bit difference** - Large numbers need to be converted to string for cross-end transfer
5. **File path UTF-8** - All file path encodings are UTF-8
