# 星火编辑器文档链接索引

## 官方入口

| 资源 | 链接 |
|------|------|
| **文档首页** | https://doc.sce.xd.com/ |
| **创作者中心(测试版)** | https://developer-alpha.spark.xd.com/ |
| **创作者中心(线上版)** | https://developer.spark.xd.com/ |
| **编辑器下载(测试版)** | https://package-pd.spark.xd.com/alpha_editor.html |
| **开发者交流群** | https://qm.qq.com/q/kuDBOInVPq |

---

## 技术文档索引

### 代码开发入门
| 文档 | 路径 |
|------|------|
| Lua版Hello World | `/技术文档/代码开发必读/HelloWorld_lua` |
| TypeScript版Hello World | `/技术文档/代码开发必读/HelloWorld_TS` |

### 服务端 Lua API
| 模块 | 路径 | 说明 |
|------|------|------|
| 简介 | `/技术文档/服务端Lua API/简介` | Lua开发入门 |
| Lua修改 | `/技术文档/服务端Lua API/我们对Lua做的修改` | 标准库限制说明 |
| 单位属性 | `/技术文档/服务端Lua API/单位属性` | u:get/set/add |
| 小地图 | `/技术文档/服务端Lua API/小地图` | base.minimap |

### 客户端 Lua API
| 模块 | 路径 | 说明 |
|------|------|------|
| 简介 | `/技术文档/客户端Lua API/简介` | 客户端开发入门 |
| 游戏 | `/技术文档/客户端Lua API/游戏` | base.game |

---

## 功能手册索引

### 编辑器
| 编辑器 | 路径 |
|--------|------|
| 地形编辑器 | `/Manual/SceneEditor/Intro` |
| 数据编辑器 | `/Manual/DataEditor/Intro` |
| 触发编辑器 | `/Manual/TriggerEditor/Intro` |
| 界面编辑器 | `/Manual/UIEditor/Introduction` |
| 血条编辑器 | `/Manual/HealthBarEditor/HealthBarEditor` |
| 类型编辑器 | `/Manual/TriggerEditor/Advanced/TypeEditor` |

### 配置与发布
| 文档 | 路径 |
|------|------|
| 项目文件夹介绍 | `/Manual/VersionControl/FolderDescription` |
| 核心配置项 | `/Manual/DataEditor/CoreConfiguration` |
| 常用链接 | `/Manual/Welcome/CommonLinks` |
| 开发者常见问题 | `/Manual/Welcome/QuestionsAboutDeveloper` |
| 编辑器常见问题 | `/Manual/Welcome/QuestionsAboutEditor` |

---

## API分类速查

### base 全局对象
```
base.game       - 游戏核心（事件、聊天、退出等）
base.minimap    - 小地图（图标、信号）
base.player     - 玩家相关
base.unit       - 单位相关
base.item       - 物品相关
base.buff       - Buff相关
base.skill      - 技能相关
```

### 常用事件名称
```
-- 服务端事件
"玩家-按键按下"
"玩家-进入游戏"
"玩家-离开游戏"
"单位-死亡"
"单位-受到伤害"

-- 客户端事件
"按键-按下"
"按键-抬起"
"鼠标-点击"
```

### 日志函数
```lua
log.info(msg)      -- 普通日志
log.warn(msg)      -- 警告日志
log.error(msg)     -- 错误日志
printerror(msg)    -- 错误输出（代码中主动调用）
```

---

## 搜索技巧

当直接链接404时，使用以下搜索方式：

```
# Google/Bing 搜索
site:doc.sce.xd.com [关键词]

# 示例
site:doc.sce.xd.com 单位属性
site:doc.sce.xd.com base.game
site:doc.sce.xd.com 事件监听
```

---

## 版本说明

- **Lua版本**: 5.3.4
- **客户端Lua**: 32位
- **服务端Lua**: 64位
- **主推语言**: TypeScript
- **Lua+状态**: 已废弃

---

*最后更新: 2026-01-12*
