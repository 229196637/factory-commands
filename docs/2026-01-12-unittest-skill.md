## 研究发现

### 从历史会话中发现的单元测试模式

| 项目 | 测试框架 | 工作流 |
|------|---------|--------|
| C# AITask | UnitTest 项目 | `dotnet build` → `dotnet run` → 记录输出 → 可视化测试 |
| 已有 skill | build-and-test (项目级) | 编译 → 单元测试 → 记录 → 运行主程序 |

---

## 创建方案

### 位置
`C:\Users\unclefata\.factory\skills\unit-test\SKILL.md` (个人级 skill，跨项目通用)

### Skill 内容

```yaml
---
name: unit-test
description: 运行单元测试并验证代码质量。当用户实现新功能、修复Bug、修改代码后，或原项目出现Bug时自动触发。
---
```

### 核心功能

1. **自动检测项目类型和测试框架**
   - C# → UnitTest 项目 (`dotnet run`)
   - Python → pytest (`pytest tests/`)
   - Node.js → jest/mocha (`npm test`)
   - Lua → busted/luaunit (`D:\Tools\Lua\lua-5.4.2_Win64_bin\lua.exe tests/run.lua`)

2. **设置测试环境** (如果不存在)
   - 创建 `UnitTest/` 目录
   - 创建 `TestOutput/` 目录
   - 生成测试模板文件

3. **运行测试并记录输出**
   - 执行测试命令
   - 保存结果到 `TestOutput/test_YYYY-MM-DD_HHmmss.txt`

4. **可视化测试** (可选)
   - 单元测试通过后运行主程序验证

### Lua 环境配置
- Lua 虚拟机路径: `D:\Tools\Lua\lua-5.4.2_Win64_bin`
- 执行命令: `D:\Tools\Lua\lua-5.4.2_Win64_bin\lua.exe`

### 触发条件
- 实现新功能后
- 修复 Bug 后
- 修改代码后
- 原项目出现 Bug 时

---

是否确认创建此 skill？