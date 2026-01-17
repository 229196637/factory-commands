## 研究发现

### 会话证据表

| 发现项 | 证据/引用 |
|--------|----------|
| **相关会话** | 找到70+个dahuaxiyou_checkout项目的动画相关会话 |
| **常见问题** | 1) 动画配置缺失导致播放失败 2) Animator/MeshRender初始化顺序问题 3) 动画名称不在animclipdata中 |
| **工作流变化** | 有时需要先添加配置再调用，有时需要修改Animator.lua的播放逻辑 |
| **边界情况** | 1) 新模型ID需要在animclipdata.lua添加配置 2) 新动画名需要在AnimationData.lua配置循环属性 |

### 项目动画架构分析

```
动画系统核心文件:
├── ui/script/client/UnityEngine/AnimationSystem/
│   ├── Animator.lua          # 动画控制器（Play/CrossFade等）
│   ├── AnimationClip.lua     # 动画片段
│   ├── AnimationState.lua    # 动画状态
│   └── data/AnimationData.lua # 动画循环配置
├── ui/script/client/logic/datauser/
│   ├── animclipdata.lua      # 模型动画帧数/时长配置（按模型ID）
│   └── animclipdata2.lua     # 扩展动画配置
└── ui/script/client/classtype/
    └── MeshRender.lua        # 3D模型渲染（Actor管理）
```

### 动画调用链
```
CModel/CModelBase → Animator.Play(state) → AnimationClip.Play() → Actor:play_animation()
                                        ↓
                         AnimationData.IsLoop(state) 判断是否循环
```

---

## 技能方案

### 技能名称
`animation-code-workflow`

### 技能位置
**项目级技能**: `D:\Project\GameProject\dahuaxiyou_checkout\.factory\skills\animation-code-workflow\`

（因为这是项目特定的动画框架，不适用于其他项目）

### 技能目录结构
```
animation-code-workflow/
├── SKILL.md              # 主技能文档
├── references.md         # 关键文件路径和API参考
└── checklists.md         # 动画添加检查清单
```

### 技能核心流程

**Phase 1: 需求分析**
- 识别动画类型（播放/添加配置/修改逻辑）
- 确定涉及的模型ID和动画名称

**Phase 2: 配置检查与补全**
1. 检查 `animclipdata.lua` 是否有该模型ID的动画配置
2. 检查 `AnimationData.lua` 是否有该动画名的循环配置
3. 缺失则添加配置

**Phase 3: 代码实现**
- 根据需求在指定位置调用动画API
- 使用正确的播放方法（Play/CrossFade/PlayInFixedTime）

**Phase 4: 验证**
- 确认配置完整性
- 检查调用链正确性

### 技能触发场景
- 用户说"添加动画"、"播放动画"、"动画配置"
- 用户说"animation"、"Animator"、"动画代码"
- 涉及CModel、CModelBase、Animator的动画相关修改

---

## 是否继续创建此技能？