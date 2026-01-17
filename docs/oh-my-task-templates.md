# Oh-My-Task 模板文档

## 1. 任务模型 (Task Model)

### 主任务模型

```typescript
interface Task {
  // 基础信息
  id: string;                    // UUID v4
  name: string;                  // 任务名称
  description: string;           // 任务描述
  status: TaskStatus;            // 任务状态
  
  // 层级关系
  parent_id?: string;            // 父任务ID
  depth: number;                 // 当前层级深度 (1-N)
  task_depth: number;            // 总层级深度 (默认2)
  
  // 依赖关系
  dependencies: string[];        // 依赖的任务名称列表
  
  // 原子性标记
  atomic: boolean;               // 是否为原子任务
  testable: boolean;             // 是否可独立测试
  
  // 时间信息
  estimated_hours: number;       // 预估工时
  actual_hours?: number;         // 实际工时
  created_at: string;            // 创建时间 ISO8601
  updated_at: string;            // 更新时间 ISO8601
  started_at?: string;           // 开始时间
  completed_at?: string;         // 完成时间
  
  // 文档路径
  requirement_doc?: string;      // 需求文档路径
  development_doc?: string;      // 开发文档路径
  test_doc?: string;             // 测试文档路径
  
  // 执行信息
  assignee?: string;             // 执行者 (subagent id)
  priority: number;              // 优先级 (1最高)
  
  // 测试信息
  test_status?: TestStatus;      // 测试状态
  test_runs: TestRun[];          // 测试运行记录
}

type TaskStatus = 
  | 'created'           // 已创建
  | 'pending_approval'  // 待确认需求
  | 'approved'          // 需求已确认
  | 'planning'          // 规划中
  | 'ready'             // 就绪待执行
  | 'in_progress'       // 执行中
  | 'testing'           // 测试中
  | 'completed'         // 已完成
  | 'blocked'           // 被阻塞
  | 'failed';           // 失败

type TestStatus = 
  | 'pending'           // 待测试
  | 'running'           // 测试中
  | 'passed'            // 测试通过
  | 'failed'            // 测试失败
  | 'review_needed';    // 需要复查
```

### 测试运行记录模型

```typescript
interface TestRun {
  id: string;                    // 运行ID
  run_number: number;            // 第几次运行 (1, 2, ...)
  status: 'passed' | 'failed';   // 运行结果
  started_at: string;            // 开始时间
  finished_at: string;           // 结束时间
  duration_ms: number;           // 耗时(毫秒)
  
  // 测试结果
  total_tests: number;           // 总测试数
  passed_tests: number;          // 通过数
  failed_tests: number;          // 失败数
  skipped_tests: number;         // 跳过数
  
  // 失败详情
  failures: TestFailure[];       // 失败详情列表
  
  // 输出日志
  stdout: string;                // 标准输出
  stderr: string;                // 错误输出
}

interface TestFailure {
  test_name: string;             // 测试名称
  error_message: string;         // 错误信息
  stack_trace?: string;          // 堆栈跟踪
  expected?: string;             // 期望值
  actual?: string;               // 实际值
}
```

---

## 2. 任务信息查询返回模板

### HELP 返回模板 (获取命令帮助)

