---
id: P003
title: 为 deepen-plan 命令添加文档 ID 支持
status: completed
created: 2026-01-15
---

# 为 deepen-plan 命令添加文档 ID 支持

## 概述
增强 `/deepen-plan` 命令，使其除了支持文件路径外，还能通过文档 ID（如 `P001`）快速定位计划文件。

## 技术方案

### 关键实现步骤
1. 修改 `deepen-plan.md` 的参数解析逻辑
2. 添加 ID 到路径的解析功能
3. 支持两种输入格式：
   - 路径格式：`/deepen-plan plans/my-feature.md`
   - ID 格式：`/deepen-plan P001`

### 需要修改的文件
- `~/.factory/commands/deepen-plan.md`

### 解析逻辑
```
输入参数 → 判断格式：
  - 如果匹配 /^P\d{3}$/（如 P001）→ 在 .factory/docs/plans/ 中查找匹配的文件
  - 如果是路径 → 直接使用
  - 如果为空 → 列出可用计划供选择
```

## 验收标准
- [ ] `/deepen-plan P001` 能正确找到并处理 `P001-*.md` 文件
- [ ] `/deepen-plan plans/xxx.md` 路径方式仍然正常工作
- [ ] 当 ID 不存在时，给出友好的错误提示
- [ ] 当有多个匹配时（理论上不应该），提示用户选择

## 实现任务
1. 在 `deepen-plan.md` 的 `## Plan File` 部分添加 ID 解析逻辑
2. 添加查找计划文件的指令
3. 更新 `argument-hint` 描述

## 风险与注意事项
- ID 格式需要与现有计划文件命名规范一致（`P001-xxx.md`）
- 需要处理 ID 不存在的情况
