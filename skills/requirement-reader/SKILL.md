---
name: requirement-reader
description: 读取项目根目录下docs/requirement/对应模块文件夹中的需求文档。当用户要求阅读指定模块需求、查看需求规则、了解功能规范时使用。关键词：需求、需求文档、规则、规范、功能说明。
---

# 需求文档阅读器

读取 `docs/requirement/<模块名>/` 目录下的需求文档，帮助理解模块的功能需求、业务规则和技术约束。

## 触发条件

- 用户说"阅读需求"、"查看需求"、"需求文档"
- 用户说"XX模块的需求"、"XX模块的规则"
- 用户说"功能规范"、"业务规则"、"技术约束"
- 开发新功能前需要了解需求规范
- 用户提到"需求"、"规则"、"规范"相关词汇

---

## Phase 1: Locate Requirement Documents

### 1.1 Confirm Directory Structure

First check if `docs/requirement/` directory exists under project root:

```
Use LS tool to view: <project-root>/docs/requirement/
```

### 1.2 Locate Module Folder

Based on user-specified module name, locate corresponding folder:

| User Input | Search Path |
|---------|---------|
| Module name is clear | `docs/requirement/<module-name>/` |
| Module name is vague | List all available modules for user to choose |
| Module not specified | List all available modules for user to choose |

### 1.3 List Document Files

Use Glob tool to find all documents in module folder:

```
Pattern: docs/requirement/<module-name>/**/*.md
Pattern: docs/requirement/<module-name>/**/*.txt
```

---

## Phase 2: Read Requirement Documents

### 2.1 Reading Priority

Read documents in the following priority order:

1. **README.md** - Module overview and entry document
2. **index.md** - Index document
3. **Other .md files** - In alphabetical order
4. **.txt files** - Supplementary notes

### 2.2 Extract Key Information

Focus on the following content when reading:

| Info Type | Focus Points |
|---------|--------|
| Functional Requirements | What features the module should implement |
| Business Rules | Business logic and constraints |
| Technical Constraints | Technical limitations, dependencies, interface definitions |
| Data Structures | Data formats, field descriptions |
| Process Descriptions | Operation flows, state transitions |
| Edge Cases | Exception handling, special scenarios |

### 2.3 Code Example Handling

If requirement documents contain code examples:
- Understand the purpose and context of the code
- Record API calling methods
- Note parameter and return value descriptions

---

## Phase 3: Output Report

### 3.1 Report Template

```markdown
## 需求阅读报告

### 1. 模块概述
- **模块名称**: [模块名]
- **文档路径**: [docs/requirement/模块名/]
- **文档数量**: [N个文件]

### 2. 功能需求
- [需求点1]
- [需求点2]
- ...

### 3. 业务规则
| 规则 | 说明 |
|------|------|
| 规则1 | 描述 |
| 规则2 | 描述 |

### 4. 技术约束
- [约束1]
- [约束2]

### 5. 关键接口/数据结构
[如有API或数据结构定义，列出关键信息]

### 6. 注意事项
- [边界情况1]
- [特殊处理1]
```

---

## Edge Case Handling

### Case 1: docs/requirement/ Directory Does Not Exist

```
提示: 项目根目录下未找到 docs/requirement/ 目录。
建议: 
1. 确认当前是否在正确的项目目录
2. 检查需求文档是否存放在其他位置
```

### Case 2: Specified Module Does Not Exist

```
提示: 未找到模块 [模块名] 的需求文档。
可用模块列表:
- 模块A
- 模块B
- ...
请选择一个模块或检查模块名称。
```

### Case 3: Module Folder Is Empty

```
提示: 模块 [模块名] 的需求文件夹为空，暂无需求文档。
```

### Case 4: Contains Subfolders

Recursively read all documents in subfolders and mark document hierarchy structure in report.

---

## Coordination with Other Skills

```
Read requirements → [requirement-reader] Understand requirements
    ↓
Code implementation → Develop features based on requirements
    ↓
Encounter problems → [legacy-code-bug-fixer] Fix Bug
    ↓
Need debugging → [debug-info-adder] Add debug info
```

---

## Usage Examples

### Example 1: Read Specific Module Requirements
```
User: Read scene loading module requirements
AI: 
1. Check docs/requirement/场景加载/ directory
2. Read all documents in directory
3. Output requirement report
```

### Example 2: View All Available Modules
```
User: What requirement documents are available
AI:
1. List all subfolders under docs/requirement/
2. Display available module list
```

### Example 3: Fuzzy Search
```
User: Read UI-related requirements
AI:
1. Search module folders containing "UI" keyword
2. List matching results for user to choose
```
