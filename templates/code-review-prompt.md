# Code Review Prompt

Use this to review a diff or small set of files.

```text
Review this diff.

Context:
[Describe the intended change]

Focus on:
- Logic errors
- Edge cases
- Security concerns
- Missing tests
- Duplicated or redundant code
- Over-engineering
- Unexpected behavior changes

Do not rewrite the code unless necessary.

Return:
- Critical issues
- Suggested improvements
- What is safe to leave as-is
- Questions for the author
```
