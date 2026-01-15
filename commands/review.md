Run a comprehensive code review on: $ARGUMENTS

## Instructions

1. First, identify the changes to review:
   - If a git ref is provided (e.g., HEAD~3..HEAD), run `git diff $ARGUMENTS`
   - If files are provided, read those files
   - If nothing provided, run `git diff --cached` for staged changes

2. Launch parallel reviews using the Task tool with these droids:
   - **security-sentinel**: Check for security vulnerabilities
   - **performance-oracle**: Check for performance issues
   - **code-simplicity-reviewer**: Check for unnecessary complexity

3. Synthesize all findings into a unified report:

```
# Code Review Summary

## Critical Issues (Must Fix)
- [CATEGORY] Issue description at file:line

## Recommendations (Should Fix)
- [CATEGORY] Suggestion

## Approved
- What looks good

## Verdict: APPROVED / CHANGES REQUESTED / NEEDS DISCUSSION
```

Now review: $ARGUMENTS
