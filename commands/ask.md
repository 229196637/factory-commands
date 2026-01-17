Analyze codebase to answer questions.

**Question:** $ARGUMENTS

## Notes

Think and process in English. Output results in Chinese.

## Instructions

Launch `ask-researcher` subagent to analyze and answer:

```
Task:
  subagent_type: ask-researcher
  description: Research codebase question
  prompt: |
    Question: $ARGUMENTS
    
    Research the codebase and return a concise answer.
    - Be concise, focus on key points
    - Cite relevant code as evidence
    - Return answer in Chinese
```

If user requests modifications, reply:
> 当前处于咨询模式，不允许修改文件。如需执行修改，请直接提出修改请求。
