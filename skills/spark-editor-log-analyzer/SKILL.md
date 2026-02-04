---
name: spark-editor-log-analyzer
description: 分析星火编辑器Lua日志文件，排查错误和调试信息。当用户提到星火编辑器日志、lua日志、报错分析、stack traceback时使用。关键词：星火日志、lua日志、报错、错误日志、stack traceback、日志分析。
---

# 星火编辑器日志分析

## 触发条件

- 用户说"星火日志"、"lua日志"、"编辑器日志"
- 用户说"报错"、"错误日志"、"stack traceback"
- 用户说"分析日志"、"查看日志"、"日志分析"
- 需要分析星火编辑器的Lua日志文件
- 需要排查错误和调试信息

## Instructions

**You MUST call the Task tool to launch the log-analyzer subagent.**

Use Task tool with these parameters:
- subagent_type: `log-analyzer`
- description: `Analyze Spark Editor logs`
- prompt: Pass the user's log analysis request. Tell it to analyze logs in `E:\Tools\星火编辑器\logs\lua\` and return error analysis with fix suggestions in Chinese.

**IMPORTANT**: The log-analyzer agent uses `claude-sonnet-4-5` model for efficient log analysis.

## Log Locations

- **路径**: `E:\Tools\星火编辑器\logs\lua\`
- **编辑器日志**: `lua-editor-*.log`
- **游戏日志**: `lua-game-*.log`
- **其他日志目录**: `server/`, `game/`, `lobby/`, `Network/` 等

## Path Mapping

| 日志路径 | 项目路径 |
|---------|---------|
| `p_qaz1/client/` | `ui/script/client/` |
| `p_qaz1/server/` | `script/server/` |
| `client/` | `ui/script/client/` |
| `server/` | `script/server/` |
