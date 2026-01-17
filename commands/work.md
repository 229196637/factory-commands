Execute the current plan by launching a code-writer agent.

## Parameters

Format: `/work [forceTest]`
- `true`: Force run unit tests, retry up to 3 times on failure
- `false` or omit: Run tests only if test framework detected

## Notes

Think and process in English. Output results in Chinese.

## Instructions

### Step 1: Read Plan
1. Find the latest pending plan in `.factory/docs/plans/` directory
2. Read the plan file and update its status to `in_progress`

### Step 2: Launch Code Writer Agent

Launch `code-writer` subagent to execute coding:

```
Task:
  subagent_type: code-writer
  description: Execute plan coding
  prompt: |
    Plan content: [full plan text]
    forceTest: [true/false]
    
    Implement the plan and return a change report including:
    - File path, change scope, content, purpose
    - Old vs new code comparison
```

### Step 3: Receive Change Report

After code-writer returns, collect the change report:
- List of modified files
- Change scope for each file
- Change content and purpose
- Old vs new code comparison

### Step 4: Unit Test (Optional)

| Parameter | Behavior |
|-----------|----------|
| `true` | Force execution, retry up to 3 times on failure |
| `false` or omit | Run only if test framework detected |

### Step 5: Update Plan Status
- Update plan status to `completed`

### Step 6: Output Summary

Output the change report from code-writer to user (in Chinese):

```markdown
## 执行完成

### 变更文件
| 文件 | 范围 | 作用 |
|------|------|------|

### 详细变更
[Old vs new comparison for each file]

### 测试结果
[If tests were run]
```

Now execute plan: $ARGUMENTS
