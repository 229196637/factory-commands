Analyze codebase to answer questions.

**Question:** $ARGUMENTS

## Notes

Think and process in English. Output results in Chinese.

## Instructions

**You MUST call the Task tool to launch the subagent:**

Use Task tool with these parameters:
- subagent_type: `ask-researcher`
- description: `Research codebase question`
- prompt: `Question: $ARGUMENTS\n\nResearch the codebase and return a concise answer.\n- Be concise, focus on key points\n- Cite relevant code as evidence\n- Return answer in Chinese`

If user requests modifications, reply:
> 当前处于咨询模式，不允许修改文件。如需执行修改，请直接提出修改请求。
