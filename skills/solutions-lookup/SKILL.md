---
name: solutions-lookup
description: 查询项目历史解决方案和踩坑记录。当用户提到"之前修复过"、"之前动过"、"以前遇到过"、"踩过坑"、"历史问题"等关键词时使用。关键词：之前、以前、历史、踩坑、解决方案、类似问题。
---

# 解决方案查询

查询 `.factory/docs/solutions/` 目录中的历史解决方案，帮助避免重复踩坑。

## 触发条件

- 用户说"之前修复过"、"之前动过"、"以前遇到过"
- 用户说"踩过坑"、"历史问题"、"之前的解决方案"
- 用户说"有没有类似问题"、"查一下知识库"
- 用户说"以前怎么解决的"、"历史记录"
- 遇到问题时想查看是否有历史记录

---

## Phase 1: Scan Solutions Directory

### 1.1 Check Directory

```
Use LS tool to view: <project-root>/.factory/docs/solutions/
```

### 1.2 List All Solutions

Use Glob tool to find all solution files:
```
Pattern: .factory/docs/solutions/**/*.md
```

---

## Phase 2: Search and Match

### 2.1 Keyword Search

Based on user's problem description, use Grep to search matching solutions:

| Search Scope | Description |
|---------|------|
| Title | Short description of the problem |
| tags | Tags in YAML frontmatter |
| Problem description | Problem symptoms |
| Cause | Root cause |

### 2.2 Read Matching Files

For matching files, use Read tool to read complete content.

---

## Phase 3: Output Report

### 3.1 When Match Found

```markdown
## 找到相关解决方案

### [S001] 问题标题
- **问题**: 一句话描述
- **原因**: 根本原因
- **解决**: 解决方案
- **避坑**: 如何避免

---

### [S002] 另一个问题
...
```

### 3.2 When No Match Found

```markdown
## 未找到相关解决方案

知识库中暂无匹配的历史记录。

**建议**:
1. 尝试使用不同的关键词搜索
2. 解决问题后，使用 `/compound` 命令记录此解决方案
```

---

## Edge Case Handling

### Case 1: solutions Directory Does Not Exist

```
提示: .factory/docs/solutions/ 目录不存在。
建议: 使用 `/compound` 命令记录第一个解决方案，目录会自动创建。
```

### Case 2: Directory Is Empty

```
提示: 知识库为空，暂无历史解决方案。
建议: 解决问题后，使用 `/compound` 命令记录。
```

---

## Coordination with Other Commands

```
Encounter problem → [solutions-lookup] Query history
    ↓
Not found → Solve problem
    ↓
After solving → /compound Record solution
    ↓
Next time → [solutions-lookup] Can find it
```

---

## Usage Examples

### Example 1: Query Specific Problem
```
User: Have we encountered this nil crash before?
AI:
1. Search files containing "nil" in solutions directory
2. Display matching solutions
```

### Example 2: Query by Tag
```
User: Any pitfalls related to UI?
AI:
1. Search solutions with tags containing "ui"
2. List all matches
```

### Example 3: Fuzzy Query
```
User: There was an issue with animation system before
AI:
1. Search solutions containing "animation" or "动画"
2. Display related records
```
