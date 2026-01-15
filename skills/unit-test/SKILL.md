---
name: unit-test
description: 运行单元测试并验证代码质量。当用户实现新功能、修复Bug、修改代码后，或原项目出现Bug时自动触发此技能。
---

# Unit Test Skill

## 概述

此技能用于在项目中设置和运行单元测试框架，独立于主程序运行，确保代码质量。

## 触发条件

- 用户完成功能实现后
- 用户修复 Bug 后
- 用户修改代码后
- 原项目出现 Bug 需要验证时
- 用户明确要求运行测试

## 指令

### 1. 检测项目类型和测试框架

首先识别项目类型和对应的测试框架：

| 项目类型 | 测试框架 | 测试目录 | 运行命令 |
|---------|---------|---------|---------|
| C# (.NET) | UnitTest 项目 | `UnitTest/` | `dotnet build && dotnet run --project UnitTest` |
| Python | pytest | `tests/` | `pytest tests/ -v` |
| Node.js | jest/mocha | `__tests__/` 或 `test/` | `npm test` |
| Lua | busted/luaunit | `tests/` | `D:\Tools\Lua\lua-5.4.2_Win64_bin\lua.exe tests/run.lua` |

### 2. 设置测试环境（如果不存在）

如果项目没有测试目录，创建标准测试结构：

**C# 项目:**
```
项目根目录/
├── UnitTest/
│   ├── UnitTest.csproj
│   └── TestProgram.cs
└── TestOutput/
    └── .gitkeep
```

**Python 项目:**
```
项目根目录/
├── tests/
│   ├── __init__.py
│   └── test_main.py
└── pytest.ini
```

**Lua 项目:**
```
项目根目录/
├── tests/
│   └── run.lua
└── TestOutput/
    └── .gitkeep
```

### 3. 运行测试

根据项目类型执行对应的测试命令：

**C# 项目:**
```bash
# 编译
dotnet build <解决方案文件>

# 运行测试
dotnet run --project <UnitTest项目路径>
```

**Python 项目:**
```bash
pytest tests/ -v --tb=short
```

**Node.js 项目:**
```bash
npm test
```

**Lua 项目:**
```bash
D:\Tools\Lua\lua-5.4.2_Win64_bin\lua.exe tests/run.lua
```

### 4. 记录测试输出

将测试结果保存到 `TestOutput/` 目录：
- 文件命名格式：`test_YYYY-MM-DD_HHmmss.txt`
- 包含完整的测试输出和时间戳
- 使用 UTF-8 编码

### 5. 可视化测试（可选）

单元测试通过后，如果用户要求，运行主程序进行可视化验证：
- 验证 UI 显示正常
- 验证功能行为符合预期
- 检查日志输出

## 成功标准

- [ ] 测试框架已设置或已存在
- [ ] 所有单元测试通过
- [ ] 测试输出已记录
- [ ] 无编译错误
- [ ] 无运行时异常

## 失败处理

### 编译失败
1. 分析错误信息
2. 定位问题代码
3. 提供修复建议
4. 修复后重新运行测试

### 测试失败
1. 分析失败的测试用例
2. 检查相关代码逻辑
3. 提供修复方案
4. 修复后重新验证

### 运行时错误
1. 检查日志文件
2. 分析堆栈跟踪
3. 定位问题根源
4. 提供解决方案

## 测试用例模板

### C# 测试模板
```csharp
namespace UnitTest;

public class TestProgram
{
    public static void Main(string[] args)
    {
        Console.WriteLine("=== Unit Test ===");
        Console.WriteLine($"Test started at: {DateTime.Now}");
        
        RunTests();
        
        Console.WriteLine("=== Test Completed ===");
    }

    private static void RunTests()
    {
        // [TC001] 测试用例名称
        try
        {
            // 测试逻辑
            Console.WriteLine("[TC001] Test Name");
            Console.WriteLine("  Result: PASS");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"  Result: FAIL - {ex.Message}");
        }
    }
}
```

### Python 测试模板
```python
import pytest

class TestMain:
    def test_example(self):
        """TC001: 测试用例描述"""
        # Arrange
        expected = True
        
        # Act
        result = True
        
        # Assert
        assert result == expected
```

### Lua 测试模板
```lua
-- tests/run.lua
local function test_example()
    print("[TC001] Test Example")
    local expected = true
    local result = true
    
    if result == expected then
        print("  Result: PASS")
        return true
    else
        print("  Result: FAIL")
        return false
    end
end

print("=== Unit Test ===")
print("Test started at: " .. os.date("%Y-%m-%d %H:%M:%S"))

local all_passed = true
all_passed = test_example() and all_passed

print("=== Test Completed ===")
if all_passed then
    print("All tests passed!")
else
    print("Some tests failed!")
    os.exit(1)
end
```

## 环境配置

### Lua 虚拟机
- 路径: `D:\Tools\Lua\lua-5.4.2_Win64_bin`
- 执行命令: `D:\Tools\Lua\lua-5.4.2_Win64_bin\lua.exe`

## 测试的文档生成位置
- 测试文件夹记录文件夹: docs/测试文档

## 注意事项

1. **独立性**: 测试必须独立于主程序运行
2. **可重复性**: 测试结果必须可重复
3. **隔离性**: 测试之间不应相互影响
4. **记录性**: 所有测试结果必须记录到文件
