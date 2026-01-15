使用双模型并行审查代码。

## 参数说明

格式: `/review-code [文件或目录]`
- 指定文件或目录进行审查
- 省略则审查当前变更 (git diff)

## Instructions

1. 确定审查范围:
   - 如果提供了路径，审查指定文件/目录
   - 如果未提供，使用 `git diff` 获取当前变更

2. **并行调用两个审查 droid** (使用 Task 工具同时启动):
   - `code-reviewer-gpt`: 审查代码逻辑、安全漏洞、性能问题
   - `code-reviewer-opus`: 审查架构设计、可维护性、错误处理

3. 收集审查结果并输出:
   ```markdown
   ## 代码审查报告
   
   ### GPT 审查 (逻辑/安全/性能)
   - Summary: ...
   - Issues: ...
   - Score: X/10
   
   ### Opus 审查 (架构/可维护性)
   - Summary: ...
   - Issues: ...
   - Score: X/10
   ```

4. 可选: 保存结果到 `.factory/docs/testing/`

现在审查: $ARGUMENTS
