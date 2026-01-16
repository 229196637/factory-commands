# FAT 工作流系统

Factory AI Tools (FAT) 工作流系统 - 一套用于 AI 辅助软件开发的命令和代理工具。

## 命令列表

| 命令 | 说明 |
|------|------|
| `/plan [ulw] [ID] <内容>` | 创建实现计划。`ulw` 启用深度思考模式 |
| `/edit-plan [ID] [内容]` | 编辑计划 |
| `/work [true]` | 执行计划。`true` 强制测试 |
| `/list-plan` | 列出所有计划 |
| `/list-solutions` | 列出已解决问题 |
| `/ask <问题>` | 只读咨询模式 |
| `/api-analyze [模块]` | 分析模块接口 |
| `/deepen-plan <ID>` | 深化增强计划 |
| `/unit-test [路径]` | 运行单元测试 |
| `/compound` | 记录解决方案 |
| `/fat-help` | 显示帮助菜单 |

## 代理 (Droids)

| 代理 | 说明 |
|------|------|
| `deep-thinking-planner` | 深度思考计划代理 (claude-opus-4-5-think) |
| `standard-planner` | 普通计划代理 |
| `work-coder` | 执行编码任务 |
| `ask-researcher` | 只读研究代理 |
| `api-analyzer` | API 分析代理 |
| `code-reviewer-gpt` | GPT 代码审查 |
| `code-reviewer-opus` | Opus 代码审查 |

## 目录结构

```
.factory/
├── commands/          # 斜杠命令定义
├── droids/            # 代理定义
├── docs/
│   ├── plans/         # 计划文件
│   └── solutions/     # 解决方案
└── _backup/           # 备份文件
```

## 使用示例

```bash
# 创建普通计划
/plan 添加用户登录功能

# 创建深度思考计划
/plan ulw 实现复杂的权限系统

# 指定计划ID
/plan ulw P005 实现缓存系统

# 编辑计划
/edit-plan P001 添加错误处理

# 执行计划
/work true
```

## 版本历史

- v1.1 - 添加 `ulw` 深度思考模式，创建 standard-planner
- v1.0 - 初始化 FAT 工作流配置
