---
name: session-solutions-loader
description: 新对话开始时加载历史解决方案索引。当新会话开始、用户提到"查看历史问题"、"之前解决过什么"、"历史方案"时使用。关键词：历史问题、历史方案、之前解决、解决过什么。
---

# 会话解决方案加载器

新对话开始时加载历史解决方案索引，帮助主代理了解之前解决过的问题。

## 触发条件

### 自动触发
- 新对话/会话开始

### 手动触发
- 用户说"查看历史问题"、"之前解决过什么"、"历史方案"
- 用户说"有哪些解决方案"、"知识库概览"

---

## Instructions

### Step 1: Check INDEX.md

Read `.factory/docs/solutions/INDEX.md`

### Step 2: Output Format

If INDEX.md exists and has content:

```markdown
## 历史解决方案概览

已记录 X 个解决方案:

| ID | 问题 | 日期 |
|----|------|------|
| [S001](./S001-xxx.md) | 问题描述 | 日期 |
| ... | ... | ... |

提示: 遇到类似问题时，可使用 solutions-lookup skill 查询详情。
```

If INDEX.md doesn't exist or is empty:

```markdown
## 历史解决方案概览

暂无历史解决方案记录。

提示: 解决问题后，使用 `/compound` 命令记录解决方案。
```

---

## Coordination

```
新对话开始 → [session-solutions-loader] 加载索引
    ↓
遇到问题 → [solutions-lookup] 查询详情
    ↓
解决问题 → /compound 记录方案
    ↓
下次对话 → [session-solutions-loader] 能看到新记录
```
