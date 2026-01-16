# 相关命令参考

## /compound

记录已解决的问题到知识库。

**位置**: `.factory/commands/compound.md`

**用法**: `/compound [问题描述]`

**功能**:
- 从对话上下文收集问题信息
- 生成解决方案 ID (S001, S002, ...)
- 创建解决方案文档到 `.factory/docs/solutions/`

---

## /list-solutions

列出所有已记录的解决方案。

**位置**: `.factory/commands/list-solutions.md`

**用法**: `/list-solutions`

**功能**:
- 扫描 solutions 目录
- 以表格形式展示所有解决方案

---

## 解决方案文件格式

```markdown
---
id: S001
date: 2026-01-17
tags: [lua, nil, crash]
---

# 问题标题

## 问题
一句话描述问题现象

## 原因
一句话描述根本原因

## 解决
解决方案或代码片段

## 避坑
如何避免此问题
```