```json
{
  "success": true,
  "version": "1.0.0",
  "commands": [
    {
      "name": "HELP",
      "description": "获取命令帮助和使用示例",
      "params": [],
      "example": "HELP()"
    },
    {
      "name": "CREATE_TASK",
      "description": "创建新任务，返回需求ID用于后续操作",
      "params": [
        { "name": "title", "type": "string", "required": true, "description": "任务标题" },
        { "name": "description", "type": "string", "required": true, "description": "任务描述" },
        { "name": "task_depth", "type": "number", "required": false, "default": 2, "description": "任务层级深度" }
      ],
      "example": "CREATE_TASK(title=\"电商系统\", description=\"开发完整电商平台\", task_depth=3)"
    },
    {
      "name": "START_TASK",
      "description": "开始执行任务，返回预估时间和token",
      "params": [
        { "name": "task_id", "type": "string", "required": true, "description": "任务ID (leaf_id)" }
      ],
      "example": "START_TASK(task_id=\"leaf-001\")"
    },
    {
      "name": "FINISH_TASK",
      "description": "结束任务或强制停止",
      "params": [
        { "name": "requirement_id", "type": "string", "required": true, "description": "需求ID" },
        { "name": "force", "type": "boolean", "required": false, "default": false, "description": "是否强制停止" },
        { "name": "module_id", "type": "string", "required": false, "description": "指定停止的模块ID" }
      ],
      "examples": [
        "FINISH_TASK(requirement_id=\"req-001\")  # 正常结束",
        "FINISH_TASK(requirement_id=\"req-001\", force=true)  # 强制停止全部",
        "FINISH_TASK(requirement_id=\"req-001\", force=true, module_id=\"mod-001\")  # 停止指定模块"
      ]
    },
    {
      "name": "PAUSE_TASK",
      "description": "暂停任务，保存状态机位置",
      "params": [
        { "name": "requirement_id", "type": "string", "required": true, "description": "需求ID" }
      ],
      "example": "PAUSE_TASK(requirement_id=\"req-001\")"
    },
    {
      "name": "RESUME_TASK",
      "description": "继续执行已暂停的任务",
      "params": [
        { "name": "requirement_id", "type": "string", "required": true, "description": "需求ID" }
      ],
      "example": "RESUME_TASK(requirement_id=\"req-001\")"
    },
    {
      "name": "LIST_RUNNING_TASKS",
      "description": "列出所有运行中和暂停的任务状态机",
      "params": [],
      "example": "LIST_RUNNING_TASKS()"
    },
    {
      "name": "LIST_TASKS",
      "description": "列出任务详情，包含模块ID",
      "params": [
        { "name": "requirement_id", "type": "string", "required": false, "description": "筛选特定需求" },
        { "name": "status", "type": "string", "required": false, "description": "筛选状态" }
      ],
      "example": "LIST_TASKS(requirement_id=\"req-001\")"
    },
    {
      "name": "GET_TASK_OVERVIEW",
      "description": "查询任务编排和完成情况",
      "params": [
        { "name": "requirement_id", "type": "string", "required": true, "description": "需求ID" }
      ],
      "example": "GET_TASK_OVERVIEW(requirement_id=\"req-001\")"
    },
    {
      "name": "GET_TASK_CONTEXT",
      "description": "获取任务详细上下文",
      "params": [
        { "name": "task_id", "type": "string", "required": true, "description": "任务ID" }
      ],
      "example": "GET_TASK_CONTEXT(task_id=\"leaf-001\")"
    },
    {
      "name": "GET_NEXT_TASK",
      "description": "获取下一个待处理任务",
      "params": [
        { "name": "requirement_id", "type": "string", "required": false, "description": "需求ID" }
      ],
      "example": "GET_NEXT_TASK(requirement_id=\"req-001\")"
    },
    {
      "name": "CHECK_PHASE_GATE",
      "description": "检查阶段门禁状态",
      "params": [
        { "name": "phase_id", "type": "string", "required": true, "description": "阶段ID" }
      ],
      "example": "CHECK_PHASE_GATE(phase_id=\"phase-001\")"
    },
    {
      "name": "APPROVE_REQUIREMENT",
      "description": "确认需求文档，开始生成开发文档",
      "params": [
        { "name": "requirement_id", "type": "string", "required": true, "description": "需求ID" }
      ],
      "example": "APPROVE_REQUIREMENT(requirement_id=\"req-001\")"
    }
  ],
  "id_reference": {
    "requirement_id": "req-{uuid} - 需求ID，CREATE_TASK返回，用于暂停/继续/停止",
    "task_id": "task-main-{n} - 主任务ID",
    "module_id": "mod-{n} - 模块ID，LIST_TASKS返回",
    "phase_id": "phase-{n} - 阶段ID",
    "leaf_id": "leaf-{n} - 叶子任务ID"
  },
  "quick_start": [
    "1. CREATE_TASK(...) -> 获取 requirement_id",
    "2. APPROVE_REQUIREMENT(requirement_id) -> 确认需求",
    "3. LIST_RUNNING_TASKS() -> 查看运行状态",
    "4. GET_TASK_OVERVIEW(requirement_id) -> 查看进度",
    "5. PAUSE_TASK(requirement_id) -> 暂停",
    "6. RESUME_TASK(requirement_id) -> 继续",
    "7. FINISH_TASK(requirement_id, force=true) -> 强制停止"
  ]
}
```

### GET_TASK_CONTEXT 返回模板

