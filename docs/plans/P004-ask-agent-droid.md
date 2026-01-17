---
id: P004
title: 为 Ask 命令启用专用 Agent
status: completed
created: 2026-01-15
---

# 为 Ask 命令启用专用 Agent

## 概述
更新现有的 `/ask` 命令，使其启用一个专门的 ask agent (droid) 来处理咨询请求。这样可以利用 agent 的独立上下文窗口进行深度研究，同时保持主对话的简洁。

## 技术方案

### 关键实现步骤
1. 创建新的 `ask-researcher.md` droid 文件
2. 修改 `ask.md` 命令，使用 `Task` 工具调用该 droid
3. droid 配置为只读模式 (`tools: read-only`)

### 需要创建/修改的文件
- **新建**: `.factory/droids/ask-researcher.md` - Ask 研究员 droid
- **修改**: `.factory/commands/ask.md` - 更新为调用 agent

### Droid 设计
```yaml
name: ask-researcher
description: 只读研究代理，深度分析代码库回答问题
model: inherit
tools: read-only
```

### 命令更新
- 使用 `Task` 工具启动 `ask-researcher` droid
- 传递用户问题作为 prompt
- 返回 agent 的研究结果

## 验收标准
- [ ] `ask-researcher.md` droid 文件已创建
- [ ] `/ask <问题>` 能正确启动 agent
- [ ] agent 只能使用只读工具
- [ ] 研究结果能正确返回给用户

## 实现任务
1. 创建 `.factory/droids/ask-researcher.md` droid 定义
2. 更新 `.factory/commands/ask.md` 使用 Task 工具调用 droid
3. 测试命令是否正常工作

## 风险与注意事项
- 确保 droid 的 `tools: read-only` 配置正确生效
- agent 返回的结果可能较长，需要考虑如何简洁呈现
- 参考现有的 `repo-research-analyst.md` droid 设计
