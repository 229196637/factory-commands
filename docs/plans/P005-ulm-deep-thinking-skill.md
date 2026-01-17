---
id: P005
title: ULM深度思考模式 - Plan命令增强
status: completed
created: 2026-01-15
---

# ULM深度思考模式 - Plan命令增强

## 概述

修改现有的 `/plan` 命令，当用户输入以 `ulm` 前缀开头时，自动启用深度思考模式：

- **主流程**: 切换到 `claude-opus-4-5-think` 模型进行深度推理
- **子任务**: 调用 `api-analyzer` droid（使用默认 `claude-opus-4-5-20251101` 模型）分析代码调用链

深度思考模式会在独立上下文中分析代码调用链，最小化每个API的输入参数，生成更精简的实现方案。

## 技术方案

### 关键实现步骤

1. **创建深度思考 Skill** (`deep-thinking`)
   - 位置: `.factory/skills/deep-thinking/SKILL.md`
   - 功能: 定义深度思考的工作流程
   - 核心: 调用 `api-analyzer` droid 分析代码调用链

2. **修改 Plan 命令**
   - 位置: `.factory/commands/plan.md`
   - 检测 `$ARGUMENTS` 是否以 `ulm` 开头
   - 如果是，先调用深度思考 skill，再生成计划

3. **深度思考工作流**
   ```
   用户输入: ulm 实现xxx功能
         ↓
   检测到 ulm 前缀
         ↓
   启动 deep-thinking skill
         ↓
   调用 api-analyzer droid (独立上下文)
         ↓
   分析代码调用链，最小化输入
         ↓
   返回精简的API清单
         ↓
   基于精简API生成实现计划
   ```

### 需要创建/修改的文件

| 操作 | 文件路径 | 说明 |
|------|---------|------|
| 新建 | `.factory/skills/deep-thinking/SKILL.md` | 深度思考技能定义 |
| 修改 | `.factory/commands/plan.md` | 添加 ulm 前缀检测逻辑 |

### Skill 设计

```markdown
---
name: deep-thinking
description: 深度思考模式，使用claude-opus-4-5-think模型进行深度推理。当plan命令检测到ulm前缀时自动触发。
model: custom:claude-opus-4-5-think  # 关键：指定思考模型
---

# 深度思考模式

## 触发条件
- plan 命令检测到 `ulm` 前缀

## 工作流程
1. 解析用户需求，提取功能关键词
2. 调用 api-analyzer droid 分析相关模块（使用默认模型 claude-opus-4-5-20251101）
3. 整理最小化的API输入清单
4. 基于深度推理生成精简的技术方案
```

### 模型配置说明

| 组件 | 模型 | 说明 |
|------|------|------|
| deep-thinking skill | `custom:claude-opus-4-5-think` | 主流程深度推理 |
| api-analyzer droid | `inherit` (默认 claude-opus-4-5-20251101) | 子任务代码分析 |

### Plan 命令修改逻辑

```markdown
## 参数解析

检查 $ARGUMENTS 是否以 "ulm" 开头：

1. **如果以 "ulm" 开头**:
   - 提取 ulm 后面的实际需求
   - 启用 deep-thinking skill
   - 调用 api-analyzer droid 分析代码
   - 基于分析结果生成精简计划

2. **如果不以 "ulm" 开头**:
   - 使用原有的 plan 流程
```

## 验收标准

- [ ] `deep-thinking` skill 文件已创建
- [ ] `/plan ulm xxx` 能正确触发深度思考模式
- [ ] 深度思考模式能调用 `api-analyzer` droid
- [ ] 生成的计划包含最小化的API输入清单
- [ ] 不带 `ulm` 前缀的 plan 命令保持原有行为

## 实现任务

1. 创建 `.factory/skills/deep-thinking/SKILL.md` 技能定义
2. 修改 `.factory/commands/plan.md` 添加 ulm 前缀检测
3. 在 plan.md 中集成 deep-thinking skill 调用逻辑
4. 测试 `/plan ulm xxx` 命令是否正常工作
5. 测试普通 `/plan xxx` 命令是否保持原有行为

## 风险与注意事项

- `api-analyzer` droid 已存在，需确保其输出格式与 deep-thinking skill 兼容
- 深度思考模式会增加 plan 命令的执行时间（因为需要额外的 agent 调用）
- 需要处理 api-analyzer 返回结果为空的情况
- ulm 前缀检测应该不区分大小写（ulm/ULM/Ulm 都应该触发）

## 参考资料

- 现有 api-analyzer droid: `.factory/droids/api-analyzer.md`
- 现有 skill 示例: `.factory/skills/lua-deep-reader/SKILL.md`
- ask 命令调用 agent 示例: `.factory/commands/ask.md`