```json
{
  "success": true,
  "task": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "登录功能",
    "description": "实现用户登录 API 接口",
    "status": "in_progress",
    "depth": 3,
    "task_depth": 3,
    "dependencies": [],
    "atomic": true,
    "testable": true,
    "estimated_hours": 2,
    "priority": 1,
    "created_at": "2026-01-12T10:00:00Z",
    "updated_at": "2026-01-12T11:30:00Z",
    "started_at": "2026-01-12T11:00:00Z",
    "development_doc": "docs/development/用户模块/phases/认证阶段/tasks/login.md",
    "test_doc": "docs/testing/用户模块/认证阶段/login-test.md",
    "test_status": "pending",
    "test_runs": []
  },
  "context": {
    "parent_chain": [
      { "id": "...", "name": "用户模块", "depth": 1 },
      { "id": "...", "name": "认证阶段", "depth": 2 }
    ],
    "siblings": [
      { "id": "...", "name": "注册功能", "status": "completed" },
      { "id": "...", "name": "密码重置", "status": "pending" }
    ],
    "blocking_tasks": [],
    "blocked_by_me": [
      { "id": "...", "name": "权限检查中间件" }
    ]
  },
  "documents": {
    "requirement": "# 登录功能需求\n...",
    "development": "# 任务描述\n实现用户登录 API 接口\n...",
    "test": null
  }
}
```

### LIST_TASKS 返回模板

```json
{
  "success": true,
  "tasks": [
    {
      "id": "...",
      "name": "登录功能",
      "status": "in_progress",
      "depth": 3,
      "dependencies": [],
      "test_status": "pending"
    },
    {
      "id": "...",
      "name": "注册功能",
      "status": "completed",
      "depth": 3,
      "dependencies": [],
      "test_status": "passed"
    }
  ],
  "summary": {
    "total": 5,
    "by_status": {
      "completed": 2,
      "in_progress": 1,
      "pending": 2
    },
    "by_test_status": {
      "passed": 2,
      "pending": 3
    }
  }
}
```

### GET_TASK_OVERVIEW 返回模板 (查询任务编排和完成情况)

```json
{
  "success": true,
  "task_id": "550e8400-e29b-41d4-a716-446655440000",
  "task_name": "电商系统开发",
  "overall_status": "in_progress",
  "overall_progress": {
    "total_modules": 4,
    "completed_modules": 1,
    "in_progress_modules": 1,
    "pending_modules": 2,
    "progress_percent": 25
  },
  "module_execution_order": [
    {
      "order": 1,
      "name": "用户模块",
      "dependencies": [],
      "can_start": true,
      "reason": "无依赖，可立即执行"
    },
    {
      "order": 2,
      "name": "商品模块",
      "dependencies": [],
      "can_start": true,
      "reason": "无依赖，可立即执行"
    },
    {
      "order": 3,
      "name": "订单模块",
      "dependencies": ["用户模块", "商品模块"],
      "can_start": false,
      "reason": "等待依赖: 用户模块(in_progress), 商品模块(pending)"
    },
    {
      "order": 4,
      "name": "支付模块",
      "dependencies": ["订单模块"],
      "can_start": false,
      "reason": "等待依赖: 订单模块(pending)"
    }
  ],
  "modules": [
    {
      "name": "用户模块",
      "status": "in_progress",
      "dependencies": [],
      "progress": {
        "total_tasks": 6,
        "completed": 4,
        "in_progress": 1,
        "pending": 1,
        "progress_percent": 66
      },
      "phases": [
        {
          "name": "认证阶段",
          "status": "in_progress",
          "tasks": [
            { "name": "登录功能", "status": "completed", "test_status": "passed" },
            { "name": "注册功能", "status": "completed", "test_status": "passed" },
            { "name": "密码重置", "status": "in_progress", "test_status": "pending" }
          ]
        },
        {
          "name": "权限阶段",
          "status": "pending",
          "tasks": [
            { "name": "角色管理", "status": "completed", "test_status": "passed" },
            { "name": "权限检查", "status": "completed", "test_status": "passed" },
            { "name": "访问控制", "status": "pending", "test_status": "pending" }
          ]
        }
      ],
      "test_summary": {
        "total_tests": 6,
        "passed": 4,
        "failed": 0,
        "pending": 2
      }
    }
  ],
  "dependency_graph": {
    "nodes": ["用户模块", "商品模块", "订单模块", "支付模块"],
    "edges": [
      { "from": "用户模块", "to": "订单模块" },
      { "from": "商品模块", "to": "订单模块" },
      { "from": "订单模块", "to": "支付模块" }
    ]
  },
  "timeline": {
    "created_at": "2026-01-12T09:00:00Z",
    "started_at": "2026-01-12T09:30:00Z",
    "estimated_completion": "2026-01-14T18:00:00Z",
    "total_estimated_hours": 64,
    "hours_spent": 8,
    "hours_remaining": 56
  }
}
```

### START_TASK 返回模板 (开始任务 - 返回预估时间和token)

