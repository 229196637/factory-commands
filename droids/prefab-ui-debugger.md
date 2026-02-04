---
name: prefab-ui-debugger
description: Unity Prefab UI调试预览工具。AI生成/修改UI后自动运行，用户可视化调整布局、为任意组件添加AI修改指令、创建新锚点，生成session文件供AI读取执行。关键词：Prefab、UI调试、Unity UI、预览、布局调整。
model: inherit
---

# Prefab UI Debugger

Unity Prefab UI可视化调试工具，用于预览和调整UI布局，生成AI可读的修改指令。

## 完整执行流程

### 1. 启动工具 (PowerShell)
```powershell
Start-Process python -ArgumentList '"D:\Projects\UnityProjects\client\mytools\prefabDebugger\prefab_viewer.py"', '"<prefab完整路径>"' -WindowStyle Normal
```

### 2. 等待用户完成操作并导出
用户在工具中操作后点击 "Export Session" 按钮。

### 3. 读取最新session文件
```python
import os
import json
from glob import glob

sessions_dir = "mytools/prefabDebugger/sessions"
session_files = sorted(glob(os.path.join(sessions_dir, "session_*.json")))
if session_files:
    latest_session = session_files[-1]
    with open(latest_session, 'r', encoding='utf-8') as f:
        session_data = json.load(f)
```

### 4. 执行修改
- 对于 `position_changed=true` 的节点，更新 Transform 的 m_LocalPosition
- 对于 `new_anchors`，创建完整的 GameObject + Transform + UISprite/UILabel

## Unity Prefab 编辑要点

创建新节点需要添加3个部分：
1. **GameObject** (`!u!1`): 包含 m_Component 引用
2. **Transform** (`!u!4`): 包含位置、父子关系
3. **MonoBehaviour** (`!u!114`): UISprite/UILabel 组件数据

FileID 命名规则：
- GameObject: `1100000000000XXX`
- Transform: `4100000000000XXX`  
- MonoBehaviour: `114100000000XXX`

## Session文件位置

```
mytools/prefabDebugger/sessions/session_*.json
```

## Session JSON结构示例

```json
{
  "prefab_path": "Assets/GameRes/UI/Test/TestView.prefab",
  "timestamp": "2026-01-27T14:30:52",
  "node_instructions": [
    {
      "node_path": "TestView/Label",
      "node_name": "Label",
      "component_type": "UILabel",
      "original_position": {"x": 0, "y": 0, "z": 0},
      "new_position": {"x": 10, "y": 20, "z": 0},
      "position_changed": true,
      "ai_instruction": "修改文字为'任务列表'，字号改为24"
    }
  ],
  "new_anchors": [
    {
      "anchor_name": "CloseButtonAnchor",
      "position": {"x": 450, "y": 250},
      "size": {"width": 60, "height": 60},
      "anchor_type": "center",
      "ai_instruction": "type:UISprite, sprite:btn_close, depth:10"
    }
  ]
}
```

## 日志文件

人类可读的操作日志：
```
mytools/prefabDebugger/logs/debug_history.log
```
