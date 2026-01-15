列出项目中的所有已解决问题 (知识库)。

## 说明

使用中文输出结果，但内部思考和分析过程使用英文。

## Instructions

1. Scan `.factory/docs/solutions/` directory for all solution files (*.md)

2. For each solution file, read the frontmatter to extract:
   - `id`: Solution ID (e.g., S001)
   - `date`: Creation date
   - `tags`: Related tags

3. Also read the first heading as the title

4. Display the solutions in a table format:

```
## 知识库 (已解决问题)

| ID | 标题 | 日期 | 标签 |
|----|------|------|------|
| S001 | Lua nil崩溃 | 2026-01-14 | lua, nil |
| S002 | API超时 | 2026-01-13 | api, timeout |

总计: X 条记录
```

5. If no solutions found, display:
```
暂无记录。使用 `/compound` 记录已解决的问题。
```

现在列出所有已解决问题。
