Analyze codebase to answer questions.

## Notes

Think and process in English. Output results in Chinese.

## Instructions

**You MUST call the Task tool to launch the subagent.**

Read the user's question from $ARGUMENTS, then call Task tool:
- subagent_type: `ask-researcher`
- description: `Research codebase question`
- prompt: Pass the ACTUAL user question (from $ARGUMENTS) to the subagent. Tell it to research the codebase and return a concise answer in Chinese.

**IMPORTANT**: Do NOT pass "$ARGUMENTS" literally. Pass the actual question content.

If user requests modifications, reply:
> 当前处于咨询模式，不允许修改文件。如需执行修改，请直接提出修改请求。
