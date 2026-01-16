系统地执行当前计划，支持并行Agent审查。

## 参数说明

格式: `/work [forceTest]`
- `true`: 强制运行单元测试，失败则修复重试(最多3次)
- `false` 或 省略: 视情况运行测试 (检测到测试框架时运行)

## 说明

使用中文输出结果，但内部思考和执行逻辑使用英文。

## Instructions

### Step 1: 读取计划
1. Find the latest pending plan in `.factory/docs/plans/` directory
2. Read the plan file and update its status to `in_progress`
3. Break down the work into actionable tasks using TodoWrite

### Step 2: 接口分析 (如涉及重构)
- 如果计划涉及模块重构或接口修改:
  - 调用 `api-analyzer` droid 分析相关模块
  - 获取接口清单作为编码参考
  - 结果不保存，直接传递给编码步骤

### Step 3: 编码执行
- 按计划逐步实现功能
- 遵循项目现有约定
- 使用 TodoWrite 更新任务进度

### Step 4: 单元测试

调用 `unit-test` skill:

| 参数 | 行为 |
|------|------|
| `true` | **强制执行**: 必须通过，失败则修复重试，最多3次 |
| `false` 或 省略 | **视情况**: 有 jest/pytest/vitest 等配置时运行 |

测试失败处理 (仅 `forceTest=true`):
```
测试失败 → 分析错误 → 修复代码 → 重新测试
    ↑                              │
    └──────── 最多重试3次 ─────────┘
    
3次仍失败 → 暂停并报告问题，等待用户决策
```

如果 `unit-test` skill 不存在 → 提示"缺失 unit-test 技能，请手动测试"

### Step 5: 保存结果
- 更新计划状态为 `completed`

### Step 6: 生成总结报告
```markdown
## 执行总结
- 修改文件: [文件列表]
- 测试结果: 通过 X / 失败 Y / 总计 Z (如运行了测试)
- 问题修复: [修复记录]
```

现在执行计划: $ARGUMENTS
