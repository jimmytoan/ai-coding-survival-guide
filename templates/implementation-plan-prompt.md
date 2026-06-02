# Implementation Plan Prompt

Use this before asking AI to write code.

```text
Create an implementation plan before writing code.

Goal:
[Describe the goal]

Background:
[Add short context]

Relevant files:
[List files if known]

Constraints:
[List constraints]

Output:
- Summary of the approach
- Files likely affected
- Data model changes
- API changes
- UI changes
- Test plan
- Risks
- Questions before implementation

Rules:
- Do not write code yet.
- Do not assume unrelated files can be changed.
- Call out any missing information.
- Prefer small, reviewable steps.
```
