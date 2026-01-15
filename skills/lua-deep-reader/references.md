# 边界API参考

深度阅读Lua代码时，遇到以下API应停止追踪。

## 星火底层API (base._xxx)

这些是星火编辑器的内部实现，无法追踪源码。

### 单位相关
| API | 说明 |
|-----|------|
| `base._unit_set_attr` | 设置单位属性 |
| `base._unit_get_attr` | 获取单位属性 |
| `base._create_unit` | 创建单位 |
| `base._destroy_unit` | 销毁单位 |
| `base._unit_add_buff` | 添加Buff |
| `base._unit_remove_buff` | 移除Buff |

### UI相关
| API | 说明 |
|-----|------|
| `base.gui_new` | 创建UI界面 |
| `base.gui_check` | 检查UI是否有效 |
| `base.gui_destroy` | 销毁UI |

### 游戏相关
| API | 说明 |
|-----|------|
| `base.game:event` | 注册游戏事件 |
| `base.minimap.*` | 小地图操作 |

---

## 服务器协议

客户端-服务器通信边界，协议内容由服务端定义。

### 协议命名规则
| 前缀 | 方向 | 说明 |
|-----|------|------|
| `GS2C` | 服务器→客户端 | Game Server to Client |
| `C2GS` | 客户端→服务器 | Client to Game Server |

### 常见协议模块
| 模块 | 文件 | 说明 |
|-----|------|------|
| `netwar` | `net/netwar.lua` | 战斗协议 |
| `netlogin` | `net/netlogin.lua` | 登录协议 |
| `netmap` | `net/netmap.lua` | 地图协议 |
| `netchat` | `net/netchat.lua` | 聊天协议 |

---

## 引擎API (UnityEngine.*)

Unity/星火引擎封装，底层由C#/C++实现。

### GameObject
| API | 说明 |
|-----|------|
| `UnityEngine.GameObject.New` | 创建游戏对象 |
| `UnityEngine.GameObject.Destroy` | 销毁游戏对象 |
| `UnityEngine.GameObject.Find` | 查找游戏对象 |
| `UnityEngine.GameObject.CreateFromObject` | 从对象创建 |

### Transform
| API | 说明 |
|-----|------|
| `transform:SetParent` | 设置父节点 |
| `transform.position` | 位置 |
| `transform.rotation` | 旋转 |
| `transform.localScale` | 缩放 |

### 其他
| API | 说明 |
|-----|------|
| `UnityEngine.Time.*` | 时间相关 |
| `UnityEngine.AudioClip.*` | 音频相关 |
| `UnityEngine.ParticleAsset.*` | 粒子特效 |
| `UnityEngine.ActorTextureAsset.*` | 角色贴图 |

---

## 类型系统 (classtype.*)

底层类型定义，由引擎提供。

| 类型 | 说明 |
|-----|------|
| `classtype.SceTransform` | 星火Transform组件 |
| `classtype.MeshRender` | 网格渲染器 |
| `classtype.Camera` | 摄像机 |
| `classtype.UIFollowTarget` | UI跟随目标 |

---

## 全局控制器 (g_*Ctrl)

项目中的全局单例控制器，通常已有完整实现。

| 控制器 | 说明 |
|--------|------|
| `g_WarCtrl` | 战斗控制器 |
| `g_MapCtrl` | 地图控制器 |
| `g_ResCtrl` | 资源控制器 |
| `g_AudioCtrl` | 音频控制器 |
| `g_TimeCtrl` | 时间控制器 |
| `g_NotifyCtrl` | 通知控制器 |
| `g_ViewCtrl` | 界面控制器 |
| `g_HudCtrl` | HUD控制器 |
| `g_CameraCtrl` | 摄像机控制器 |

---

## 判断是否为边界的规则

1. **带下划线的base方法**: `base._xxx` → 停止
2. **服务器协议**: `GS2C*`, `C2GS*` → 停止
3. **引擎命名空间**: `UnityEngine.*` → 停止
4. **类型定义**: `classtype.*` → 停止
5. **非项目文件**: 路径不在项目目录内 → 停止
6. **已分析过的函数**: 避免循环 → 停止
