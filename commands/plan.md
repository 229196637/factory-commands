为以下功能/任务创建详细的实现计划。

**功能:** $ARGUMENTS

## 说明

使用中文输出结果，但内部思考和分析过程使用英文。

## Instructions

1. First, research the current project structure using Read, Grep, and Glob tools to understand:
   - Project type and framework
   - Existing conventions and patterns
   - Related code that might be affected

2. Generate a simple plan ID (format: `P001`, `P002`, etc.) by checking existing plans in `.factory/docs/plans/`

3. Create a plan document at `.factory/docs/plans/<ID>-<feature-name>.md` with this structure:

```markdown
---
id: P001
title: [功能标题]
status: pending
created: [today's date]
---

# [功能标题]

## 概述
简要描述此功能的作用。

## 技术方案
- 关键实现步骤
- 需要创建/修改的文件
- 所需依赖

## 验收标准
- [ ] 标准 1
- [ ] 标准 2
- [ ] 标准 3

## 实现任务
1. 任务 1
2. 任务 2
3. 任务 3

## 风险与注意事项
- 需要注意的潜在问题
```

4. After creating the plan, ask the user if they want to:
   - 使用 `/work` 或 `/work true` 开始实现
   - 使用 `/review` 进行代码审查
   - 修改计划

现在分析项目并创建计划: $ARGUMENTS
