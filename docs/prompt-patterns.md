# Prompt Patterns

Reusable prompt patterns for AI coding assistants.

## 1. Explain first

```text
Explain this code before suggesting changes.

Focus on:
- What it does
- Inputs and outputs
- Important side effects
- Edge cases
- Any unclear assumptions

Do not rewrite the code yet.
```

## 2. Plan before code

```text
Create an implementation plan before writing code.

Goal:
[Describe goal]

Relevant files:
[List files if known]

Constraints:
[List constraints]

Output:
- Files likely affected
- Proposed changes
- Data model changes
- API changes
- UI changes
- Test plan
- Risks
- Questions before implementation

Do not write code yet.
```

## 3. Scope-limited review

```text
Review only the following files:
[List files]

Goal:
[Describe goal]

Focus on:
- Logic errors
- Edge cases
- Security concerns
- Missing tests
- Duplicated or redundant code

Do not rewrite code unless necessary.
```

## 4. Ask before large context

```text
Before scanning many files or using the whole workspace as context, tell me:
- Which files you need
- Why you need them
- What you expect to find

Do not proceed until I approve.
```

## 5. Small implementation step

```text
Implement only this step:
[Describe step]

Scope:
- Only modify these files:
[List files]

Do not:
- Add new dependencies
- Change public API behavior
- Touch unrelated files
- Refactor unrelated code

After the change, summarize what changed and why.
```

## 6. Diff review

```text
Review this diff.

Focus on:
- Does it solve the stated problem?
- Is the code simple?
- Is there duplicated logic?
- Are there hidden behavior changes?
- Are tests sufficient?
- Are there security concerns?

Return:
- Critical issues
- Suggested improvements
- What is safe to leave as-is
```

## 7. Cleanup without over-engineering

```text
Suggest cleanup for this code.

Rules:
- Do not introduce unnecessary abstraction.
- Do not change behavior.
- Prefer small improvements.
- Identify duplicated or redundant code.
- Explain each suggestion briefly.
```

## 8. Test generation

```text
Generate tests for this function.

Focus on:
- Happy path
- Edge cases
- Invalid input
- Error handling

Do not change production code.
If behavior is unclear, ask questions first.
```

## Key takeaway

Good prompts reduce waste by narrowing the task.
