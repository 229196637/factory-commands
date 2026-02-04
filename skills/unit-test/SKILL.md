---
name: unit-test
description: 运行单元测试并验证代码质量。当用户实现新功能、修复Bug、修改代码后，或原项目出现Bug时自动触发此技能。关键词：测试、单元测试、unit test、运行测试、验证代码。
---

# 单元测试技能

## 概述

此技能用于在项目中设置和运行单元测试框架，独立于主程序运行以确保代码质量。

## 触发条件

- 用户说"测试"、"单元测试"、"unit test"
- 用户说"运行测试"、"验证代码"、"跑一下测试"
- 用户完成功能实现后
- 用户修复bug后
- 用户修改代码后
- 原项目有bug需要验证
- 用户明确要求运行测试

## Instructions

### 1. Detect Project Type and Test Framework

First identify project type and corresponding test framework:

| Project Type | Test Framework | Test Directory | Run Command |
|---------|---------|---------|---------|
| C# (.NET) | UnitTest Project | `UnitTest/` | `dotnet build && dotnet run --project UnitTest` |
| Python | pytest | `tests/` | `pytest tests/ -v` |
| Node.js | jest/mocha | `__tests__/` or `test/` | `npm test` |
| Lua | busted/luaunit | `tests/` | `D:\Tools\Lua\lua-5.4.2_Win64_bin\lua54.exe tests/run.lua` |

### 2. Set Up Test Environment (If Not Exists)

If project has no test directory, create standard test structure:

**C# Project:**
```
Project Root/
├── UnitTest/
│   ├── UnitTest.csproj
│   └── TestProgram.cs
└── TestOutput/
    └── .gitkeep
```

**Python Project:**
```
Project Root/
├── tests/
│   ├── __init__.py
│   └── test_main.py
└── pytest.ini
```

**Lua Project:**
```
Project Root/
├── tests/
│   └── run.lua
└── TestOutput/
    └── .gitkeep
```

### 3. Run Tests

Execute corresponding test command based on project type:

**C# Project:**
```bash
# Build
dotnet build <solution-file>

# Run tests
dotnet run --project <UnitTest-project-path>
```

**Python Project:**
```bash
pytest tests/ -v --tb=short
```

**Node.js Project:**
```bash
npm test
```

**Lua Project:**
```bash
D:\Tools\Lua\lua-5.4.2_Win64_bin\lua54.exe tests/run.lua
```

### 4. Record Test Output

Save test results to `TestOutput/` directory:
- File naming format: `test_YYYY-MM-DD_HHmmss.txt`
- Include complete test output and timestamp
- Use UTF-8 encoding

### 5. Visual Testing (Optional)

After unit tests pass, if user requests, run main program for visual verification:
- Verify UI displays correctly
- Verify feature behavior meets expectations
- Check log output

## Success Criteria

- [ ] Test framework is set up or already exists
- [ ] All unit tests pass
- [ ] Test output is recorded
- [ ] No compilation errors
- [ ] No runtime exceptions

## Failure Handling

### Compilation Failure
1. Analyze error message
2. Locate problem code
3. Provide fix suggestions
4. Re-run tests after fix

### Test Failure
1. Analyze failed test cases
2. Check related code logic
3. Provide fix solution
4. Re-verify after fix

### Runtime Error
1. Check log files
2. Analyze stack trace
3. Locate root cause
4. Provide solution

## Test Case Templates

### C# Test Template
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
        // [TC001] Test case name
        try
        {
            // Test logic
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

### Python Test Template
```python
import pytest

class TestMain:
    def test_example(self):
        """TC001: Test case description"""
        # Arrange
        expected = True
        
        # Act
        result = True
        
        # Assert
        assert result == expected
```

### Lua Test Template
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

## Environment Configuration

### Lua VM
- Path: `D:\Tools\Lua\lua-5.4.2_Win64_bin`
- Execute command: `D:\Tools\Lua\lua-5.4.2_Win64_bin\lua54.exe`

## Test Documentation Location
- Test folder record location: docs/测试文档

## Important Notes

1. **Independence**: Tests must run independently from main program
2. **Repeatability**: Test results must be repeatable
3. **Isolation**: Tests should not affect each other
4. **Recording**: All test results must be recorded to file
