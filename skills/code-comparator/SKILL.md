---
name: code-comparator
description: 跨项目代码对比分析。当用户需要对比不同项目的实现、研究问题解决方案、分析代码差异时使用。关键词：对比、比较、差异、vs、不同、两个项目。
---

# 代码对比技能

跨项目代码对比分析。用于对比不同项目的实现、研究问题解决方案、分析代码差异。

## 触发条件

- 用户说"对比"、"比较"、"差异"、"不同"
- 用户说"两个项目"、"A和B"、"vs"
- 用户说"怎么实现的"、"参考实现"、"最佳实践"
- 用户需要研究某功能在不同项目中的实现方式
- 用户需要分析代码相似点和差异点

## Workflow

### Phase 1: Collect Comparison Targets

**Must confirm the following information:**

1. **Project paths**: Absolute paths of projects to compare
2. **Comparison scope**: Specific feature/module/file
3. **Comparison dimensions**:
   - Structure comparison (file organization, module division)
   - Implementation comparison (algorithms, logic flow)
   - API comparison (interface design, parameter differences)
   - Pattern comparison (design patterns, code style)

**Example questions:**
```
Please provide the following information:
1. Project A path:
2. Project B path:
3. Feature/module to compare:
4. Comparison dimensions of interest:
```

### Phase 2: Code Reading

**Use tools to locate and read code:**

```
1. Glob - Locate related files
   - Search patterns: **/*.lua, **/*.py, **/*.js, etc.
   - Filter by feature module

2. Grep - Search key implementations
   - Search function names, class names, key logic
   - Locate core code positions

3. Read - Read code content
   - Read located files
   - Extract key code segments
```

### Phase 3: Comparison Analysis

**Analysis dimensions:**

| Dimension | Analysis Content |
|------|----------|
| Structure | File organization, directory structure, module division |
| Implementation | Algorithm selection, logic flow, data structures |
| API | Interface design, parameter definitions, return values |
| Pattern | Design patterns, code style, naming conventions |
| Dependencies | External libraries, internal module dependencies |

### Phase 4: Output Report

**Standard report format:**

```markdown
## 代码对比报告

### 对比目标
- **项目A**: [名称] - [路径]
- **项目B**: [名称] - [路径]
- **对比范围**: [功能/模块描述]
- **对比维度**: [结构/实现/API/模式]

### 相似点

| 方面 | 项目A | 项目B | 说明 |
|------|-------|-------|------|
| [方面1] | [实现] | [实现] | [相似原因] |

### 差异点

| 方面 | 项目A | 项目B | 分析 |
|------|-------|-------|------|
| [方面1] | [实现] | [实现] | [差异原因及影响] |

### 代码片段对比

**项目A - [文件路径]:**
\`\`\`[语言]
[代码片段]
\`\`\`

**项目B - [文件路径]:**
\`\`\`[语言]
[代码片段]
\`\`\`

### 总结与建议

1. **优势对比**: [哪个项目在哪方面更优]
2. **可借鉴点**: [可以相互学习的地方]
3. **改进建议**: [基于对比的改进方向]
```

## Edge Cases

### Different Programming Languages
- Note language differences
- Focus on logic and design comparison
- Ignore syntax-level differences

### File Does Not Exist
- Report missing files
- Continue comparing available parts
- Suggest user confirm paths

### Large Codebases
- Request user to specify specific module or file scope
- Compare in batches
- Prioritize comparing core features

### Version Differences
- Confirm if comparing different versions of same feature
- Note version information
- Focus on change history

## Examples

### Example 1: Compare Login Feature of Two Projects

```
User: Help me compare login feature implementation of Project A and Project B

AI:
1. Confirm project paths and login module locations
2. Use Glob to locate login-related files
3. Read and analyze both implementations
4. Output comparison report
```

### Example 2: Research Different Implementations of an API

```
User: I want to see how different projects implement file upload

AI:
1. Confirm projects to compare
2. Search file upload related code
3. Analyze implementation differences
4. Summarize best practices
```

## Important Notes

- Understand overall architecture of both projects before comparing
- Focus on core logic, ignore irrelevant details
- Stay objective, don't favor either side
- Provide actionable improvement suggestions