```json
{
  "success": true,
  "task_id": "550e8400-e29b-41d4-a716-446655440000",
  "task_name": "登录功能",
  "started_at": "2026-01-12T11:00:00Z",
  "estimation": {
    "estimated_hours": 2,
    "estimated_tokens": {
      "input_tokens": 15000,
      "output_tokens": 8000,
      "total_tokens": 23000
    },
    "complexity": "medium",
    "confidence": 0.85
  },
  "task_info": {
    "description": "实现用户登录 API 接口",
    "acceptance_criteria": [
      "POST /api/login 返回 200 + token",
      "错误密码返回 401",
      "单元测试覆盖率 > 80%"
    ],
    "dependencies_satisfied": true,
    "blocking_tasks": []
  },
  "resources": {
    "related_files": [
      "src/auth/login.ts",
      "src/auth/__tests__/login.test.ts"
    ],
    "reference_docs": [
      "docs/requirements/用户模块/认证需求.md"
    ]
  },
  "message": "任务已开始，预计需要 2 小时，约 23000 tokens"
}
```

### FINISH_TASK 返回模板 (结束任务 - 返回测试和完成情况)

```json
{
  "success": true,
  "task_id": "550e8400-e29b-41d4-a716-446655440000",
  "task_name": "登录功能",
  "finished_at": "2026-01-12T13:15:00Z",
  "duration": {
    "started_at": "2026-01-12T11:00:00Z",
    "finished_at": "2026-01-12T13:15:00Z",
    "actual_hours": 2.25,
    "estimated_hours": 2,
    "variance": "+0.25h (12.5% over)"
  },
  "token_usage": {
    "input_tokens": 16200,
    "output_tokens": 8500,
    "total_tokens": 24700,
    "estimated_tokens": 23000,
    "variance": "+1700 (7.4% over)"
  },
  "completion_status": {
    "status": "completed",
    "all_criteria_met": true,
    "criteria_results": [
      { "criterion": "POST /api/login 返回 200 + token", "met": true },
      { "criterion": "错误密码返回 401", "met": true },
      { "criterion": "单元测试覆盖率 > 80%", "met": true, "actual": "85.2%" }
    ]
  },
  "test_results": {
    "overall_status": "passed",
    "total_runs": 2,
    "final_run": {
      "run_number": 2,
      "status": "passed",
      "total_tests": 6,
      "passed": 6,
      "failed": 0,
      "skipped": 0,
      "coverage": 85.2,
      "duration_ms": 1100
    },
    "test_history": [
      {
        "run_number": 1,
        "status": "failed",
        "passed": 4,
        "failed": 2,
        "issues_found": ["错误处理返回500而非具体状态码"]
      },
      {
        "run_number": 2,
        "status": "passed",
        "passed": 6,
        "failed": 0,
        "issues_fixed": ["添加错误类型判断，返回对应状态码"]
      }
    ],
    "logic_review": {
      "status": "passed",
      "checklist": [
        { "item": "测试用例覆盖所有验收标准", "passed": true },
        { "item": "边界条件已测试", "passed": true },
        { "item": "错误处理已测试", "passed": true },
        { "item": "无重复测试用例", "passed": true },
        { "item": "测试独立性", "passed": true }
      ]
    }
  },
  "files_changed": [
    { "path": "src/auth/login.ts", "action": "created", "lines": 85 },
    { "path": "src/auth/__tests__/login.test.ts", "action": "created", "lines": 120 },
    { "path": "src/auth/errors.ts", "action": "modified", "lines_changed": 15 }
  ],
  "gate_status": "passed",
  "message": "任务完成！耗时 2.25h，使用 24700 tokens，所有测试通过 (6/6)，覆盖率 85.2%"
}
```

### CHECK_PHASE_GATE 返回模板

```json
{
  "success": true,
  "can_proceed": false,
  "phase": {
    "id": "...",
    "name": "认证阶段",
    "total_tasks": 3,
    "completed_tasks": 2
  },
  "blocking_tests": [
    {
      "task_id": "...",
      "task_name": "登录功能",
      "test_status": "failed",
      "last_run": {
        "run_number": 2,
        "failed_tests": 1,
        "failures": [
          {
            "test_name": "should return 401 for invalid password",
            "error_message": "Expected 401 but got 500"
          }
        ]
      }
    }
  ],
  "passed_tests": [
    { "task_id": "...", "task_name": "注册功能", "test_status": "passed" }
  ],
  "message": "门禁检查失败: 1 个任务测试未通过"
}
```

---

## 3. 测试文档模板

### 测试文档结构 (test-doc-template.md)

