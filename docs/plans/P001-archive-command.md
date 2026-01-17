---
id: P001
title: 归档指令 (Archive Command)
status: completed
created: 2026-01-14
---

# 归档指令 (Archive Command)

## 概述
创建一个新的 `/archive` 指令，用于在新功能开发完成后，自动将开发过程和使用说明制作成文档，存放到 `docs/module-instruction/` 目录下，以模块名字命名文件夹。

## 技术方案

### 关键实现步骤
1. 在 `.factory/commands/` 目录下创建 `archive.md` 指令文件
2. 指令接收模块名称作为参数
3. 自动收集以下信息生成文档：
   - 从最近的 plan 文件中提取功能概述
   - 从 git 提交历史中提取变更记录
   - 从代码注释中提取 API 说明
   - 生成使用示例

### 需要创建/修改的文件
- **新建**: `.factory/commands/archive.md` - 归档指令定义
- **新建**: `docs/module-instruction/` - 模块说明文档存放目录（按需创建）

### 文档输出结构
```
docs/module-instruction/
└── <模块名>/
    ├── README.md          # 模块概述和快速开始
    ├── api.md             # API 文档（如适用）
    ├── usage.md           # 使用说明和示例
    └── changelog.md       # 变更记录
```

## 验收标准
- [ ] `/archive <模块名>` 指令可正常执行
- [ ] 自动创建 `docs/module-instruction/<模块名>/` 目录结构
- [ ] 生成的文档包含功能概述、使用说明、API 文档
- [ ] 支持从 git 历史和 plan 文件中提取信息
- [ ] 文档格式规范，可读性强

## 实现任务
1. 创建 `.factory/commands/archive.md` 指令文件
2. 实现模块信息收集逻辑（读取 plan、git log）
3. 实现文档模板和生成逻辑
4. 测试指令执行流程

## 风险与注意事项
- 需要确保目标项目有 git 仓库，否则无法提取提交历史
- 模块名称需要规范化处理（去除特殊字符）
- 如果没有对应的 plan 文件，需要提示用户手动补充信息
- 注意用户提到的目录名是 `module-intrustion`（可能是拼写错误），建议使用 `module-instruction`
