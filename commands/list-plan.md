列出项目中的所有计划。

## 说明

使用中文输出结果，但内部思考和分析过程使用英文。

## Instructions

1. Scan `.factory/docs/plans/` directory for all plan files (*.md)

2. For each plan file, read the frontmatter to extract:
   - `id`: Plan ID (e.g., P001)
   - `title`: Plan title
   - `status`: pending | in_progress | completed

3. Display the plans in a table format:

```
## 计划列表

| ID | 标题 | 状态 |
|----|------|------|
| P001 | xxx功能 | pending |
| P002 | yyy功能 | in_progress |
| P003 | zzz功能 | completed |

总计: X 个计划 (待处理: X, 进行中: X, 已完成: X)
```

4. If no plans found, display:
```
暂无计划。使用 `/plan <功能描述>` 创建新计划。
```

现在列出所有计划。