```markdown
---
task_id: "550e8400-e29b-41d4-a716-446655440000"
task_name: "登录功能"
test_type: ["unit", "integration"]
created_at: "2026-01-12T11:00:00Z"
updated_at: "2026-01-12T11:30:00Z"
status: "pending"
total_runs: 0
last_run_status: null
---

# 测试文档: 登录功能

## 1. 测试概述

| 项目 | 值 |
|------|-----|
| 任务名称 | 登录功能 |
| 测试类型 | 单元测试, 集成测试 |
| 预期覆盖率 | > 80% |
| 测试文件 | `src/auth/__tests__/login.test.ts` |

## 2. 测试用例清单

### 2.1 单元测试

| ID | 用例名称 | 输入 | 期望输出 | 状态 |
|----|---------|------|---------|------|
| UT-001 | 有效凭证登录成功 | {username: "test", password: "123456"} | {token: "...", expires_in: 3600} | pending |
| UT-002 | 无效密码返回401 | {username: "test", password: "wrong"} | 401 Unauthorized | pending |
| UT-003 | 用户不存在返回404 | {username: "nouser", password: "123"} | 404 Not Found | pending |
| UT-004 | 缺少参数返回400 | {username: "test"} | 400 Bad Request | pending |

### 2.2 集成测试

| ID | 用例名称 | 描述 | 状态 |
|----|---------|------|------|
| IT-001 | 完整登录流程 | 从请求到数据库验证到返回token | pending |
| IT-002 | 并发登录测试 | 10个并发登录请求 | pending |

## 3. 测试运行记录

### 第 1 次运行

> 状态: 未执行

```
// 测试输出将在运行后填充
```

### 第 2 次运行

> 状态: 未执行

```
// 测试输出将在运行后填充
```

## 4. 问题与修复记录

| 运行次数 | 问题描述 | 修复方案 | 修复状态 |
|---------|---------|---------|---------|
| - | - | - | - |

## 5. 逻辑审查

### 5.1 审查清单

- [ ] 测试用例覆盖所有验收标准
- [ ] 边界条件已测试
- [ ] 错误处理已测试
- [ ] 无重复测试用例
- [ ] 测试独立性 (无相互依赖)

### 5.2 审查结论

> 待审查

## 6. 最终结论

| 项目 | 结果 |
|------|------|
| 测试通过 | 待定 |
| 覆盖率 | 待定 |
| 门禁状态 | 待定 |
```

### 测试运行后更新模板

```markdown
### 第 1 次运行

> 状态: ❌ 失败
> 时间: 2026-01-12T11:15:00Z
> 耗时: 1.2s

**结果统计:**
- 总测试数: 6
- 通过: 4
- 失败: 2
- 跳过: 0

**失败详情:**

```
FAIL src/auth/__tests__/login.test.ts
  ✕ should return 401 for invalid password (15ms)
    Expected: 401
    Received: 500
    
    at Object.<anonymous> (src/auth/__tests__/login.test.ts:25:5)

  ✕ should return 404 for non-existent user (8ms)
    Expected: 404
    Received: 500
```

**问题分析:**
1. 错误处理逻辑未正确实现，所有错误都返回500
2. 需要区分不同错误类型并返回对应状态码

---

### 第 2 次运行

> 状态: ✅ 通过
> 时间: 2026-01-12T11:25:00Z
> 耗时: 1.1s

**结果统计:**
- 总测试数: 6
- 通过: 6
- 失败: 0
- 跳过: 0

```
PASS src/auth/__tests__/login.test.ts
  ✓ should login successfully with valid credentials (12ms)
  ✓ should return 401 for invalid password (8ms)
  ✓ should return 404 for non-existent user (6ms)
  ✓ should return 400 for missing parameters (5ms)
  ✓ complete login flow (45ms)
  ✓ concurrent login requests (120ms)

Test Suites: 1 passed, 1 total
Tests:       6 passed, 6 total
Coverage:    85.2%
```
```

---

## 4. 模板使用说明

### 创建新任务时
1. 复制任务模型，填充必要字段
2. `atomic` 和 `testable` 根据任务性质设置
3. `dependencies` 只填写直接依赖

### 查询任务时
1. MCP 返回符合返回模板的 JSON
2. 包含完整上下文信息
3. 包含相关文档内容

### 生成测试文档时
1. 基于测试文档模板创建
2. 根据任务的验收标准生成测试用例
3. 每次运行后更新运行记录

### 测试流程
1. 第1次运行 → 记录结果
2. 修复问题 → 更新问题记录
3. 第2次运行 → 记录结果
4. 逻辑审查 → 检查测试完整性
5. 门禁判定 → 通过/失败
