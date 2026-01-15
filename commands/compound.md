记录最近解决的问题以供将来参考: $ARGUMENTS

## 说明

使用中文输出结果，但内部思考和分析过程使用英文。

## Instructions

1. Gather context from the conversation:
   - What was the problem/error?
   - What was tried that didn't work?
   - What was the root cause?
   - What fixed it?

2. Generate a simple ID (format: `S001`, `S002`, etc.) by checking existing solutions in `.factory/docs/solutions/`

3. Create a solution document at `.factory/docs/solutions/<ID>-<problem-name>.md`:

```markdown
---
id: S001
date: [today's date]
tags: [relevant, tags]
---

# [问题标题]

## 问题
[一句话描述问题现象]

## 原因
[一句话描述根本原因]

## 解决
[一句话或代码片段描述解决方案]

## 避坑
[一句话描述如何避免]
```

4. Confirm the documentation was created.

现在记录: $ARGUMENTS
