## 分析总结

**compound-engineering-plugin** 是一个 Claude Code 插件，包含：
- **27 个 Agents** (代码审查、研究、设计、工作流等)
- **20 个 Commands** (斜杠命令)
- **13 个 Skills** (技能/知识库)
- **2 个 MCP Servers** (Playwright, Context7)

## 转换方案

### 1. Agents → Custom Droids

Claude Code Agent 格式:
```yaml
---
name: kieran-rails-reviewer
description: "Rails code review..."
model: inherit
---
[System Prompt Body]
```

Factory Droid 格式 (几乎相同):
```yaml
---
name: kieran-rails-reviewer
description: Rails code review with strict conventions
model: inherit
tools: ["Read", "Grep", "Glob", "LS"]
---
[System Prompt Body]
```

**转换要点:**
- `name`, `description`, `model` 字段直接保留
- 添加 `tools` 字段限制工具访问 (read-only 用于审查类)
- 系统提示词主体保持不变
- 文件存放位置: `~/.factory/droids/` (个人) 或 `.factory/droids/` (项目)

### 2. Commands → Custom Slash Commands

Claude Code Command 格式:
```yaml
---
name: workflows:plan
description: Transform feature descriptions into plans
argument-hint: "[feature description]"
---
[Command Instructions]
```

Factory Slash Command 格式:
```markdown
# /plan

Transform feature descriptions into well-structured project plans.

## Usage
/plan [feature description]

## Instructions
[Command Instructions]
```

**转换要点:**
- 存放位置: `~/.factory/commands/` 或 `.factory/commands/`
- 文件名即命令名 (如 `plan.md` → `/plan`)
- 可以是 Markdown 或可执行脚本

### 3. Skills → 无直接对应 (需拆分)

Claude Code Skills 是带有参考文件的知识库目录结构:
```
skills/compound-docs/
├── SKILL.md          # 主技能定义
└── references/       # 参考文档
    └── yaml-schema.md
```

**Factory 替代方案:**
- **复杂 Skills** → 转换为 Custom Droid + AGENTS.md 知识注入
- **简单 Skills** → 直接嵌入 Droid 的 system prompt
- **参考文档** → 放入项目 `docs/` 目录，让 Droid 按需读取

### 4. 推荐转换的核心组件

| 原组件 | 转换为 | 优先级 |
|--------|--------|--------|
| `kieran-rails-reviewer` | Droid | 高 |
| `code-simplicity-reviewer` | Droid | 高 |
| `security-sentinel` | Droid | 高 |
| `workflows:plan` | Slash Command | 高 |
| `workflows:review` | Slash Command | 高 |
| `compound-docs` | Droid + 文档 | 中 |
| `git-worktree` | Slash Command | 中 |

### 5. 实施步骤

1. **创建目录结构**
   ```
   ~/.factory/droids/          # 个人 Droids
   ~/.factory/commands/        # 个人 Commands
   ```

2. **转换 Review Agents** (14个) → 14个 Droid 文件

3. **转换核心 Commands** (4个 workflow 命令) → 4个 Slash Command 文件

4. **整合 Skills 知识** → 嵌入相关 Droid 或创建参考文档

## 示例转换

### kieran-rails-reviewer.md (Droid)
```yaml
---
name: kieran-rails-reviewer
description: Rails code review with strict conventions and high quality bar
model: inherit
tools: read-only
---

You are Kieran, a super senior Rails developer with impeccable taste...
[完整 system prompt]
```

### plan.md (Slash Command)
```markdown
# /plan

Transform feature descriptions into well-structured project plans.

## Usage
/plan [feature description]

## Process
1. Repository Research & Context Gathering
2. Issue Planning & Structure
3. SpecFlow Analysis
4. Choose Implementation Detail Level
5. Issue Creation & Formatting
6. Final Review & Submission
```

## 下一步

确认方案后，我将：
1. 创建 `~/.factory/droids/` 目录
2. 逐个转换最有价值的 Agents 为 Droid 文件
3. 创建核心 Slash Commands
4. 提供使用说明